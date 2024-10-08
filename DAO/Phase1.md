---
title: 3. Phase1, Simplified version
description: 
published: true
date: 2024-10-08T13:48:28.955Z
tags: 
editor: markdown
dateCreated: 2024-10-08T08:36:31.522Z
---

## **3.1. Explanation of the simplified version**

#### **⭐ For beginners**

Phase 1 of the RealToken DAO is a simplified version of the final system. It's like a trial period where we set up the foundations of the DAO. During this phase, we learn how governance works and we start making decisions together, but in a simpler way than in the future. The DAO's power is limited for now to minimize risks and allow for gradual learning, with the scope of decision-making power increasing over time.

#### **⭐⭐ For the initiated**

Phase 1 is a crucial step in deploying the RealToken DAO. It aims to:

1. Establish a basic governance structure,
2. Test voting and proposal mechanisms,
3. Identify community needs and challenges,
4. Begin making collective decisions on simple topics,
5. Prepare the ground for more advanced features in future phases,
6. Establish the operational foundations of the DAO,
7. Educate the community on DAO mechanisms,
8. Experiment with various governance functions and approaches,
9. Demonstrate the future potential of the DAO and REG.

This phase uses a limited set of smart contracts and features to facilitate adoption and learning. The technological choice was made so that this first phase allows for rapid and simple deployment with limited development costs. This phase will therefore allow for accumulating knowledge and establishing fundamental basic principles for the future of the DAO and the new version in phase 2 with the arrival of NFTs.

#### **⭐⭐⭐ For experts**

Phase 1 of the RealToken DAO is designed as a minimalist but functional implementation, with the following characteristics:

1. Simplified governance:
   - Basic voting mechanism based on the OpenZeppelin governor standard,
   - Evolving and less rigid proposal process to allow for greater participation and flexibility in exploration,
   - Quorum and voting thresholds regularly adjusted to adapt to participation,
   - No delegation of voting power,
   - Limitation on access to proposal creation to limit the risk of malicious proposals.
2. Basic smart contracts:
   - Governance contract, with voting and proposal functions, based on Open Zeppelin standards,
   - Simple treasury contract for managing DAO funds,
   - Incentive contract for managing voting incentive rewards,
   - PowerVotingRegistry contract, allowing registration of voting power for each REG holder.
3. Limited integration with the ecosystem:
   - Limited interactions with existing DApps (RealT remaining the trusted third party for DApps for the time necessary for the DAO to build competence),
   - Simple incentive mechanisms to encourage participation.
4. Security mechanisms:
   - Timelock on proposal executions, providing additional time to detect potential problems or errors in a proposal,
   - Limits on manageable treasury amounts,
   - Veto rights from RealT on extreme proposals or those endangering the DAO.
5. Data collection:
   - Metrics on member participation and engagement,
   - Continuous community feedback to inform future phases.

This phase will serve as a basis for the future evolution of the DAO, allowing for identification of necessary adjustments and features to develop for subsequent phases. By using this phase as a laboratory, we can experiment with various governance approaches and data collection before integrating them into the next version of the DAO with the arrival of NFTs. Although piloted by the trusted partner