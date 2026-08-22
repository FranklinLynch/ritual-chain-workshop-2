# Questions About Claims

I followed the claim path separately because I wanted to understand what
happens after the market is resolved.

## 1. Why does the user claim?

The contract could theoretically send funds during resolution.

The workshop instead uses a pull-based model.

This separates settlement from distribution.

## 2. Can everyone claim?

No.

The normal payout path is for participants on the winning side.

Invalid markets use the refund path.

## 3. Can a loser claim?

A losing stake is not a winning payout.

The contract needs to distinguish the winning side from the losing side.

## 4. Can a winner claim twice?

This should not be possible.

The claim operation needs state that prevents the same stake from being paid
again.

## 5. What happens before resolution?

A user should not be able to claim a result that does not exist yet.

The market needs to be in the correct resolved state.

## 6. What happens if the market is Invalid?

There is no normal winning side.

The user follows the refund path instead.

## 7. What happens if nobody bet on the winning side?

This is an important edge case.

The payout formula divides by the winning pool.

If the winning pool is zero, the normal formula does not make sense.

## 8. Why not store the payout amount when the market resolves?

A pull-based design can calculate the amount when the user claims.

This avoids having to iterate through all users during resolution.

## 9. What happens to rounding?

The contract uses integer arithmetic.

A mathematical payout can therefore contain a fractional remainder that cannot
be represented directly.

## 10. What I Learned

I originally thought "resolve market" and "pay winners" were basically one
operation.

They are actually two different parts of the application.

Resolution determines the result.

Claiming determines what an individual user can withdraw from that result.

That separation makes the design easier to scale conceptually.
