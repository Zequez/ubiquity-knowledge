# DebtCouponManager

## Current features
- Mints claimable dollars to get the price back to 1
- Exchange uAD for uAR 1-1 + premium when TWAP < 1
	-  Premium is calculated by UARForDollarsCalculator
- Exhange uAD for uDEBT + premium when TWAP < 1
	- Premium is calculated by CouponsForDollarsCalculator
- Exchange uAR for uAD 1-1 when TWAP > 1
- Exchange uDEBT for uAD 1-1 when TWAP > 1 and there are more uAD available than uAR
- Exchange expired uDEBT for UBQ at a configurable rate
- Exchange un-expired uDEBT for uAR at same value

## Considerations
- At the moment will redeem your uAR fo uAD at 1-1; so if we implement a `UarDeprecationClock` contract and use it on the [[Proxy Yield Aggregator]], we'll need to re-deploy this one to also use it. However, while we keep the inflation at 0% nothing will change.

## Changes needed
- Take into account he uBOND token, prioritize it over uAR and uDEBT
- Divide the uAD burned for uAR by the `uarRateAtBlockNumber`
- Multiply the uAR burned for uAD by the `uarRateAtBlockNumber`
- The exchange of uDEBT for uAR must be decreased as uDEBT gets closer to expiration

So we need a new contract `UarDeprecationClock`