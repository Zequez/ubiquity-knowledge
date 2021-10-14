# Debt tokens

The ecosystem debt tokens are:

- [[uBOND]]
- [[uAR]]
- [[uDEBT]]

## Redeeming Conditions

- All debt tokens can only be redeemed for [[uAD]] when TWAP > 1
- Collateralization must be 1:20 (configurable)
- Must be enough redemption credit for [[uBOND]], [[uAR]] and [[uDEBT]] in that order of priority


## Debt Cycle Minting Premium

The later on the debt cycle, the more premium we offer when minting uAR or uDEBT by burning uAD.

- Since the length of the debt cycles are not known beforehand; this premium should be derived from a constantly incrementing value rather than a fractional. And the rate should be configurable. Correct?
	- Based on blocks that passed since the debt cycle started?
	- Based on the burned uAD since the debt cycle started?

**What's the math for this?**

## Redeeming Priority

If there are [[uAD]] available for minting to bring the [[TWAP Oracle]] back to 1 then the debts are honoured in the following order of priority:

- [[uBOND]]
- [[uAR]]
- [[uDEBT]]

## Debt Cycle Burning Estimate

At any point on the debt cycle we need to know how many [[uAD]] we would need to burn for the price to go back to 1. Based on this we allow redeeming [[uBOND]], [[uAR]], and [[uDEBT]] based on the market cap of those tokens. 

- Is this number a certainty or just a good prediction?
- What happens if there are people with [[uBOND]] or [[uAR]] that never redeem? Would the price be locked above 1?
- Should the collateral be considered in the formula?

**What is the math for this?**

## Collateralization

Minting [[uAD]] by burning debt tokens should not be allowed unless there is at least 1:20 collateralization. The collateralization should be configurable.