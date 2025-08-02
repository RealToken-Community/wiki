---
title: Claim for RMM fees
description: 
published: true
date: 2025-08-02T08:09:43.881Z
tags: 
editor: markdown
dateCreated: 2025-08-02T08:09:43.881Z
---

# Technical Documentation - RmmClaimSplitFee Contract

[![Solidity](https://img.shields.io/badge/Solidity-0.8.27-blue.svg)](https://soliditylang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Network](https://img.shields.io/badge/Network-Gnosis-green.svg)](https://gnosis.io/)

## 📋 Deployment Information

- **Network**: Gnosis Chain (Chain ID: 100)
- **Contract Address**: `0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137`
- **Deployer**: `0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099`

https://gnosisscan.io/address/0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137#code

### 🔑 Configured Roles

| Role | Address | Description |
| -------------- | -------------------------------------------- | ------------------------------------------------------------------------------- |
| **Admin** | `0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099` | Reserve and Controller Management (will be transferred to the DAO at a later date) |
| **Governance** | `0x3f2d192F64020dA31D44289d62DB82adE6ABee6c` | Token Management and Allocations and Unpause Recovery |
| **Emergency** | `0xa26851621Ae64c66D09742c43591ad620A5B8256` | Emergency Controls (Pause) |

## 🎯 Overview

The `RmmClaimSplitFee` contract is an automatic token distribution system that allows fees collected in a reserve (RMMv3) to be distributed to multiple recipients according to predefined percentages.

### 🔧 Main Features

- ✅ **Multi-token Distribution**: Supports multiple ERC20 tokens simultaneously (armmToken)
- ✅ **Configurable Distribution**: Customizable percentages for each recipient (to 2 decimal places)
- ✅ **Enhanced Security**: Reentrancy protection and access control
- ✅ **Emergency Management**: Pause mechanism for critical situations

## 📊 Data Structure

### RepartitionInfo
Distribution is performed on all configured tokens for all configured recipients without distinction.

```solidity
struct RepartitionInfo {
address user; // Recipient address
uint64 repartitionPercentage; // Percentage (base 10000)
}
```

#### Percentage and Recipient Update Process

**Add a Recipient**
To add a recipient, you will need to update the other recipients to have a total of 100% (10000) for all cumulative recipients.
You must call the `addRepartitionInfo` function with the array of recipients and the percentages.

- Start by removing the recipient(s) for which the percentages need to be updated (removeRepartitionInfo).
- Add the new recipient(s) with the percentage (addRepartitionInfo).

**Delete a Recipient**
To delete a recipient, you will need to update the other recipients to have a total of 100% (10000) for all cumulative recipients.
You must call the `removeRepartitionInfo` function with the index of the recipient to be deleted.

- Start by removing the recipient(s) for which the percentages need to be updated (removeRepartitionInfo).
- Add the new recipient(s) with the percentage (addRepartitionInfo).

**Important Details:**

- The percentage is expressed in base 10000 (e.g., 5000 = 50%)
- Maximum allowed: 10000 (100%)
- Do not save a recipient with a percentage of 0%
- The `addRepartitionInfo` and `removeRepartitionInfo` functions are called by the DAO via a vote; all update steps must be performed in a single transaction.

Transaction example: https://gnosisscan.io/tx/0x0bea7110df6c16439f542fbc81f79781c9482f48ae71bac9c45857704e459b77

The structure of the tulpe[] parameter:

```
[
["0x3f2d192F64020dA31D44289d62DB82adE6ABee6c",8000],
["0x3e652E97ff339B73421f824F5b03d75b62F1Fb51",2000]
]
```

## 🚀 Main Functions

### 1. 📤 `claim()` - Token Distribution

**Signature :**

```solidity
function claim() external whenNotPaused nonReentrant
```

**Description:**
Public function (accessible from any address) that triggers the distribution of all configured tokens to all recipients according to their respective percentages.

**Execution Process:**

1. Retrieves the list of tokens and recipients
2. For each token:
- Reads the available balance in the reserve
- Calculates and transfers each recipient's share
3. Uses the formula: `amount * percentage / 10000`

**Prerequisites:**

- ✅ Contract not paused
- ✅ At least one token configured
- ✅ At least one recipient configured
- ✅ Available balance in the reserve

**Security:**

- 🛡️ Reentrancy protection (`nonReentrant`)
- 🛡️ Emergency pause enforcement (`whenNotPaused`)

**Example of Use:**

```typescript
// Via ethers.js
await rmmClaimSplitFee.claim();

// Via viem
await walletClient.writeContract({ 
address: '0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137', 
abi: contractAbi, 
functionName: 'claim',
});
```

---

### 2. 🪙 `addTokens()` - Adding Tokens

**Signature:**

```solidity
function addTokens(IERC20[] calldata newTokens) external onlyRole(GOVERNANCE_ROLE)
```

**Description:**
Adds new ERC20 tokens to the list of tokens to be distributed.

**Parameters:**

- `newTokens`: Array of ERC20 contract addresses

**Validations:**

- ✅ Only the GOVERNANCE role can call this function
- ✅ Each address must be a valid contract (code.length > 0)

**Emitted Event:**

```solidity
event TokensSet(IERC20[] newTokens);
```

**Example of Use:**

```typescript
// Add a single token
const tokenAddresses = ['0x...tokenAddress'];
await rmmClaimSplitFee.connect(governanceWallet).addTokens(tokenAddresses);

// Add several tokens
const multipleTokens = ['0x...token1Address', '0x...token2Address', '0x...token3Address'];
await rmmClaimSplitFee.connect(governanceWallet).addTokens(multipleTokens);
```

**Gas Cost:**

- **Minimum**: ~27,884 gas
- **Maximum**: ~126,897 gas
- **Average**: ~81,440 gas

---

### 3. 👥 `addRepartitionInfo()` - Recipient Configuration

**Signature:**

```solidity
function addRepartitionInfo(RepartitionInfo[] calldata newRepartitionInfo) external onlyRole(GOVERNANCE_ROLE)
```

**Description:**
Adds new recipients with their respective distribution percentages.

**Parameters:**

- `newRepartitionInfo`: Array of RepartitionInfo structures

**Validations:**

- ✅ Only the GOVERNANCE role can call this function
- ✅ Each percentage must be ≤ 10000 (100%)

**Event Emitted:**

```solidity
event RepartitionInfoSet(RepartitionInfo[] newRepartitionInfo);
```

**Usage Examples:**

#### Example 1: Simple Distribution (2 recipients)

```typescript
const repartitions = [
{
user: '0x...address1',
repartitionPercentage: 7000, // 70%
},
{
user: '0x...address2',
repartitionPercentage: 3000, // 30%
},
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(repartitions);
```

#### Example 2: Complex Distribution (5 recipients)

```typescript
const complexRepartitions = [ 
{ 
user: '0x...treasuryAddress', 
distributionPercentage: 4000, // 40% - Cash 
}, 
{ 
user: '0x...developmentAddress', 
distributionPercentage: 2500, // 25% - Development 
}, 
{ 
user: '0x...marketingAddress', 
distributionPercentage: 1500, // 15% - Marketing 
}, 
{ 
user: '0x...stakingAddress', 
distributionPercentage: 1500, // 15% - Staking Rewards 
}, 
{ 
user: '0x...burnAddress', 
distributionPercentage: 500, // 5% - Burn 
},
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(complexRepartitions);
```

**Gas Cost:**

- **Minimum**: ~27,839 gas
- **Maximum**: ~120,987 gas
- **Average**: ~81,108 gas

## 📋 Recipient Management (Recipient Info)

### 🔍 Recipient Query

**View Function:**

```solidity
function getRepartitionInfo() external view returns (RepartitionInfo[] memory)
```

**Usage Example:**

```typescript
// Retrieve all current recipients
const currentRepartitions = await rmmClaimSplitFee.getRepartitionInfo();

console.log('Configured recipients:');
currentRepartitions.forEach((repartition, index) => {
console.log(`${index + 1}. ${repartition.user} - ${repartition.repartitionPercentage / 100}%`);
});
```

### ➕ Adding New Recipients

New recipients are added to the existing list (no replacement).

**Adding Strategies:**

#### 1. Incremental Addition

```typescript
// Add a new recipient to the existing configuration
const newRecipient = [
{
user: '0x...newAddress',
repartitionPercentage: 1000, // 10%
},
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(newRecipient);
```

#### 2. Complete Reconfiguration

```typescript
// 1. Delete all existing recipients
const currentRepartitions = await rmmClaimSplitFee.getRepartitionInfo();
for (let i = currentRepartitions.length - 1; i >= 0; i--) { 
await rmmClaimSplitFee.connect(governanceWallet).removeRepartitionInfo(i);
}

// 2. Add the new configuration
const newConfiguration = [ 
{ user: '0x...address1', repartitionPercentage: 5000 }, 
{ user: '0x...address2', repartitionPercentage: 3000 }, 
{ user: '0x...address3', repartitionPercentage: 2000 },
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(newConfiguration);
```

### ➖ Recipient Removal

**Function:**

```solidity
function removeRepartitionInfo(uint256 index) external onlyRole(GOVERNANCE_ROLE)
```

**⚠️ Important:** Removal uses the "swap and pop" technique:

- The element to be removed is replaced by the last element
- The last element is removed
- **The indices change after each removal**

**Secure Removal Example:**

```typescript
// Remove multiple recipients (always start from the end)
const indicesToRemove = [2, 1, 0]; // Descending order!

for (const index of indicesToRemove) {
await rmmClaimSplitFee.connect(governanceWallet).removeRepartitionInfo(index);
}
```

### 📊 Percentage Validation

**Validation Rules:**

- ✅ Each individual percentage: `≤ 10000` (100%)
- ⚠️ **No automatic validation of the total sum**
- 🎯 **Recommendation**: The sum should equal 10000 for a complete distribution

**Manual Validation Example:**

```typescript
function validateRepartitions(repartitions) {
const total = repartitions.reduce((sum, rep) => sum + rep.repartitionPercentage, 0);

if (total !== 10000) {
console.warn(`⚠️ The sum of the percentages is ${total / 100}% (recommended: 100%)`);
}

return total;
}

// Use
const distributions = [ 
{ user: '0x...', repartitionPercentage: 6000 }, 
{ user: '0x...', repartitionPercentage: 4000 },
];

validateRepartitions(repartitions); // ✅ Total: 100%
```

## 🔧 Management Functions

### 📤 Token Deletion

```solidity
function removeToken(uint256 index) external onlyRole(GOVERNANCE_ROLE)
```

**Example :**

```typescript
// Delete the first token (index 0)
await rmmClaimSplitFee.connect(governanceWallet).removeToken(0);
```

### 🔍 View Functions

```solidity
// Retrieve all configured tokens
function getTokens() external view returns (IERC20[] memory)

// Retrieve the reserve address
function getReserve() external view returns (address)

// Retrieve the reserve controller
function getReserveController() external view returns (IReserveController)
```

## 🚨 Emergency Functions

### ⏸️ Emergency Pause

```solidity
function pause() external onlyRole(EMERGENCY_ROLE)
```

**Effect:** Blocks the `claim()` function until it resumes.

### ▶️ Resuming

```solidity
function unpause() external onlyRole(GOVERNANCE_ROLE)
```

**Note:** Only the GOVERNANCE role can resume (not EMERGENCY).

## 🛡️ Security and Best Practices

### ✅ Built-in Security Controls

- **Reentrancy Protection**: `ReentrancyGuardTransient`
- **Access Control**: OpenZeppelin Role System
- **Input Validation**: Contract address verification
- **Pause Mechanism**: Emergency shutdown available

### 🎯 Usage Recommendations

1. **Percentage Validation**: Always verify that the sum = 10,000
2. **Development Environment Testing**: Test the configuration before deployment
3. **Monitoring**: Monitor events emitted for configuration changes
4. **Configuration Backup**: Keep a copy of the configurations for rollback

### ⚠️ Points of Attention

- **Deletion of Elements**: The indices change after each deletion
- **No Sum Validation**: The contract does not verify that the percentages add up to 100%
- **Execution Order**: The recipients are processed in the order listed

## 📞 Support and Maintenance

For any technical questions or integration issues, please refer to:

- **Full Documentation**: `README.fr.md`
- **Reference Tests**: `test/RmmClaimSplitFee.test.ts`
- **Source Code**: `contracts/RmmClaimSplitFee.sol`

---

**🔗 Useful Links:**

- [Gnosis Chain Explore](https://gnosisscan.io/address/0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137)
- [OpenZeppelin AccessControl](https://docs.openzeppelin.com/contracts/4.x/access-control)
- [Solidity Documentation](https://docs.soliditylang.org/)