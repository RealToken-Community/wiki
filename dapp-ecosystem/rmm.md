---
title: RMM
description: 
published: true
date: 2024-12-20T15:45:09.755Z
tags: 
editor: markdown
dateCreated: 2024-12-08T20:59:56.572Z
---

# **RMM v3**

## **1. Introduction to RMM Wrapper**

#### **⭐ For Beginners**

The RMM Wrapper is a smart contract that serves as an interface between RealTokens and the RMM protocol (RealToken Market Maker). It allows users to deposit their RealTokens and receive RTW (RealToken Wrapped) in exchange, which are used in RMM as proof of deposit for borrowing.

#### **⭐⭐ For Initiated Users**

The Wrapper plays a crucial role in the RMM ecosystem by:

- Standardizing the RealTokens interface for the RMM protocol
- Extending RMM's token capacity beyond its base limit of 128 tokens, allowing deposits of 2000 tokens per Wrapper, or approximately 100x2000 tokens
- For each RealToken deposited, the Wrapper creates RTW (RealToken Wrapped) valued at $1 per RTW, used in RMM as collateral for borrowing
- The wrapper creates and deposits as many RTW as the value (in $) of deposited RealTokens.
  The depositor receives as many armmRTW (proof of deposit) as the value of their deposited RealTokens.
- The wrapper manages RTW balances based on each RealToken's value by creating or burning RTW to keep the deposited value (in RTW) always equal to the deposited RealToken value.

![rmm_update.drawio.svg](/assets/img/rmm_update.drawio.svg)

## **2. Major Modifications Brought to the Wrapper by v3**

Wrapper address: 0x10497611Ee6524D75FC45E3739F472F83e282AD5

These modifications follow governance proposal RIP00008 which was accepted by the DAO on 2024-12-07 21:19 UTC.

[RIP00008 - Transfer RealToken Wrapper RMMv3 execution functions to the DAO](https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposal/4412019256781079844728885554420538992900805587725535743224739658055634526928)

#### **⭐ For Beginners**

The main modifications to the Wrapper were made to ensure legal compliance and handle routine operations related to RealToken lifecycle.

In certain cases, the issuer may need to destroy RealTokens:

- Hacking or loss of access to owner's address
- Death of owner
- Sale of the house linked to the RealToken
- Repayment of part or all of the debt linked to the RealToken

which could make RMM dysfunctional for corresponding RealTokens, as there would no longer be collateral to cover contracted debts.

To meet these obligations and ensure RMM's continuity and proper functioning, new features have been added to the Wrapper to partially or fully repay debt linked to a RealToken on an account and forcefully extract the concerned RealToken from affected accounts.

This process is supervised by DAO voting to ensure there is no abuse.
It guarantees that the amount due from detokenization is properly received by the owner, that RMM can continue to function and comply with legal obligations.

#### **⭐⭐⭐ For Experts**

To maintain proper RMM functioning, modifications to the Wrapper were made to allow recovery of RealTokens linked to an offer in cases of owner death, sale of the house linked to the RealToken, repayment of part or all of the debt linked to the RealToken, detokenization, or other events making the underlying asset ineligible for RMM deposit.

To recover or force withdrawal of RealTokens from the RMMv3 protocol, two functions are necessary:

- `repayForRecover`: This function handles partial or full repayment of debt that may be active on a RMM user's account (in cases like death, detokenization, etc.). Once executed, the second function can be safely called to extract RealTokens from RMM.
- `recoverByGovernance`: This function recovers RealTokens, either by withdrawing them to a specified address (service address for burn) or by moving aTokens/RealTokens to the specified address (user's new address).

These two functions can only be called by ecosystem governance (REG DAO). A proposal must be submitted in the DAO's active process and include calls to these two functions along with necessary approvals for execution, properly configured for each address requiring recovery (batch of addresses and tokens).

These functions are necessary in the following cases:

- Hacking of depositor's address (see special note on hacked addresses)
- Loss of access to depositor's address
- Death of depositor
- Detokenization or other events making the underlying asset ineligible for RMM deposit

Access Control: These two functions are limited to governance contracts; only a proposal approved by the RealToken ecosystem governance community (REG) can activate them. This measure ensures oversight of potentially dangerous functionality usage and provides total transparency.

## **3. Stakes and Considerations**

#### **⭐ For Beginners**

These modifications grant total rights over tokens deposited in RMM, making it vital that any submission to activate these functionalities be meticulously analyzed by the DAO to ensure there is no abuse.  
In principle, only RealT or an approved DAO partner issuing tokens should initiate such proposals.  
The DAO must ensure these functionalities are activated only in cases expressly requiring it to comply with regulations or guarantee proper RMM functioning.

#### **⭐⭐ For Initiated Users**

Abusive use of these functionalities could lead to token theft, abusive debt repayments, illegitimate borrowing, compromising RMM security and its users.

The main stakes related to the Wrapper are:

- Protection of deposited RealTokens
- Prevention of potential attacks
- Maintaining proper RMM functioning
- Ensuring compliance with legal obligations
- Ability to restore RealToken ownership to users who have lost account access

> It is crucial to understand that the Wrapper is a central RMM component and any modification or use of its functionalities must be carefully evaluated.
> {.is-warning}

#### **⭐⭐⭐ For Experts**

The RMM v3 Wrapper is a central contract in extending RMM capabilities and holds the majority of value placed as collateral for RMM debts.  
Each modification or use of these functionalities must be meticulously analyzed to ensure there is no abuse or error that could damage RMM.

The main considerations are:

1. Security of funds deposited in RMM

- A code error or misuse of functionality could damage RMM or create losses for users.

2. Protection of users and investors

- The DAO plays a major role in protecting users and investors, ensuring functionalities are used legitimately and in compliance with regulations.

3. Preservation of deposited RealTokens value

- The RMM v3 Wrapper is the bridge between RealToken and RMM, managing the value of RealTokens deposited in RMM and thus the value placed as collateral for RMM debts.

4. Risk management related to debts and borrowing

- A code error or functionality misuse could lead to incorrect valuation of collateral deposited in RMM, allowing borrowing beyond RealToken deposited value, creating under-collateralization of RMM debts (bad debt).

5. Fraud attempts

- A malicious user could attempt to defraud the system by making a proposal that appears harmless or even excellent, but actually attempts to: pay their debts with DAO treasury, steal RealTokens from users, or put users in liquidation positions.

## **4. ⭐⭐⭐ Technical Information**

### **4.1. repayForRecover**

#### **Parameters:**

- `repayWallet` (address): Address holding RealTokens in RMMv3 from which tokens with active debt will be extracted.
- `refundWallet` (address): Address that will receive any payment excess after debt repayment.
- `percent` (uint256): Percentage (scale 100% = 10000) of each deposited token to be extracted from RMM.
- `recoverAssets` (address[]): Array of token addresses representing RealTokens to be extracted from RMMv3.
- `debtAssets` (address[]): Array of token addresses representing debt assets to be repaid.
- `payer` (address): Address responsible for debt payment,  
  ! important: this address must have a balance of debtAssets tokens greater than or equal to the debt to be repaid and have approval for the `transferFrom` function of each debtAssets token benefiting the Wrapper.

Note: Asset values in RMM are calculated in $USD, so USDC/XDAI or others might not be exactly $1USD. It's important to calculate the `payer` balance with a safety margin that can reasonably be 5/100k.


## **5. Important Note:**

The `repayForRecover` and `recoverByGovernance` functions don't manage the Health Factor (HF). Therefore, ensure beforehand that HF allows withdrawal, otherwise the entire proposal will fail.

The `recoverByGovernance` function doesn't verify existing debts. This must be anticipated in the proposal. The `repayForRecover` function must be executed beforehand if necessary to avoid failure.

Addresses receiving RealTokens at any point must strictly comply with these tokens' compliance registry rules, otherwise the entire proposal will fail.

Approvals and balance for debt payment must include a minimum margin of 5/100k, otherwise the entire proposal will fail if the debt token value to be repaid is less than $1USD.

## **6. Modifications**

List of modifications in RealTokenWrapper update:

- Removal of \_isRealTokenWhitelisted verification for withdraw function (line 171): since it already verifies user's token balance in RealTokenWrapper. If balance > 0, it means RealToken was whitelisted. Should allow withdrawal and not allow supply if RealToken is no longer whitelisted now.

```
if (!_isRealTokenWhitelisted[asset]) revert WrapperErrors.TokenNotWhitelisted(asset);
```

- Use of user's cached token array for gas optimization (\_tokenListOfUser instead of \_realTokenList)

Change from direct reading from \_realTokenList storage mapping

```
uint256 length = _realTokenList.length;
uint256 userIndex = 0;
```

to copying \_tokenListOfUser[user] to memory (this list is individual for each user) (lines 484-485)

```
// Cache user token list for gas optimization
address[] memory userTokenList = _tokenListOfUser[user];
uint256 length = userTokenList.length;
```

- Removal of all old recoverByGovernance and addition of 2 new repayForRecover/recoverByGovernance functions (lines 643 to 870)

> IMPORTANT: validation of a proposal executing `repayForRecover` and `recoverByGovernance` functions must be done with great attention, as it can be very dangerous if not properly configured or used for malicious purposes.
> {.is-warning}

### Upgrade December 20, 2024
**Objective**: Fix a bug related to gas optimization for the deposited tokens list.

**Issue encountered**: When a user performs a liquidation and receives aTokens, if these aTokens are not in the deposit list, the functions using this list cannot find these aTokens.
The values are correctly recorded in the Wrapper, the RTWs are properly created and deposited, but the RMM and Wrapper cannot find the values because the functions `getAllTokenBalancesOfUser` and `getUserIndex` use the `_tokenListOfUser` variable which does not contain all the information.

**Solution**: Fix the liquidation function to check if the recovered tokens are in `_tokenListOfUser`. If not, they are added, allowing the Wrapper and RMM to correctly retrieve all information.

New implementation deployed: https://gnosisscan.io/address/0xc7ca0b893c22f99bb99dfc9dafdb6a83e0e7a946#code

Modifications:
- Code added lines 312 to 317
https://gnosisscan.io/address/0xc7ca0b893c22f99bb99dfc9dafdb6a83e0e7a946#code#F1#L312

- Code added lines 357 to 362
https://gnosisscan.io/address/0xc7ca0b893c22f99bb99dfc9dafdb6a83e0e7a946#code#F1#L357

## **7. Audit**

Performed by ADBK company: https://github.com/abdk-consulting/audits/tree/main/realt