---
title: 8. FAQ
description: 
published: true
date: 2024-10-10T16:02:32.189Z
tags: 
editor: markdown
dateCreated: 2024-10-10T08:55:44.366Z
---

## 8.1 DAO Basics ⭐

> Q: What is REG and how to obtain it?  
> {.is-question}

> A: REG (RealToken Ecosystem Governance) is the governance token of the RealToken DAO. It can be obtained by participating in certain activities of the RealToken ecosystem: by purchasing RealTokens on [realt.co](http://realt.co) or by buying it on decentralized exchanges like 1inch on Gnosis Chain.
> {.is-result}

> Q: How to participate in DAO votes?  
> {.is-question}

> A: To participate in votes, you must hold REG tokens at the time of the snapshot, they can be in: your wallet, liquidity pool, locking vault, etc. Connect to the governance interface with the wallet containing or that has locked REG at the time of the snapshot, then you can vote on active proposals using the snapshot.
> {.is-result}

> Q: How can I earn bonuses?  
> {.is-question}

> A: You can earn bonuses by participating in DAO votes during periods when the DAO has activated the incentive mechanism. You also need to lock REG in the incentive vault and vote for as many proposals as possible during the period to maximize your earnings.
> {.is-result}

> Q: Who can propose ideas?  
> {.is-question}

> A: Everyone can propose ideas. To do this, you need to be active in the community, create a topic on the governance forum, and get enough support to trigger the proposal creation process.
> {.is-result}

> Q: Who can create proposals?  
> {.is-question}

> A: Only people with the Proposer role can create proposals in the first phase of v1. This restriction ensures that proposals are of quality, well-thought-out, and prevents governance attacks.
> {.is-result}

> Q: What topics can the DAO vote on?  
> {.is-question}

> A: The DAO can vote on topics related to the RealToken ecosystem. Initially, topics are limited to the governance methodology and incentive periods. For other topics, it will be necessary to determine if the resources and priorities are current for the DAO. To do this, open a topic on the governance forum and get enough support to trigger the proposal creation process.
> {.is-result}

> Q: Who develops applications and new features for the DAO?  
> {.is-question}

> A: RealT is the main provider for the DAO. It develops applications, new features, and manages the RealToken ecosystem under the DAO's mandate. Other providers may provide services to the DAO subject to DAO acceptance.
> {.is-result}

> Q: Can the DAO spend all the treasury funds?  
> {.is-question}

> A: In principle yes, the DAO has full control over the treasury funds. However, inappropriate spending could have significant consequences on the future of the DAO, the value of REG, and the continuity of DApps. It is therefore the responsibility of DAO members to have in-depth reflections before voting on expenses and to plan priorities so that the DAO can endure over time and distribute a share of its revenues just like a company that distributes dividends.
> {.is-result}

> Q: Why do we need to provide an email to participate in the governance forum?  
> {.is-question}

> A: Like any platform and to limit spam, single-use addresses are blocked. A valid address helps limit spam, create an identifiable account, and communicate information/notifications related to the forum. The community commits not to use emails for purposes other than those directly related to the governance forum and will never force you to link an email with an on-chain address. For governance V2, the mechanism will evolve to allow users to create an account on the forum directly identified by Citizen, Activity, and Collector NFTs, which will act as identity cards for the Realtoken ecosystem.
> {.is-result}

<br>

## 8.2. Advanced Mechanisms ⭐⭐

> Q: How does the REG locking mechanism work?  
> {.is-question}

> A: In v1, locking is a condition to be able to obtain bonuses with participation in votes. Each session (epoch) is activated and defined by the DAO. There is a first subscription phase during which you can deposit and withdraw your REG, followed by a locking period during which you can no longer withdraw REG and must vote. At the end of the locking period, REG are released and you can withdraw them or leave them in the vault. They will be automatically locked for the next session at the time of token locking (so you can withdraw until the end of the next subscription period).
> {.is-result}

> Q: How are addresses authorized to create proposals chosen?  
> {.is-question}

> A: During the first weeks of v1 operation, authorized addresses will only be RealT addresses, giving time for the community to organize to elect trusted people within the community to become Proposers.
> {.is-result}

> Q: What is the purpose of the Proposer role?  
> {.is-question}

> A: Proposers are trusted people within the community whose role is to ensure that proposals submitted on the governance forum are of quality, relevant, and well-thought-out, that they respect the methodology defined by the DAO for proposal creation. They can also be called upon for specific tasks related to governance and must create on-chain proposals.
> {.is-result}

> Q: Can the DAO force a change without RealT's consent?  
> {.is-question}

> A: Proposals supported by a large majority of the DAO can be forced. However, at present, the DAO does not concretely own the DApps created by RealT for the future DAO. There will therefore need to be a formal takeover of responsibility for the DApp or DApps by the DAO, which may result in a payment to RealT for the service provided.
> The DAO cannot impose or force things that are under the responsibility of RealT or other providers.
> {.is-result}

> Q: Which DApps can the DAO act on?  
> {.is-question}

> A: Currently, the DApps on which the DAO can act are: governance contracts, the REG locking vault, the DAO treasury contract. Other DApps will be progressively added, and new contracts developed by RealT, funded by the DAO, will be directly under the control and responsibility of the DAO.
> {.is-result}

> Q: Why doesn't the DAO have full control over DApps developed by RealT so far?  
> {.is-question}

> A: The DAO must first organize itself, define processes, gain maturity, and acquire experts who can exercise the roles of control and management of DApps. Controlling a DApp requires expertise and understanding of how smart contracts and blockchain work. This is why, as a provider, RealT has so far assumed responsibility for these DApps for the DAO to ensure the continuity and stability necessary to last over time.
> {.is-result}

> Q: Which liquidity pools are supported for power voting calculation?  
> {.is-question}

> A: Most liquidity pools are supported by the balance and voting power calculation tool, however this requires integration into the tools for them to be recognized. To be integrated, there must be a TheGraph graph for the pool to provide data, and a request must be made to RealT to implement the integration (it will be possible to contribute to the tool on Github in the near future).
> {.is-result}

<br>

## 8.3. Technical Aspects ⭐⭐⭐

> Q: Why not use REG as a governance token but use the `PowerVotingRegistry`?
> {.is-question}

> A: REG is a token used for power voting calculation. Voting powers are calculated off-chain by RealT and recorded on the `PowerVotingRegistry`. This approach allows not penalizing participants in liquidity pools or other services that use REG. Thus, REG can have multiple use cases, in addition to voting, without penalizing voting power. Moreover, the DAO can modify the power voting calculation mechanisms by changing the off-chain algorithm, without having to modify REG or other smart contracts, which allows it to grant a voting boost according to its objectives. For example: to support liquidity provision on one or more DEXs, the DAO can decide to count locked REG in the pool as well as the liquidity deposited in return as additional REG, or even grant more voting power for a REG deposited in a liquidity pool. The DAO could also decide that REG on wallets, which therefore do not benefit the ecosystem, could be worth less than one unit of voting power. For example, one REG on a wallet could give 0.5 voting power, versus 1 REG in a liquidity pool which could give 1.2 voting power.
> {.is-result}

> Q: Why was the delegation mechanism deactivated?  
> {.is-question}

> A: The delegation mechanism was deactivated for reasons of simplification of the implementation of v1 and to encourage more participation in the elaboration of the governance bases. Moreover, with the incentive mechanism and the low revenues of the DAO currently, it would have been counterproductive to distribute rewards without a counterpart from REG holders towards the DAO.
> {.is-result}
