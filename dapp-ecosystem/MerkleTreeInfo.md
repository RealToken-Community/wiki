---
title: What is a Merkle Tree?
description: 
published: true
date: 2025-08-02T08:14:49.752Z
tags: 
editor: markdown
dateCreated: 2025-08-02T08:14:49.752Z
---

# Merkle Trees Explained Simply

Imagine you want to prove that your photo is part of a family album without having to show all the other photos, in order to preserve the privacy of other people in that album. This is exactly what Merkle trees do: they allow you to **verify that a piece of information is part of a set without having to reveal everything**.

For the Realtoken DAO and in the blockchain, Merkle trees are also used to enable actions on a smart contract that doesn't know the actions before the user executes the transaction. It's a bit like if the smart contract were the person to whom you want to prove that the photo is in the album without providing anything other than your photo and clues (hash).
The only thing the smart contract knows is the "root" hash of the Merkle tree (your photo here would be the actions you ask the smart contract to perform).

## What is a digital fingerprint (hash)?

### The Fingerprint Analogy

Just as each person has a unique fingerprint, each piece of digital data can have its own "fingerprint" called a hash. This hash has some remarkable characteristics:

- **Unique**: Two different pieces of data will always have different hash values (this can be false with certain vulnerable algorithms like md5)
- **Always the same size**: Whether you have a single word or an entire book, the hash value is always the same length
- **Irreversible**: The original data cannot be retrieved from its hash value
- **Very sensitive**: The slightest change in the data completely alters the hash value

To test, you can enter a word, a letter, and a page of text on this site, for example: <https://devbox.tools/fr/utils/md5-sha1-sha256-hash-generator/>

### Demonstration with concrete examples

Here's how small changes produce completely different hash values based on SHA256

| Original text | Hash value (first characters) | Change made |
|---|---|---|
| "Hello" | 9172e8eec99f144f72eca9a568759580edadb2cfd154857f07e657569493bc44 | Original text |
| "hello" | 2cb4b1431b84ec15d35ed83bb927e27e8967d75f4bcd9cc4b25c8d879ae23e18 | Just the capital letter in lowercase |
| "Hello!" | 083de31ac1fa14f95671a6e39cc6c72d8fed1590b2a51759bc3f54a76b4169c4 | Adding a simple exclamation point |
| "Hello Alice" | 44a0fc349f47f9774122f1de8eca398bda30eb145ea2bb552b7961411601f0f0 | Adding a first name |
| "Hello Alice." | d57bdd277c9301838b2db4d7764afbac2e903c493ae893c3c259486475bbf665 | Adding a period |

As you can see, even changing a single letter from uppercase to lowercase completely transforms the fingerprint. This property is what makes fingerprints so useful for detecting even the smallest changes.

In the case of a smart contract, this means that if the user attempts to enter data other than that specified in the merkle tree, the contract will be able to detect that the user is attempting to cheat on the transaction execution, without ever actually knowing the data in the merkle tree.

## The Merkle Tree: Like a Family Tree for Data

### The Family Tree Analogy

Think of a family tree. At the top, you have the grandparents (the root), then their children (the branches), and finally the grandchildren (the leaves). In a Merkle tree, it's the same, but with digital fingerprints:

- **Leaves** = the fingerprints of your original data (such as photos)
- **Branches/proofs** = the combined fingerprints of several leaves
- **Root** = the final fingerprint that represents the entire set

### Concrete example: The family photo album

Let's imagine you have four family photos that you want to protect from modification, and prove to people that these photos are indeed in the album.

**Step 1: Create the leaves**
Each photo receives its own unique fingerprint (like its own digital ID).

1. **Photo of Marie** on her birthday → Fingerprint: b03a70221b0799301928f74ae1194372e6b7ee8d6d1d1ef6d1aeed9bc7d09ae5
2. **Photo of Paul** at the wedding → Fingerprint: 6c4ad6287b8599400e76ac8542fe542fd699ff644da253932dd76bd12c9a0d94
3. **Photo of Sophie** at the beach → Fingerprint: 1d1b6f002f11dd0b99f98ac981e6654d3d887038cb4cd1a3f985331fe3c7ad52
4. **Photo of Jean** Skiing → Footprint: 28344c68575b44fdd51a06ae48e33bc20d1e8eecd462b13073a18d0d13630446

**Step 2: Create the branches**

- Combine Marie's and Paul's footprints: c798a57a13f131e957ae236c3b608574f9c3f8dc53c443af214b478bee7158f9
- Combine Sophie's and Jean's footprints: 33a5d59250e94276f5b63e50b32fd4842a53f6ac7d93234db3c39a646294fae7

**Step 3: Create the Root**
We combine the two branches to obtain a final fingerprint: e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

This final fingerprint (the root) is like a "summary" of the entire album. If someone changes even one pixel in a photo, the order of the photos, or anything else, the root will be completely different.

## How to verify without revealing everything: The magic of the Merkle proof

### The problem to solve

Suppose Sophie wants to prove that her photo is part of the family album, but she doesn't want to reveal the other photos (to respect privacy).

### The solution: The Merkle proof

Instead of showing all the photos, Sophie only needs a few details.

1. **Her fingerprint**: 1d1b6f002f11dd0b99f98ac981e6654d3d887038cb4cd1a3f985331fe3c7ad52
2. **John's fingerprint** (her "neighbor" in the tree): 28344c68575b44fdd51a06ae48e33bc20d1e8eecd462b13073a18d0d13630446
3. **The fingerprint of the Mary+Paul branch**: c798a57a13f131e957ae236c3b608574f9c3f8dc53c443af214b478bee7158f9
4. **The public root** (known to all): e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

### The verification process

**Step 1**: Combine Sophie + John = 33a5d59250e94276f5b63e50b32fd4842a53f6ac7d93234db3c39a646294fae7

**Step 2**: Combine this new branch with the Marie+Paul branch: c798a57a13f131e957ae236c3b608574f9c3f8dc53c443af214b478bee7158f9 + 33a5d59250e94276f5b63e50b32fd4842a53f6ac7d93234db3c39a646294fae7 = e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

**Step 3**: Compare with the public root: e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee === e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

If the two roots are identical, this proves that Sophie's photo is indeed part of the original album. The person verifying the proof can therefore: hash Sophie's photo, perform the combination steps, and prove that the root is indeed the one in the album.

In our use case with a smart contract, they can therefore verify that the requested action is indeed the one listed in the merkle tree, which was voted on and approved by the DAO. At no time can a user ask the contract to execute anything other than what is in the merkle tree. Remember the hash test: the slightest modification, no matter its size or location, will produce a completely different result. The execution of freely accessible actions is therefore secure without having to store a large amount of data in the contract.

## Why is this revolutionary?

### Benefits in everyday life:
1. **Efficiency**: No need to check the entire album, just a few fingerprints
2. **Privacy**: Other photos remain private
3. **Security**: Impossible to cheat without it being noticed
4. **Speed**: Verification is almost instantaneous

### Real-life analogy

It's like being able to prove that your name is on a wedding guest list without having to reveal all the other names on the list. You simply provide your name and a few "clues" that allow the reconstruction of a "secret code" that matches the one on the official list.

## Practical applications: This technology is used all around us.

- **Bitcoin**: To verify that your transaction is in a block and cannot be modified in the future.
- **Backups**: To ensure that your files have not been corrupted.
- **Software updates**: To guarantee that a program has not been modified.
- **Electronic voting systems**: To verify the integrity of results.
- **Privacy**: To prove information without disclosing all the information.

## In summary: A Merkle tree is like a **data fingerprinting system**

It allows you to:

- Create a unique "identity card" for a set of data
- Instantly detect any changes
- Verify that a piece of data is part of a set without revealing everything
- Protect privacy while ensuring security

The next time you hear about Merkle trees, think of that family photo album: it's a simple and secure way to prove that your data is authentic without having to show it in full.

## In conclusion

For the DAO, using a Merkle tree to securely execute transactions will allow a single vote to validate a multitude of transactions that would otherwise not have been able to fit entirely into the voting transaction due to limited block space. The use of merkle tree is therefore suitable for votes which require a large number of transactions exceeding the capacity of a block, for other cases the transactions will remain included directly in the voting transaction.