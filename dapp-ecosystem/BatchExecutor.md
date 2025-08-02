---
title: Group Action Executor for DAO
description: 
published: true
date: 2025-08-02T08:23:36.739Z
tags: 
editor: markdown
dateCreated: 2025-08-02T08:23:36.739Z
---

# 📜 `MerkleGovernanceExecutor` Documentation

## Introduction: What is it?

The `MerkleGovernanceExecutor` is the "powerhouse" of our DAO. It's a smart contract on the blockchain with a single primary mission: to enable the execution of batches of complex transactions approved by the DAO in a secure and verifiable manner.

Imagine the DAO voting to fund a project, change a parameter in the RMM, recover lost funds from multiple addresses that have deposited in the RMM, or launch a new feature. This contract is the one that pushes the buttons and ensures that only the will of the DAO, and nothing else, is implemented, all in a decentralized, secure, and transparent manner by anyone who wants to execute the transactions.

---

! Before continuing, you should understand how a Merkle tree works -> [merkle-tree](merkle-tree.md)

### ⭐ : For Everyone (The Big Picture) 🔭

Think of this contract as an ultra-secure and automated notary.

1. The DAO Decision: The community votes on a set of decisions (e.g., "Pay Alice €100," "Update the website logo," "Allocate 1,000 tokens to project X, retrieve the RealTokens of 20 users on the RMM").
2. The Seal of Approval: Once the vote is complete and approved, all these decisions are grouped and sealed in a unique, tamper-proof "digital envelope." This seal is what we call the Merkle Root. This is a unique fingerprint of all approved decisions (transactions to be executed).
3. **Execution by the Notary**: Anyone can then present themselves to the "notary" (the `MerkleGovernanceExecutor` contract) with one of the approved decisions (for example, "Pay €100 to Alice"). To prove that this decision was indeed part of the vote, the person presents a "Merkle proof," which is a bit like a key that only works with the envelope seal.
4. **Verification and Action**: The "notary" (the `MerkleGovernanceExecutor` contract) verifies that the key (the proof) matches the seal (the Merkle root). If everything is correct, they execute the decision without question. If the proof is false, they categorically refuse.

**In short: This contract ensures that only actions validated by a DAO vote can be executed, in a completely transparent, secure, and autonomous manner by anyone.**

---

### ⭐⭐: For the Curious (How does it work?) 🤔

At this point, we introduce some technical concepts, but without diving into the code.

The contract operates on a principle of **validation by cryptographic proof**. It does not store each individual decision voted on by the DAO, which would be very expensive in terms of gas fees on the blockchain. Instead, it uses an optimized data structure called a **Merkle Tree**.
The contract stores the Merkle root of each vote approved by the DAO. It does not know any of the information about the voted transactions before their execution. This process therefore allows an unlimited number of transactions to be scheduled with a single vote.

**The Process, Step by Step:**

1. **Voting and Tree Creation**:
* The classic DAO voting process is applied, with discussions and preparations on the governance forum.
* An off-chain process takes all the transactions to be included in the vote, "hashes" them (creates a fingerprint for each), and organizes them into a Merkle Tree.
* The DAO votes on the contents of the entire Merkle Tree, which contains one or more batches of transactions to be executed.
* The top of this tree is the **Merkle Root** (`merkleRoot`). This is the only information the DAO needs to publish and record in our `MerkleGovernanceExecutor` contract; therefore, it is this information that the vote writes to the blockchain upon final execution of the vote.

2. **The Execution Call**:
* A user (or bot) can execute one or more transactions in this batch without requesting authorization.
* They call the contract's `execute` function.
* They must provide four key elements:
		1. The `merkleRoot` (the vote approval seal).
		2. The `transactions` they wish to execute (a set of transaction groups).
		3. A `batchId` (an identifier to ensure this batch is executed only once).
		4. The `merkleProof` (the cryptographic "key" that proves that their transactions are indeed part of the tree validated by the `merkleRoot`).

3. **Contract Verification**:
* The contract first checks whether the `merkleRoot` is an active and approved root.
* The contract checks whether the `batchId` has not already been executed.
* Then, it uses the `merkleProof` to recalculate the root from the provided transactions.
* If the recalculated root matches the expected `merkleRoot`, the proof is valid! The contract then executes the transactions.
* Finally, it marks this `batchId` as "executed" to prevent any re-execution.

**The major advantage**: It's extremely efficient. The contract only needs to know a single hash (`merkleRoot`) to potentially validate thousands of transactions and allow anyone to execute transactions without requesting authorization, without relying on a centralized entity that must be trusted.

---

### ⭐⭐⭐ : For advanced users (In Depth) 👩‍💻

The `MerkleGovernanceExecutor` is an *upgradeable* (via the UUPS pattern), *pausable*, and secure contract against reentrancy attacks. It uses OpenZeppelin's `AccessControl` to manage permissions.
Its main purpose is to enable the execution of complex transaction batches that cannot be contained in a voting transaction due to the block size limitation. The vote approves the Merkle root of the Merkle tree, which can contain an unlimited number of transactions grouped into batches that respect the gas limit. Transaction execution is completely free and limited to transactions and parameters approved by the DAO in a secure, verifiable, and tamper-proof manner.

**Main Roles:**

* `GOVERNANCE_ROLE`: This role (held by the DAO itself) is the only one that can add or remove `merkleRoots` via the `addMerkleRoots` and `removeMerkleRoot` functions. It is the one that "officializes" the results of a vote.
* `DEFAULT_ADMIN_ROLE`: Manages contract updates and can assign roles (also held by the DAO).
* `EMERGENCY_ROLE`: Can only pause the contract in case of an emergency (held by the security committer).

#### Focus on the `execute` function (main function)

Here is the signature of the most important function:

```solidity
function execute(
bytes32 expectedMerkleRoot,
bytes32[] calldata merkleProof,
uint256 batchId,
TransactionPayload[][] calldata transactions
) external payable whenNotPaused nonReentrant;
```

**Parameter Analysis:**

The contract manages multiple active merkle roots in parallel. This allows the DAO to have separate votes and record the result at any time in the contract, even if previous executions have not yet been fully executed.

* `expectedMerkleRoot`: The Merkle root that authorizes this batch. The contract verifies that it has been previously added by the `GOVERNANCE_ROLE`. * `merkleProof`: The `bytes32` array that constitutes the cryptographic proof.
* `batchId`: A `uint256` that serves as a "nonce" or unique identifier for this transaction group batch to be executed. It is combined with the "leaf" of the Merkle tree, ensuring that the same batch cannot be executed twice with different `batchId`s.
* `transactions`: An array of `TransactionPayload` (transaction group) arrays. This structure allows you to group transactions that must be executed together successfully. If a transaction group fails, only that transaction group reverts, and this does not prevent other valid groups from being executed. This is called a soft revert.

In the case of a soft revert, the DAO will have to examine the group of transactions that have reverted and decide whether to re-execute it or not, for which it will have to vote again for this group of transactions.

**Internal logic of `execute`:**

1. **Verify the `merkleRoot`**: `require($.merkleRoots[expectedMerkleRoot], INVALID_MERKLE_ROOT());` verifies that the `merkleRoot` is an active and approved root.
2. **Build the leaf**: The contract calculates a hash of the `transactions` and the `batchId`. This is the leaf that must be proven.

```solidity
bytes32 leaf = keccak256(abi.encode(batchId, keccak256(abi.encode(transactions))));
```

3. **Non-execution check**: It verifies that this batch has not already been executed: `require(!$.executed[expectedMerkleRoot][leaf], ALREADY_EXECUTED());`
4. **Merkle proof check**: This is the core of the security process, verifying that the address executing the transaction is not attempting to cheat by executing modified transactions.

```solidity
require(MerkleProof.verify(merkleProof, expectedMerkleRoot, leaf), INVALID_PROOF());
```

5. **Marking and Execution**: If everything is OK, it marks the leaf as executed *before* executing the transactions to protect against reentrancy attacks. Then, it loops through the transaction groups and executes them via an internal `executeGroup` call. 6. **Fund Management**: The contract tracks the value of XDAI sent and ensures at the end that the contract balance matches what was expected, providing additional security (if XDAI is transferred).

This design enables decentralized, robust, and low-cost governance, where vote validation is decoupled from transaction execution, providing great flexibility.

---

### ⭐⭐⭐: Concrete transaction example

soon

### ⭐: FAQ

soon