# Payout Notes

After understanding the basic market flow, I wanted to follow the money.

The prediction result is interesting, but from a user perspective the more
important question is:

"If I win, how do I actually receive my funds?"

## Pull Based Payout

The workshop uses a pull-based payout model.

Instead of the contract automatically sending money to every winner during
resolution, a winner can claim the amount owed to them.

I initially wondered why this was necessary.

Sending money to everyone immediately sounds simpler.

## Why Pull?

A prediction market can have many participants.

Resolving the market and paying every participant in one transaction would
make the resolution operation more expensive as the number of participants
grows.

With a pull model:

resolution
→ market becomes resolved

Then:

winner
→ calls claim
→ receives payout

This separates resolution from distribution.

## Basic Formula

The payout is based on the user's share of the winning pool.

Conceptually:

user stake
×
total pool
÷
winning pool

This means winners receive their proportional share of the total pool.

## Simple Example

Suppose:

YES pool = 60
NO pool = 40

Total pool = 100

If YES wins and Alice has 10 YES tokens:

10 × 100 ÷ 60

Alice receives approximately 16.66 units, subject to integer arithmetic.

## Why This Is Proportional

Alice owns:

10 / 60

of the winning pool.

So she receives the same fraction of the total pool.

## What About Losers?

If YES wins, NO participants do not receive normal winnings.

Their stake has been used as part of the total pool available to the winners.

## Claim

A winner does not automatically receive funds merely because the market is
resolved.

The user needs to claim.

This is an important distinction between:

market result

and

user payout

## Double Claim

The contract needs to make sure a user cannot claim the same payout twice.

This means claim state is important.

## Invalid Market

An Invalid market is different.

If the market could not be resolved because the external data was unavailable,
there is no normal winning side.

The Invalid path therefore uses refunds instead of normal winner payouts.

## Rounding

The payout formula uses integer arithmetic.

This means there can be rounding effects.

For example, a mathematical result might contain decimals while the contract
must use an integer representation.

The remaining small amount is therefore something worth understanding.

## Why I Wanted to Trace This

I originally thought the workshop was mostly about getting external data into
the contract.

Following the payout path showed me that the application also needs careful
state management after resolution.

The oracle tells us what happened.

The payout logic determines what happens to the money.
