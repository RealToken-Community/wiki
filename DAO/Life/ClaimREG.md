---
title: REG Claim
description: 
published: true
date: 2025-03-19T13:10:47.930Z
tags: 
editor: markdown
dateCreated: 2025-03-19T13:08:51.748Z
---

# Preamble: Why are REGs allocated to you? ⭐

Each time an asset issued in the ecosystem is tokenized, the DAO allows its tokenization partners to allocate an amount of USDREG (convertible only to REG) to customers who have purchased and held the tokens for a variable period depending on the asset. These USDREGs are convertible into REG at any time at the market price.

Currently, the amount of USDREG corresponds to the fees charged by RealT for its property sourcing work and is allocated at the time of the first revaluation of the property.

RealT has announced its intention to evolve the criteria for allocating USDREG rights to be applicable to all types of tokenizations RealT offers, not just traditional real estate (loan tokenization, for example, which does not require revaluation).

Before the creation of REG, this amount was paid in SOON tokens, whose value was 1 USDREG.

Once the REG claim application is operational, SOON will disappear, and allocation at the first revaluation will be made directly in USDREG (see RIP00009).

![valuation.png](/imag-en/regconvertor/valuation.png){.align-right .img40}

Property revaluations are performed by companies independent of RealT.

The cost of this action is becoming high (especially for multi-family properties) and is leading RealT to postpone these revaluations, which were originally planned to be annual.

In the future, the distribution of USDREG should no longer be linked to the revaluation, but to a delay after tokenization.

# The application to claim your REG ⭐

[https://claim.realtoken.network/](https://claim.realtoken.network/)
![claim1.png](/imag-en/regconvertor/claim1.png){.align-right .img35}

1. The application runs on the Gnosis blockchain.
2. You must connect one of the following wallets:
- the one declared as receiving the Realtokens, on [Realt.co](https://realt.co/) (at the time the conversion is launched): to claim the USDREG ($ convertible to REG) corresponding to the Soon received over several years.
- the one holding the Realtokens, which will be revalued subsequently (once the Soon distribution has stopped).
3. Choose your presentation mode (language, background).
4. Make sure you are in "REG Claim" and not "Genesis".

Note: *Walletless* users will need to migrate to *Realtoken Wallet or other compatible wallet* to connect to the application, then ask support to make the claim on the new Wallet for the tokens allocated to the Walletless. [FR Help Link](https://community-realt.gitbook.io/tuto-community/site-realt/option-realtoken-wallet-account-abstraction)

![claim2.png](/imag-en/regconvertor/claim2.png){.align-right .img30}
Once logged in, you will see:
1. The amount of USDREG allocated to you (20 in the image),
2. The current REG parity in $ (1.93),
3. The corresponding number of REG (10.34), which you can claim (only in full),
4. The amount of USDREG you have already claimed (100), before a new allocation,
5. The delegation option (see next chapter),
6. The Auto Claim option
You can authorize the automatic execution of claims for you (by a robot), as soon as USDREG is allocated to you. allocated (maximum 24-hour delay).
A fee may be charged for this service (currently 0%).
<br>

## Delegate Claim

![delegateclaim.png](/imag-en/regconvertor/delegateclaim.png){.align-right .img45}

It is possible to delegate another address to claim and collect your REG (for example, in the event of a corrupted wallet):
1. Log in to the application with the initial wallet (0x69…94a in the image),
2. Check the claim with another address box, and indicate the delegated address (0x…C08 in the image). Then sign this delegation authorization with the initial address. 3. The application indicates that you must change your login address to claim.
4. Log in with the delegated address (0x…C08 in the image).
5. Launch the claim.
6. The claim will send the REG allocated to the initial address to the delegated address.
<br>

# How it works ⭐⭐
## Before the creation of the reclamation contract

During the Soon period (2021 to March 2025) and before the creation of the REG reclamation contract (Vault):

1. Following each revaluation of a Realtoken, RealT launched the mint of the corresponding Soon,
![ccm1.png](/imag-en/regconvertor/ccm1.png){.align-right .img50}
2. These Soon were allocated to the wallets of the Realtoken owners at the time of the revaluation.

To initialize the reclamation contract, the Soon tokens are converted into USDREG and then destroyed:
3. RealT initiates the Soon token conversion,
4. Collects all wallets holding Soon tokens and their quantity,
5. The Soon tokens are converted into USDREG,
This amount for each wallet is recorded in an off-chain file ([Merkel Tree](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json), the converted Soon tokens are allocated for each address to the last known Realtoken delivery address on the user's realt.co website,
6. The Soon tokens are destroyed (burned).

All of the above actions are executed only once, when the reclamation contract is created.

## User Claim

Once the "Merkle Tree" file is initialized (the first time using accumulated Soons):

1. REG claims can be requested:
- from the claim application, which displays the number of USDREG,
- or directly on the contract from the user wallet (a more complex solution). 
![ccm2.png](/imag-en/regconvertor/ccm2.png){.align-right .img50}
2. The claim is executed by the Vault conversion smart contract.
3. It verifies, based on the Merkle Tree root transmitted to it, that the user has the corresponding rights.
4. It calculates the number of USDREGs based on the Merkle Tree data and the USDREGs already claimed (whose value it stores) to determine the number of USDREGs that can be claimed. This amount is then converted into REGs based on the Oracle price.
5. It requests the REG contract to create (mint) the REGs.
6. The new REGs are transferred to the user's wallet.
7. Subsequent revaluations/distributions (after the Soons disappear) will be made directly in USDREGs, by updating the Merkle Tree. This update will make new USDREG available for claiming appear in the application.
The new USDREG will be allocated to the address holding Realtokens at the time of the snapshot (unlike the USDREG entitlement from the Soon conversion, which is allocated to the last RealToken delivery address registered in your realt.co account).

If the address holding USDREG has been corrupted, it is possible to simply sign with the corrupted address, delegating the claim to another address. This other address will then be able to claim and receive the REG.

## "Automatic" Claim

The execution of the claim by the Vault smart contract can be triggered:
- either manually, at any time, by the user (the parity is then chosen),
- or automatically by a bot after the allocation of new USDREG (the parity is not chosen). The latter mode requires prior authorization.

1. The automaton will monitor Merkel Tree updates for registered users, then trigger a REG claim for the affected users.
![ccm3.png](/imag-en/regconvertor/ccm3.png){.align-right .img50}
2. Depending on the configuration of the Vault conversion smart contract, an automatic claim fee may be applied (0% initially) to encourage the creation of such an automaton and cover the associated transaction fees (enabling fees will require a governance vote).
3. The Vault will request the minting of two amounts of REG: one for the fees and one for the user.
4. The REG contract mints the fee REGs into the automaton's wallet.
5. The REG contract mints the user's REGs into the user's wallet.

Note: This feature cannot be combined with a delegated claim (which is similar to a user claim).

## Address

Smart contract:

- Soon: [0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88](https://gnosisscan.io/token/0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88)
- REG: [0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce](https://gnosisscan.io/token/0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce)
- Vault: [0x71eb2b2518556700f5b778b9cc313bfe8de5086e](https://gnosisscan.io/address/0x71eb2b2518556700f5b778b9cc313bfe8de5086e)
- Oracle: [0x86339b40e588f774bd766eB70D47bEFBe68B6F64](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced)

Merkel Tree (OffChain): [https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_conversion/current.json](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_conversion/current.json)

This file can be backed up by the community to remain available in all situations. The latest version, whose root is registered in the claim contract, remains valid even if the interface or source file no longer exists.

## Validation with Merkle Proof

Validation with Merkle Proof works by verifying that a user's information (address and amount) is included in a data structure called a Merkle tree.

Execution in the program:

1. Leaf Generation: For each claim, a leaf is generated by hashing the user's data (address and amount) with the keccak256 function. This creates a unique hash that represents that claim.

![](/imag-en/regconvertor/rc2.png){.align-right .img50}

<br>

2. Proof Verification: The *\_validateMerkleProof* function takes as input the user's address, the amount, the expected Merkle root, and an array of Merkle proofs. It verifies that the Merkle proof is valid for the user and the specified amount. ![](/imag-en/regconvertor/rc1.png){.align-right .img50}
<br>
[Link to corresponding code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L669)
<br>
<br>

3. **Merkle Tree Verification**: The *\_verifyAsm* function uses an assembly approach to traverse Merkle proofs. It takes each node of the proof and combines it with the leaf to reconstruct the hash until it reaches the Merkle root. If the reconstructed root matches the expected Merkle root, it means the claim is valid. ![](/imag-en/regconvertor/rc3.png){.align-right .img50}
<br>
[Link to the corresponding code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L719)
<br>

4. **Rejection of Invalid Claims**: If the verification fails, the contract rejects the claim by throwing an error, thus ensuring that only valid claims, which match the structure of the Merkle tree, are accepted.


## Claim Mode

The main claim modes in the contract are:

### User Claim (lines 176 to 200)

- Function: *claim*
- Description: This mode allows a user to claim REG tokens based on an amount in USDREG.
- Process:

1. The user calls the claim function with the amount they wish to claim and a Merkle proof.
2. The function verifies that the contract is not paused and that the timestamps are valid.
3. It verifies that the Merkle proof is valid for the user and the specified amount.
4. It verifies the amount already claimed by the user to avoid double claims.
5. If all checks pass, the contract calculates the amount of REG to issue and mints the REG tokens for the user.

![](/imag-en/regconvertor/rc4.png){.align-right .img50}

[Link to corresponding code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L176)
<br>
<br>

### Claim by Delegate (lines 302 to 336)

- Function: *claimByDelegate*
- Description: This mode allows a user to designate a delegate to claim tokens on their behalf.
- Process:

1. The user provides claim information (account address, amount, recipient) and a signature to prove authorization.
2. The function verifies that the contract is not paused and that the timestamps are valid. 3. It validates the signature to ensure that the delegate is authorized to act on the user's behalf.
4. As in the user-initiated claim method, the Merkle proof is validated, and the amount already claimed is verified.
5. If everything is valid, the contract mints the REG tokens for the recipient.

> The interface for this function sends the REG to the delegated address.
In a future update of the interface: it will be possible to delegate the claim without sending the tokens to the delegate, but to an address determined at the time of signing. {.is-warning}

![](/imag-en/regconvertor/rc5.png){.align-right .img50}
<br>
[Link to the corresponding code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L302)
<br>
<br>

### Automatic Claim (lines 552 to 640)

- Function: claimByAutoClaim
- Description: This mode allows users to automatically claim REG tokens without having to manually initiate the claim.
- Process:

1. The user calls the function with an array of claim structures.
2. The function checks that the contract is not paused and that the timestamps are valid. 3. For each claim, it checks whether self-claim is enabled for the recipient.
4. It validates the Merkle proof and verifies the amount already claimed.
5. REG tokens are minted for the recipient, and fees may apply if configured.

![](/imag-en/regconvertor/rc6.png){.align-right .img50}


[Link to the corresponding code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L552)
<br>
<br>

## REG Price Oracle

The REG Price Oracle contract is based on Chainlink code and includes:

- the smart contract [https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code)
- a Chainlink infrastructure to automate on-chain and off-chain price surveys across multiple sources and perform calculations aimed at preventing price manipulation. The calculation takes into account trading volumes, time spent at a price, differences between price sources, it creates a weighting to avoid manipulations, the update rate with the calculated value is updated onchain every 24 hours by a Chainlink automaton.

