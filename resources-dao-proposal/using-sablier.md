---
title: Proposal using Sablier.com
description: 
published: true
date: 2025-01-03T12:38:46.398Z
tags: 
editor: markdown
dateCreated: 2025-01-03T12:38:46.398Z
---

# Creating a Sablier stream
The objective of this article is twofold:
- Any voting proposal (on tally) must be analyzed in detail, especially the part concerning the code that will be executed.
The end of the article will help to understand the code of the proposal: Liberation of REGs from the DAO Team RealT budget.
- In the future, Proposers may wish to use this example to build new proposals on Tally.
The article begins with the presentation of the parameters (on Tally and Sablier) to create a Sablier stream.

## Resources

- site : https://sablier.com/
- docs : https://docs.sablier.com/
- Vesting docs : https://docs.sablier.com/apps/features/vesting
- Courbe docs : https://docs.sablier.com/concepts/lockup/stream-shapes#lockup-linear
- Deployed contract docs : https://docs.sablier.com/guides/lockup/deployments#mainnets
- Sepolia example : https://sepolia.etherscan.io/tx/0x86239fed5f924b61c7f5656e0e9322d7809097f2c2153b478fb38c07f85bdb4d
- tally net test example : https://www.tally.xyz/gov/reg-dao-beta/proposal/6332954951702383415567855302130620076860949854692323526582374508844353542470

## Instructions common to all types of streams on Tally

1. Create a new proposal
1. Complete the proposal description form
1. Add an Action
- custom action
- Contract of the token sent
- Use the imported ABI (if successful, otherwise the abi must be provided)
- Contract Method -> approve
1. Add an Action
- custom action
- Contract Sablier SablierV2BatchLockup ([deployment docs](https://docs.sablier.com/guides/lockup/deployments))
- Use the imported ABI (if successful, otherwise the abi no you have to provide the abi)
- Contract Method -> [Select the method that is suitable] (preferably the one with timestamps are easier to configure)
- Calldatas -> [Fill in the parameters] the batch part can be complicated to complete, especially the elements of type tulpe[]
1. Perform the simulation to verify that everything is good

## Types of Hourglass stream

There are several types of streams that give different unlocking behaviors:

### Linear Stream

- Distribution at a constant rate per second
- The curve forms a straight diagonal line
- Ideal for simple and predictable distributions

#### variant

Linear with Cliff

- Similar to the linear stream but with an initial blocking period
- No tokens are available during the cliff period
- After the cliff, the distribution becomes linear
- Perfect for vesting plans with a blocking period

Unlocking and Linear

- Similar to the linear stream but with an unlocking initial
- A number of tokens are unlocked instantly
- Distribution balance becomes linear
- Perfect for short and medium term incentive plans

Unlock, Cliff and Linear

- Combination of unlock, cliff and linear
- A number of tokens are unlocked instantly
- After the first unlock, a lock period is applied
- Distribution balance becomes linear after the lock period
- Perfect for token launch plans

### Exponential Stream

- Distribution accelerates over time
- Recipients receive more tokens towards the end of the period
- Ideal for airdrops and long term incentives

#### variant

Exponential Cliff

- Combines three elements:
1. A cliff period (initial lock)
2. An instant unlock of a specific amount at the end of the cliff
3. An exponential distribution for the rest of the tokens
- Particularly suitable for team vesting plans
- Encourages long term retention

### Timelock

- Locks all tokens for a set period
- Release all at once at the end of the period

### Unlock in Steps

- Unlock in regular steps
- Allows regular periodic releases
- Unclaimed tokens accumulate

#### variant

Unlock Monthly

- Monthly tier
- Monthly unlock on a fixed date
- Ideal for salaries or ESOP plans

BackWeighted

- Progressive tier unlock
- Tier with independent value
- Unclaimed tokens accumulate
- Ideal for vesting plans with unlocking tiers

## Specific information

### Technical structure of segments

This structure is used by the dynamic stream (LD)
Each `segment` in a `dynamic stream` is defined by three parameters:

- `amount`: amount of tokens to distribute (uint128)
- `exponent`: the exponent defining the distribution curve (UD2x18)
- `timestamp`: end timestamp of the segment (uint40)

The distribution formula follows: f(x) = x^exp \* csa + Σ(esa)
where:

- x = elapsed time / total time of the segment
- exp = exponent of the segment
- csa = amount of the current segment
- Σ(esa) = sum of the amounts of the previous segments

#### Structure of the tuple

In Tally, the Batch part of the LD type functions are composed of a `segments` field which is an array of type `tuple[]`.
Please note that the following explanations are based on the `createWithTimestampsLD` function, for other LD type functions, you will have to adapt the tuple types.

```solidity
struct Segment {
uint128 amount;
UD2x18 exponent;
uint40 timestamp;
}
```

the `segments` field in the `createWithTimestampsLD` function is composed of 3 segments

- segment 0: Represents the period between the startTime and the end of segment 0
- segment 1: Represents the period between the end of segment 0 and the end of segment 1
- segment 2: Represents the period between the end of segment 1 and the end of the stream

## Creation of the hourglass for the Team RealT budget
The following parameters are those of the proposal [RIP000xx] - Release of REGs of the Team RealT budget
The stream is of type Cliff Exponential, this means that the stream starts after a cliff period, then distributes the tokens exponentially, with larger releases towards the end of the period.

![payout_sablier_team_realt.png](/imag-en/payout_sablier_team_realt.png)

### Parameters for the `createWithTimestampsLD` function


Find the proposal on Tally: https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposals

```
Function/Signature: createWithTimestampsLD(address,address,tuple[])
Contract Target: 0x0F324E5CB01ac98b2883c8ac4231aCA7EfD3e750

Calldata (batch):
lockupDynamic -> address: 0x555eb55cbc477Aebbe5652D25d0fEA04052d3971
asset -> address: 0x0AA1e96D2a46Ec6beB2923dE1E61Addf5F5f1dce
batch -> tuple[]:
 sender -> address: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c
 recipient -> address: 0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099
 amount -> uint256: 50000000000000000000000000
 transferable -> bool: true
 cancelable -> bool: false
 startTime -> uint40: 1714521600
 segments -> tuple[]: [
 ["0", "1000000000000000000", "1746057600"],
 ["500000000000000000000000", "1000000000000000000", "1746057601"],
 ["4950000000000000000000000", "3000000000000000000", "1872288000"]
 ]
 broker -> tuple [address, uint256]: [0x0000000000000000000000000000000000000000, 0]
```

#### Details of segments

```json
[
 ["0", "1000000000000000000", "1746057600"],
 ["500000000000000000000000", "1000000000000000000", "1746057601"],
 ["4950000000000000000000000", "3000000000000000000", "1872288000"]
]
```

- segment 0: ["0", "1000000000000000000", "1746057600"]
- segment 1: ["5000000000000000000000000", "10000000000000000000", "1746057601"]
- segment 2 : ["4950000000000000000000000000", "3000000000000000000", "1872288000"]

##### Explanations :

- segment 0 :

- 0 = amount distributed during the segment
- 1000000000000000000 = segment exponent = 1 in decimal (1000000000000000000 = 10^18)
- 1746057600 = segment end timestamp (May 1st) 2025 00:00:00 UTC)

- segment 1 :

- 500000000000000000000000 = amount distributed during the segment (500k ^18)
- 10000000000000000000 = segment exponent = 1 in decimal (1000000000000000000 = 10^18)
- 1746057601 = segment end timestamp (May 1, 2025 00:00:01 UTC)

- segment 2 :

- 4950000000000000000000000 = amount distributed during the segment (49.5 Million ^18)
- 30000000000000000000 = segment exponent (3000000000000000000 = 10^18)
- 1872288000 = segment end timestamp (May 1, 2029 00:00:00 UTC)

**Segment 1**, defines a segment end date with a distribution of 0 from the `startTime` parameter, which creates a hard lock period without distribution.

**Segment 2**, defines a segment end date with a distribution of 500k tokens, which releases an amount defined in amount instantly available at the segment end date.

**Segment 3**, defines a segment end date with a distribution of 49.5 Million tokens, which creates the exponential curve from the end of segment 2 to the end of segment 3, it will distribute the amount amount over the period with an exponent of 3