# Payout Examples

I used small numbers here because they make the payout calculation easier to
reason about.

## Example 1: Simple YES Win

YES pool = 60

NO pool = 40

Total pool = 100

Alice stakes 10 on YES.

Alice's share of the winning pool is:

10 / 60

Her proportional payout is:

10 × 100 / 60

The result is approximately:

16.66

The actual contract uses integer arithmetic.

## Example 2: Larger Winner

YES pool = 80

NO pool = 20

Total pool = 100

Alice stakes 40 on YES.

Her share is:

40 / 80

So she owns half of the winning pool.

Her payout is:

40 × 100 / 80

= 50

## Example 3: Small Winner

YES pool = 90

NO pool = 10

Alice stakes 1 on YES.

Her payout is:

1 × 100 / 90

The mathematical result contains a fraction.

The contract cannot represent arbitrary decimal precision in normal integer
arithmetic.

This is why rounding needs to be considered.

## Example 4: Several Winners

YES pool = 70

NO pool = 30

Alice = 20 YES

Bob = 30 YES

Carol = 20 YES

Total YES = 70.

Alice owns:

20 / 70

Bob owns:

30 / 70

Carol owns:

20 / 70

Their payouts therefore add up to the available pool subject to integer
rounding.

## Example 5: Only One Side Has Bets

YES pool = 100

NO pool = 0

If YES wins, the winning pool is 100.

The formula still has a valid denominator.

The situation becomes more interesting when the winning pool is zero.

## Example 6: Winning Pool Is Zero

YES pool = 0

NO pool = 100

If the system somehow determined that YES was the winning side, there would be
no YES stake to distribute against.

The normal proportional payout formula cannot work because:

winning pool = 0

This is one reason I wanted to understand the invalid and settlement checks
before thinking only about the happy path.

## Example 7: Invalid Market

If the market becomes Invalid because resolution failed, the normal winner
formula should not be used.

Instead, users follow the refund path.

## What These Examples Taught Me

The payout formula looks simple:

stake × totalPool / winningPool

But the important part is the state around the formula.

Before calculating a payout, the contract needs to know:

- Is the market resolved?
- Is it valid?
- Is there a winning side?
- Is the user entitled to claim?
- Has the user already claimed?

That was more interesting to me than the formula itself.
