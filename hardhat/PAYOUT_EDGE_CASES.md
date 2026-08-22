# Payout Edge Cases

I wrote down the cases that I think are easiest to overlook when looking only
at the normal winning scenario.

## Case 1: Normal Winner

Market is resolved to YES.

User has YES stake.

User can claim the proportional payout.

## Case 2: Losing User

Market is resolved to YES.

User has NO stake.

The user does not receive the normal winner payout.

## Case 3: Invalid Market

The market cannot be resolved successfully.

There is no YES or NO winner.

Users follow the refund path.

## Case 4: Double Claim

A user claims successfully.

The same user calls claim again.

The contract must prevent the same entitlement from being paid twice.

## Case 5: Claim Before Resolution

The market has not reached a final state.

There is no valid payout yet.

## Case 6: Zero Winning Pool

The winning side has no stake.

The proportional formula cannot divide by zero.

This is a state that needs to be handled by the market logic rather than
treated as a normal payout.

## Case 7: Small Stake

The user's stake is very small compared with the winning pool.

Integer division can make the payout differ slightly from a mathematical
decimal result.

## Case 8: Large Winning Pool

A large winning pool reduces the proportional share of each individual
winner.

The formula still works because the payout is based on the user's fraction of
the winning pool.

## Case 9: Only One Side Has Bets

If the only side with bets wins, the entire pool belongs to that winning side.

If the opposite side somehow becomes the winning side, the winning pool would
be zero and normal proportional payout would not be possible.

## Case 10: Many Winners

The contract should not need to loop over every winner during resolution.

Each user can calculate and claim their own entitlement.

## Main Observation

The pull-based design moves some complexity from the resolution transaction
to individual claim transactions.

I think this is a reasonable tradeoff because the number of users does not
need to determine how expensive the resolution transaction becomes.
