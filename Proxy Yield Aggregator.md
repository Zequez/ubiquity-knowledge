# Proxy Yield Aggregator

## Farm on Pickle Jar
### USDC
The main deposit is done in USDC

### Pickle Jar
Right now we are using a single USDC Pickle Jar that has a specific APY in USDC.

We keep the USDC yield, and to the depositer we give it back as uAR

## Deposit Fee
A percentage of the main deposit is converted to uAR

### UBQ Staking
You can stake up to 10,000 UBQ to reduce the deposit fee to 0%

## Yield Boosting
We multiply the yield by 50% more. 

### uAD Staking
By staking up to 50% of the main deposit as uAD, you get up to another 50% multiplier on the yield.

## uAR Reward Multiplier
The uAR token is perpetually inflationary. So we need to multiply it by the deprecation rate at the block number we are minting them. So if we want to reward the user with the uAR-equivalent of 1000 uAD, we would do `uarRateAtBlockNumber(currentBlockNumber) * 1000`