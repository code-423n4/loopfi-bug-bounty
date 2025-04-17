# The LoopFi Bug Bounty is closed for submissions

# LoopFi Bounty Details

- Until TVL < $500,000
  - Critical: $50,000
- Once TVL > $500,000
  - Critical: $100,000
- High severity: &#36;25,000 - &#36;50,000
- Medium severity: &#36;10,000 - &#36;25,000
- [Read our Code4rena bug bounty guidelines for more details](https://docs.code4rena.com/awarding/judging-criteria/bounty-criteria)

❗ _Note for participants: The sponsor's repo, scope definition, and contents herein are all subject to change._

## Publicly Known Issues

- Crafting malicious calldata so that users get less funds as expected when claiming lpETH on LRT deposits (e.g. by setting big slippage/price impact)
- Owner setting malicious lpETH and lpETHVault contracts. Users have 7 days to withdraw in that case.
- Previous audit findings are out of scope.

All known/disputed/unsatisfactory issues from LoopFi [contest](https://code4rena.com/audits/2024-05-loopfi#top) in C4 and any previous audits.


# Project Overview

Users can lock ETH, WETH and wrapped LRTs into this contract, which will emit events tracked on a backed to calculate their corresponding amount of points. When staking, users can use a referral code encoded as `bytes32` that will give the referral extra points.

When Loop contracts are launched, the owner of the contract can call only once `setLoopAddresses` to set the `lpETH` contract as well as the staking vault for this token. This activation date is stored at `loopActivation`.

Once these addresses are set, all deposits are paused and users have `7 days` to withdraw their tokens in case they changed their mind, or they detected a malicious contract being set. On withdrawal, users loose all their points.

After these `7 days` the owner can call `convertAllETH`, that converts all ETH in the contract for `lpETH`. This conversion has the timestamp `startClaimDate`. The conversion for LRTs happens on each claim by using 0x API. This is triggered by each user.

After the global ETH conversion, users can start claiming their `lpETH` or claiming and staking them in a vault for extra rewards. The amount of `lpETH` they receive is proportional to their locked ETH amount or the amount given by the conversion by 0x API. The minimum amount to receive is determined offchain and controlled by a slippage parameter in the frontend dApp.

For more information about LoopFi, please visit https://www.loopfi.xyz/.


## Links

- **Previous audits:** [Audit Reports](https://docs.loopfi.xyz/extras/security)
- **Documentation:**
  - [Loopfi introduction](https://docs.loopfi.xyz/)
- **Codebase:**
  - [Contracts](https://github.com/loopfi-io/loopfi-contracts.) 
- **Website:** https://www.loopfi.xyz/
- **Twitter:** [@loopfixyz](https://x.com/loopfixyz)
- **Discord:** https://discord.gg/mVqf2Q5Whg


# Scope

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [PrelaunchPoints.sol](https://github.com/LoopFi/loop-prelaunch-contracts/blob/main/src/PrelaunchPoints.sol) | 135 | Users can stake ETH into this contract, which will emit events tracked on a backed to calculate their corresponding amount of points. When staking, users can use a referral code encoded as bytes32 that will give the referral extra points. | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

# Additional context

## Main invariants

- Only the owner can set new accepted LRTs, change mode to emergency mode on failure of 0x integration, and set a new owner
- Deposits are active up to the lpETH contract and lpETHVault contract are set
- Withdrawals are only active on emergency mode or during 7 days after loopActivation is set
- Users that deposit ETH/WETH get the correct amount of lpETH on claim (1 to 1 conversion)
- Users that deposit LRTs get the correct amount assuming a favorable swap to ETH


## Attack ideas (where to focus for bugs)
- Malicious 0x protocol calldata crafting by users to steal funds on claim
- User funds getting locked forever

## All trusted roles in the protocol


| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| Owner                          | Has access to privileged functions, contract owner             |

## Miscellaneous

Employees of LoopFi and employees' family members are ineligible to participate in this bounty.
