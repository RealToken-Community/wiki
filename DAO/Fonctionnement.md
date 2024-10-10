---
title: 4. Technical operation of the DAO REG
description: 
published: true
date: 2024-10-10T16:37:47.487Z
tags: 
editor: markdown
dateCreated: 2024-10-08T08:36:22.466Z
---

## **4.1. Main Smart Contracts**

#### **Smart contract addresses:**

- [Governor: 0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63](https://gnosisscan.io/address/0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63#code)
- [REGTreasuryDAO: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c](https://gnosisscan.io/address/0x3f2d192F64020dA31D44289d62DB82adE6ABee6c#code)
- [PowerVotingRegistry: 0x6382856a731Af535CA6aea8D364FCE67457da438](https://gnosisscan.io/address/0x6382856a731Af535CA6aea8D364FCE67457da438#code)
- [Incentive: 0xe1877d33471e37fe0f62d20e60c469eff83fb4a0](https://gnosisscan.io/address/0xe1877d33471e37fe0f62d20e60c469eff83fb4a0#code)

#### **⭐ For beginners**

The RealToken DAO operates using special computer programs called "smart contracts". These contracts automatically manage the DAO's rules, votes, and rewards. There are four main contracts:

1. The governance contract: This is the brain of the DAO. It manages proposals and votes,
2. The treasury contract: It manages the DAO's funds and applies a security delay before executing decisions,
3. The voting power contract: It calculates the voting power of each member,
4. The incentive contract: It distributes rewards to active DAO members who vote and lock their REG (if a period is activated by the DAO).

#### **⭐⭐ For the initiated**

The RealToken DAO is based on a set of interconnected smart contracts:

1. Governance contract: Based on the OpenZeppelin Governor standard, with minor modifications, it manages the lifecycle of proposals, the voting process, and the execution of on-chain decisions,
2. Treasury contract (REGTreasuryDAO): Implements a timelock mechanism to secure the execution of DAO decisions and manage its funds,
3. Voting power contract (PowerVotingRegistry): Records the voting power of REG holders based on their balance, boost, and activity, thus allowing greater flexibility than the classic token voting model, 1 token = 1 vote,
4. Incentive contract: Manages the distribution of rewards to encourage active participation in governance, for this purpose, voting and locking REG for a determined duration (epoch) is required.

These contracts interact to ensure transparent and secure operation of the DAO while encouraging active participation in governance.

#### **⭐⭐⭐ For experts**

The technical architecture of the RealToken DAO is composed of several key smart contracts:

##### **4.1.1. Governance contract**

- Based on OpenZeppelin Governor with custom modifications,
- Manages the complete lifecycle of proposals: creation, voting, and execution,
- Integrates interactions with the timelock to increase execution security,
- Implements access controls for proposal creation, allowing activation of 4 different modes to authorize proposals to be created,
- Interacts directly with the PowerVotingRegistry to create snapshots of voting powers based on values recorded in the PowerVotingRegistry.

##### **4.1.2. Treasury contract (REGTreasuryDAO)**

- Based on OpenZeppelin's TimelockControllerUpgradeable,
- Implements a timelock mechanism to add a security delay before executing on-chain decisions,
- Manages the DAO's funds and controls their use,
- Integrates specific roles (UPGRADER_ROLE) for managing contract upgrades,
- Uses the UUPS (Universal Upgradeable Proxy Standard) pattern to allow future upgrades,
- Interacts with external contracts called by proposals.

##### **4.1.3. Voting power contract (PowerVotingRegistry)**

- Records voting power based on REG balance and other factors (e.g., holding duration, lock, liquidity pool, etc.),
- Implements self-delegation logic to simplify voting power activation.

##### **4.1.4. Incentive contract**

- Integrates state logic that modifies possible actions, the complete cycle is called "epoch", for each epoch voting and locking REG is required to receive rewards,
- Distributes rewards to active governance participants (who vote),
- Uses locking mechanisms to encourage long-term commitment (locking for the duration of the epoch),
- Integrates reward calculations based on governance activity (votes and REG locking).

These contracts are designed to be modular and scalable, allowing for future updates and improvements without disrupting the entire governance system. The REGTreasuryDAO contract, in particular, adds an extra layer of security by imposing a delay between proposing an action and its execution, thus allowing the community to react if necessary.

Eventually, all contracts in the RealToken ecosystem will be controlled, upgradable only by the REGTreasuryDAO contract via proposals, only a few emergency functions can be executed outside the DAO cycle by a security committee that will have rights limited to emergency actions to secure the contracts.

## **4.2. Voting and proposal mechanisms**

#### **⭐ For beginners**

The RealToken DAO allows you to participate in important decisions of the ecosystem of applications linked to the DAO. Here's how it works:

1. On the DAO forum, members can propose ideas to improve the DAO and the ecosystem,
2. A debate takes place around the proposals to measure the feasibility and interest of the proposals by the community,
3. If a proposal gets enough votes, it is transformed into a proposal and submitted to the DAO vote,
4. DAO members can vote for or against the proposal,
5. If the proposal is approved, it is executed by the REGTreasuryDAO.

#### **⭐⭐ For the initiated**

The voting and proposal system of the RealToken DAO includes:

1. The DAO forum: Where members can propose ideas and debate to measure the interest of proposals,
2. During the debate period, the community must measure the interest and feasibility of proposals,
3. If a proposal is deemed feasible and relevant, a concrete proposal is worked on with the establishment of all parameters related to the proposal, such as on-chain actions to be executed, development cost, gains for the DAO, etc...
4. Proposal creation: Process controlled by access rules defined in the governance contract, depending on the parameters there may be restrictions on proposal creation, this is to avoid governance attacks, and allow a progressive implementation of the DAO's operating rules in complete safety,
5. Proposal lifecycle: Creation, voting period, execution (if approved).
6. Timelock mechanism: Security delay before executing approved decisions.

#### **⭐⭐⭐ For experts**

The voting and proposal mechanisms are implemented as follows:

1. DAO forum:
   - Where users can propose ideas and debate to measure the interest of proposals,
   - A debate takes place around the proposals to measure the feasibility and interest of the proposals by the community or one of the DAO's service providers,
   - If a proposal is deemed feasible and relevant, a concrete proposal is worked on with the establishment of all parameters related to the proposal, such as on-chain actions to be executed, development cost, gains for the DAO, impact and risk study, security audit needs or not, etc...
2. Proposal creation:
   - Controlled by the governance contract,
   - Four authorization modes are possible for proposal creation, these proposal modes aim to optimize the DAO's risk management, and limit fund exposure, with a medium-term decentralization objective depending on the DAO's maturity,
   - On-chain registration of the proposal with all codes allowing the execution of the proposal if approved,
   - Interaction with the PowerVotingRegistry to create snapshots of voting powers.
3. Voting process:
   - Based on the OpenZeppelin Governor standard with custom modifications,
   - Use of voting power recorded in the PowerVotingRegistry,
   - Voting options: for, against, or abstain,
   - Only members registered in the PowerVotingRegistry can vote,
   - A 1-day delay is applied between the start of the vote and the validation of the proposal creation transaction (for security, for possible cancellation),
   - Voting takes place over a period determined by the governance contract, generally 7 days,
   - Voters verify that the proposal conforms to the description and exchanges on the DAO forum (verification of on-chain execution code).
4. Voting power calculation
   - Recorded in the PowerVotingRegistry,
   - Takes into account REG balance, holding duration, locking, and potentially other factors, according to the calculation algorithm validated by the DAO,
   - Implements self-delegation logic to simplify voting power activation.
5. Proposal execution:
   - Use of REGTreasuryDAO as timelock to add a security delay,
   - On-chain execution of approved actions after the delay,
   - Possibility of cancellation during the timelock period if necessary, by a security group.
6. Incentive system:
   - Managed by the incentive contract,
   - Rewards based on participation in votes and REG locking,
   - Epoch system to structure reward periods,
   - Activated by the DAO which defines the duration of epochs and associated rewards.
7. Proposal mode:
   - ProposerWithRole: The goal is to strictly limit the addresses that can submit a vote, to prevent spam and abusive proposals. This mode will be activated only during the first weeks or months of the initial governance deployment (version 1), to establish a solid foundation before expanding the number of proposal authors,
   - ProposerWithVotingPower: This mode requires the proposal author to have a minimum voting power. Without this power, no proposal can be created. This is the ultimate step of decentralization, where all those with sufficient voting power can submit proposals. There are no more privileged roles; all holders follow the same rules,
   - ProposerWithRoleAndVotingPower: This mode combines two restrictions, the proposal author must have a specific role and a minimum voting power. If one of the conditions is not met, the proposal will fail. This mode expands proposals to the most engaged members of the community, requiring a minimum REG balance to ensure their commitment and financial stakes. They must be elected and own enough REG. This allows starting decentralization, testing processes, and limiting risks,
   - ProposerWithRoleOrVotingPower: This mode allows proposal creation if the proposal author has either the required role or a minimum voting power. If neither of these conditions is met, the proposal will fail. This is the penultimate step of governance decentralization. This mode requires a significant number of REG to freely propose improvements, without going through a specific role. This allows anyone to propose improvements by committing a significant number of REG, while maintaining proposal authors with a specific role, who don't need minimum REG. The role can be removed in case of non-compliance with processes.
8. Delegation:
   - Voting power delegation has been removed to allow activation of incentives for voters and REG lockers,
   - Users cannot delegate their voting power to other addresses, but benefit from default self-delegation,
   - In a v2, delegation will return and will even be an important part of the system.

These mechanisms are designed to ensure fair, secure, and incentivized governance, while allowing future evolution of the system.

In the first weeks/months, the DAO is implemented with restrictions to allow the establishment of operating rules, experiment with processes, and limit risks.

To give a good vision of the evolution stages, a debate will have to take place to determine the thresholds triggering a higher level of decentralization, giving greater autonomy and freeing itself from the trust given to various stakeholders.

## **4.3. Incentive and reward system**

#### **⭐ For beginners**

The RealToken DAO rewards you for your active participation. Here's how it works:

1. You earn rewards in various forms by voting on proposals,
2. You can increase your rewards by locking your REG tokens for a certain period,
3. Participation in certain liquidity pools or others in the RealToken ecosystem can give you additional rewards,
4. Rewards can be in the form of REG, ERC20, or even as a boost on voting power,
5. The DAO decides on rewards and parameters of the incentive system.

#### **⭐⭐ For the initiated**

The RealToken DAO's incentive system is designed to encourage active participation:

1. The DAO defines the rewards and parameters of the incentive system, the type and amounts of rewards,
2. Rewards based on participation in votes and REG locking, participation in certain liquidity pools or others in the RealToken ecosystem can give you additional rewards,
3. Epoch system to structure reward periods and optimize participation,
4. Automatic on-chain attribution of rewards in stable coin claimable at the end of each epoch,
5. Possibility for the DAO to adjust the parameters of the incentive system to optimize participation and respond to the DAO's evolution.

#### **⭐⭐⭐ For experts**

The incentive and reward system is implemented as follows:

1. Incentive contract:
   - Manages the system state (active/inactive) and epoch cycles,
   - Calculates rewards based on voting activity and REG locking,
   - Automatically assigns rewards at the end of each epoch.
2. Locking mechanism:
   - Users can lock their REG for the duration of an epoch in the incentive contract,
   - Locking can increase voting power and potential rewards according to parameters defined by the DAO,
   - Locking allows marking a long-term commitment from the voter, decreases REG liquidity, and limits governance attack risks.
3. Adjustable parameters:
   - Duration of epochs,
   - Quantity of rewards per epoch,
   - Type of rewards,
   - These parameters are applied by the DAO in the proposal for activating an epoch,
   - For voting power boosts, the DAO can adjust as needed: the formula and weight given in the powerVoting calculation system that generates voting powers.
4. Integration with PowerVotingRegistry:
   - REG locking affects the recorded voting power,
   - Synergy between governance participation and rewards.
5. Security and scalability:
   - Upgradable contract to allow future improvements,
   - Pause mechanisms in case of emergency.
6. Reward calculation:
   - Accounts for the total number of proposals issued during an epoch,
   - Accounts for the number of votes made by each voter during the epoch,
   - Weighted by the quantity of locked REG.

```plaintext
userBonus = (userState.depositAmount * userState.voteAmount * epochState.totalBonus) / epochState.totalWeights;
```

- `userState.depositAmount`: The amount deposited by the user for this epoch in REG,
- `userState.voteAmount`: The number of votes made by the user during this epoch,
- `epochState.totalBonus`: The total amount of bonus allocated for this epoch,
- `epochState.totalWeights`: The total sum of weights for all users in this epoch.

This formula is applied in the contract at the following lines:

[REGIncentiveVault.sol](https://gnosisscan.io/address/0x4b79755d1ea8937c027408e3aa72d69a260f6237#code#F1#L342) from line 342 to 346.

The total weights are incremented by the amount deposited by the user at each of their votes (cf line 245 of the contract).

This system aims to create a virtuous circle of engagement in governance, rewarding active participation while strengthening the long-term stability of the RealToken ecosystem.

The reward mechanism will be entirely revised with the implementation of V2 governance using NFTs to improve the incentive system, allow greater granularity, and precision of actions to encourage based on the DAO's needs and holder profiles.

New types of rewards will be introduced such as dark matter from Cityzen NFTs, support points for votes allowing the highlighting of content related to Activity NFTs, etc...

The RealToken ecosystem is designed to have numerous tools for optimizing participation in votes, and encouraging various actions and contributions that are beneficial to the DAO while allowing precise control of the DAO's finances to avoid squandering the treasury or devaluing the REG token.

[Next Page](/en/DAO/Guide_Pratique)
