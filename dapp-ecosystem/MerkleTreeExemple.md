---
title: Merkle Tree Example
description: 
published: true
date: 2025-08-02T08:18:32.971Z
tags: 
editor: markdown
dateCreated: 2025-08-02T08:18:32.971Z
---

# Example of using a Merkel Tree when claiming REG (simplified)

## ⭐ Imagine you are a user who wants to claim their REG (following a revaluation, for example)

### Step 1: You log in to the interface

The interface is the application https://claim.realtoken.network/reg , which you connect to with your crypto wallet.

### Step 2: The interface prepares your proof of eligibility

In the background, the interface uses the official list of beneficiaries. This list is very long and grouped into a special structure called a Merkle Tree.

The Merkle Tree is a bit like a secure summary of the entire list of people entitled to USDREG. It makes it very easy to prove that you are on the list, without needing to show the entire list.

The interface will search the Merkel Tree for your information:
- your address,
- the amount of USDREG you are entitled to,
- and a **proof**, called the "Merkle proof" (a series of small pieces of data, generally invisible to you), which proves that your address and the amount you are entitled to are part of this official list.

### Step 3: Submit your claim request with the proof

When you click "Claim," the interface sends to the blockchain:
- Your address,
- The amount you are claiming,
- This Merkle proof, which proves that you are indeed entitled to this right

### Step 4: The blockchain verifies your proof

The smart contract (Vault) only records the **top** of the Merkle Tree (called the root), which is the official summary of the entire list. Thanks to the proof that the interface sent with your request, the contract can easily verify, with a few quick calculations, that you are on the list without storing all the details.
If your proof is valid, it means that you are indeed eligible for the claim.

### Step 5: Your claim is validated and you receive your REG

The contract marks what you have already claimed, to prevent you from claiming the same thing multiple times. Then, it issues you your REG.

## In summary, why is using the Merkle Tree convenient?

- **Security:** You cannot claim more than you are entitled to.
- **Speed:** Your entitlement is verified in an instant, even if the list is very long.
- **Economy:** The system avoids recording tons of information on the blockchain, which makes transactions less expensive.

## ⭐⭐ Now let's see how the Merkel Tree is built.

The Merkel Tree is a file that is built by RealT and updated each time it is re-evaluated.
It is available at the following address: https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json
Since it is very large, you must click on "View raw" to read it.
Here's an example of what it contains:

"merkleRoot": "0x7600588c716d7c1e3157369e8d71a43e1049fa5f65ed78bd1bfd667736ccf697",
"tokenTotal": "0x182dd106b32a8ac023000",
"claims": {
"0x5fc96c182bb7e0413c08e8e03e9d7efc6cf0b099": {
"amount": "0x7794362e3f5ec2e3000",
"proof": [ 
"0x79fa2e8520e46c313c3adb9559688b3092b25d8f7d59576f5717893bd4f8ff3d", 
"0xa0c7dfe0135f30390b0d7002ce5ef5179a3ff6411211c6b46359bba9643ef81f", 
"0xc22c90eff57be6ea4d49d258179a5b4bb3d1ee2fbddc366e5c5492ed43a43819", 
"0xc0a3ad5c235ece884b9ddde6aa348c34e4c195ff1c70d3af41e7bbbcfad87090", 
"0x08e11058f59c7d7fab751b1715bcf95b71b5f9f5314d50005588fcc9025c7e1a", 
"0x8758c7db9b5e944a2314af956cf8564c6a75a29b438dcef4c1c272f59ecd0635", 
"0x607be7690b4ee889e191f9f7a69ae56bba7915c6c1604370074a930f70172175", 
"0x4bfd41fabd1caff0fbef9c762bf011bf8e32153ef7656ce2d459ab1d9071874c", 
"0xf892c05488e77b7478594f2ef10e92b8b39b1e8006c72b6bb77a9aaa7b85d90e", 
"0x0decec6c5017c73465c238126fe7f9a7b499a4df5575538b754113e058007b5f", 
"0x32d88461ca135f2e5c1b9bed49462dda99de88d5c476678e2d74850b5081b8de", 
"0xa30dacaedce1007c66e347a81e1a39b6cdbdb05d5d33dafc642d258c8b59ff25", 
"0x1d9ccbe1d5a8de0906890a41c21c78605b3e9fa9475a0579641eb6cc1f03056e", 
"0x802061aa7424653daaf294dea7462302357b3c9dceb2d35dbb1c472a76b6e08b" ] 
}, 
"0xc2eba74e4afebe88149b3f1ce9ac97beb2c7d1a6": { 
"amount": "0x14eb1ad47000", 
"proof": [ 
"0xa2450395032033080b75c249f1394503fc295d144d349c2581b86273c1bc2cdb", 
"0x983d3cd739a2644fc1ff504f95645074ca22cf93a712d052a4a82e679882c5f9", 
"0xda5b95c7a3c216a398a0a56ef541f89fcb3bf09105efdb4431e7f94d0826b9c2", 
"0xb6032b2b751842e9f89049f39745841a607af383d540f5b5c634f7c319e5d3e6", 
....

The presentation of the information is a bit unusual (JSON format), as it's organized to be read by a computer program. However, it's fairly easy for a human to understand. Starting from the third line, the information contained in the Merkel Tree "leaves" follows:
- the user's address: "0x5fc96c182bb7e0413c08e8e03e9d7efc6cf0b099" for the first, "0xc2eba74e4afebe88149b3f1ce9ac97beb2c7d1a6" for the next...
- the amount to be claimed: "0x7794362e3f5ec2e3000" for the first, "0x14eb1ad47000" for the second..(*)
- and finally, the proof:
This is a series of hashes, which allow us to verify that the address and amount are indeed included in this list. When the interface sends this information (from the sheet corresponding to the user making the claim) to the distribution smart contract (Vault), the latter will perform the following operation:
- it will assemble the address and the amount and hash the result,
- it will then combine the resulting hash with the first proof hash, then repeat this process until the last,
- and finally compare the final result with the root of the Merkel Tree it owns.
If they are identical, it means you have the right to make this claim, and the smart contract will execute it.

(*) Note regarding the amount: it is expressed in hexadecimal. You can convert it to decimal using, for example, the following application: https://www.rapidtables.com/convert/number/hex-to-decimal.html
The amount you obtain is expressed to 18 decimal places (USDREG precision), so you need to divide it by 10 to the power of 18 (or shift the decimal point 18 digits to the left) to obtain the result 35293.477 USDREG!.