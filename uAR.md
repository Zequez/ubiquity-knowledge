# uAR
## Characteristics
- Fungible
- Perpetually inflationary token

## Minting
- When [[TWAP Oracle]] < 1 you can burn [[uAD]] and get uAD-equivalent-uAR plus a premium based on the debt cycle status
	- Right now the DebtCouponManager has a `exchangeDollarsForUAR` that does this, calling the `UARForDollarsCalculator.getUARAmount` contract to get the amount. However, the calculation there is not up to spec. It uses a `blockHeightDebt` that is set to the current block number when the debt cycle starts.
- On the Proxy Yield Aggregator you get all the rewards in uAD-equivalent-uAR

## Burning
- When [[TWAP Oracle]] > 1 you can burn uAR and get [[uAD]] based on `uarRateAtBlockNumber(block) * uAR = uAD`

## .RMap (Map of Inflation Rate Per Block)

https://docs.google.com/document/d/12gGDjRw4s1TRH4mT9v3yb_TFnFDKfzKgTjTfIKtoAMM/edit

> The depreciation rate of uAD is defined in such a way, that at the time of uDEBT expiration, UAR is worth 70% (configurable via setting the rate - R) of its original value.

`R = configurable inflation rate`
	If we want uAR to be worth 70% of it's original value after 30 days
	`(1+R)^(blockCountIn30Days) = 1/0.7 = 1/valueAfter30Days`
	How many blocks are there in 30 days? We can use the configurable `blockCountInAWeek` from the BondingContract
	`30 days block count = (30 / 7) * blockCountInAWeek`
	`R = (1/valueAfter30Days)^(1/blockCountIn30Days)-1`

On the contract, we set R directly, so we don't need to do that math.
R should be configurable for any given period; so we have a map of R based on block numbers. 

We can model RMap as an array of touples that we can push new rates to:

`RMap: (blockNumber, rate)[]`

**Example:**

Block 0 => R = 0
Block 1000 => R = 0.001
Block 3000 => R = 0.005

Then:

1 uAR @ B0 = 1 uAD
1 uAR @ B1000 = 1 uAD
1 uAR @ B1300 = 1 / ((1 + 0.001) ^ 300) = 0.74 uAD
1 uAR @ B3200 = 1 / ((1 + 0.001)^2000) * ((1 + 0.005)^200) = 0.37 uAD

So if we redeem 1 uAR @ B3200 we get 0.37 uAD

If at B1300 we wanted to mint 1000 uAD equivalent uAR we would just use the same number:

1000 uAD @ B1300 = (1/0.74) * 1000 = 1350 uAR

## .uarRateAtBlockNumber(blockNumber)

Using the R-map we can calculate the price of 1 uAR at any block number.

For example, let's imagine that R = 0.0001 since block 120000:

**Example: burning 1000 uAR for uAD at certain block:**

uarRateAtBlockNumber(123700) * 1000 = 691 uAD

**Example: minting the equivalent of 1000 USD in uAR at certain block**

(1 / uarRateAtBlockNumber(133000)) * 1000 = 3669 uAR

**Example: calculating the amount of uAR inflation between 2 block numbers**

uarRateAtBlockNumber(100000) = 1
uarRateAtBlockNumber(123000) = 0.74
1 / 0.74 = 1.351351351 => 35% inflation

## .setRateAtCurrentBlock(blockNumber)

We don't want to allow ourselves to change the inflation rate for past blocks. This function should push the current block and rate to the R-map.