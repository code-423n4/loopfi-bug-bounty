# LoopFi Bounty Details
- Until TVL < $500,000
  - Critical: $50,000
- Once TVL > $500,0000
  - Critical: $100,000
- High severity: $25,000 - $50,000
- Medium severity: $10,000 - $25,000

- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/bounties/SponsorName/submit)
- [Read our Code4rena Blue guidelines for more details](https://docs.google.com/document/d/1jzNh1Bat5iK6ryqvQ41_8GQjQ-ifxhHDlFINL_uijr4/edit?usp=sharing)

❗ _Note for participants: The sponsor's repo, scope definition, and contents herein are all subject to change._

## Publicly Known Issues

- LoopFi commits to providing Known Issue Assurance to bug submissions through their program. This means that LoopFi will either disclose known issues publicly, or at the very least, privately via a self-reported bug submission. 

Bug reports covering previously-discovered bugs (listed below) are not eligible for a reward within this program. This includes known issues that the project is aware of but has consciously decided not to “fix”, necessary code changes, or any implemented operational mitigating procedures that can lessen potential risk. 

All known/disputed/unsatisfactory issues from LoopFi [contest](https://code4rena.com/audits/2024-05-loopfi#top) in C4


# Project Overview

LoopFi is a dedicated lending market for Ethereum carry trades. Users can supply a long tail of Liquid Restaking Tokens (LRT) and their derivatives (e.g., Pendle LP tokens) as collateral to borrow ETH for increased yield and points exposure.

For more information about LoopFi, please visit https://www.loopfi.xyz/.


## Links

- **Previous audits:** [List of completed audits](https://docs.loopfi.xyz/extras/security)
- **Documentation:** [Loopfi Documentation](https://docs.loopfi.io/)
- **Codebase:** [Loopfi-contracts](https://github.com/loopfi-io/loopfi-contracts.) 
- **Website:** [Visit Website](https://www.loopfi.xyz/)
- **Twitter:** [Profile](https://x.com/loopfixyz)
- **Discord:** [Check LinkTree](https://tr.ee/31_WN-0wcv)


# Scope

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [loop-prelaunch-contracts/blob/main/src/](https://github.com/LoopFi/loop-prelaunch-contracts/blob/main/src/PrelaunchPoints.sol) | 135 | Users can stake ETH into this contract, which will emit events tracked on a backed to calculate their corresponding amount of points. When staking, users can use a referral code encoded as bytes32 that will give the referral extra points. | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

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

Employees of [SPONSOR NAME] and employees' family members are ineligible to participate in this bounty.
