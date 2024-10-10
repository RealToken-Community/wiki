---
title: 3. Phase1, Simplified version
description: 
published: true
date: 2024-10-10T12:43:23.494Z
tags: 
editor: markdown
dateCreated: 2024-10-08T08:36:31.522Z
---

## **3.1. Explanation of the simplified version of the DAO**

#### **⭐ For beginners**

Phase 1 of the RealToken DAO is a simplified version of the final system. It's like a trial period where we set up the basics of the DAO. During this phase, we learn how governance works and we start making decisions together, but in a simpler way than in the future. The DAO's power is limited for now to limit risks and allow progressive learning, the scope of decision-making power will increase over time.

#### **⭐⭐ For the initiated**

Phase 1 is a crucial step in the deployment of the RealToken DAO. It aims to:

1.  Set up a basic governance structure,
2.  Test voting and proposal mechanisms,
3.  Identify community needs and challenges,
4.  Start making collective decisions on simple topics,
5.  Prepare the ground for more advanced features in future phases,
6.  Establish the operational foundations of the DAO,
7.  Educate the community on DAO mechanisms,
8.  Experiment with various governance functions and approaches,
9.  Demonstrate the future potential of the DAO and REG.

This phase uses a limited set of smart contracts and features to facilitate adoption and learning. The technological choice was made so that this first phase allows for rapid and simple deployment with limited development cost. This phase will therefore allow accumulating knowledge and establishing the fundamental basic principles for the future of the DAO and the new version in phase 2 with the arrival of NFTs.

#### **⭐⭐⭐ For experts**

Phase 1 of the RealToken DAO is designed as a minimalist but functional implementation, with the following characteristics:

1.  Simplified governance:
    - Basic voting mechanism adapted from the OpenZeppelin governor standard,
    - More flexible and less rigid proposal process to allow greater participation and exploration flexibility,
    - Quorum and voting thresholds regularly adjusted to adapt to participation,
    - No delegation of voting power,
    - Limitation on access to proposal creation to limit the risk of malicious proposal creation.
2.  Basic smart contracts:
    - Governance contract, with voting and proposal functions, adapted from Open Zeppelin standards,
    - Simple treasury contract for DAO fund management,
    - Incentive contract for managing voting incentive rewards,
    - PowerVotingRegistry contract, allowing registration of the voting power of each REG holder.
3.  Limited integration with the ecosystem:
    - Limited interactions with existing DApps (RealT remaining the trusted third party for DApps for the time necessary for the DAO to gain competence),
    - Simple incentive mechanisms to encourage participation.
4.  Security mechanisms:
    - Timelock on proposal executions, giving additional time to detect potential problems or errors in a proposal,
    - Limits on manageable treasury amounts,
    - Veto rights from RealT on extreme proposals or those endangering the DAO.
5.  Data collection:
    - Metrics on member participation and engagement,
    - Continuous community feedback to inform future phases.

This phase will serve as a basis for the future evolution of the DAO, allowing identification of necessary adjustments and features to be developed for subsequent phases. By using this phase as a laboratory, we can experiment with various governance approaches and data collection before integrating these into the next version of the DAO with the arrival of NFTs. Although piloted by the trusted partner RealT, the DAO will remain independent and decisions will always be made by DAO members within the progressively expanded scope of application. Several points will therefore need to be debated between REG holders and RealT to establish the stages of expanding the DAO's scope of application to ensure the stability and security of the ecosystem.

## **3.2. Implementation process**

#### **⭐ For beginners**

The DAO is set up step by step. First, we create the basic tools for voting and making proposals. Then, we'll start using them to make simple decisions. As we go along, we'll learn and improve the system together.

#### **⭐⭐ For the initiated**

The implementation process for Phase 1 includes several key steps:

1.  Deployment of basic smart contracts (already done),
2.  Creation of a simple user interface for voting and proposing (Using an existing site: Tally),
3.  Training the community on the use of governance tools (Wiki),
4.  Presentation of the first projects the DAO can vote on (RealT has already prepared a series of proposals),
5.  Setting up the discussion and debate system for proposals (Forum),
6.  Launch of the first proposals (October),
7.  Launch of the first voting incentive campaign (October-November),
8.  Adjustment of governance parameters based on feedback,
9.  Gradual expansion of the DAO's scope of action.

#### **⭐⭐⭐ For experts**

The implementation process for Phase 1 is structured to maximize learning and adaptation:

1.  Technical infrastructure:
    - Deployment of smart contracts (governance, treasury, incentive, PowerVotingRegistry),
    - Restricted group testnet (already done),
    - Security audits and thorough testing (done internally by RealT),
    - Public testnet (already done),
    - Development of tools for powerVoting calculation (Done by RealT).
2.  Initial governance:
    - Definition of initial parameters (quorum, voting threshold, timelock: RealT),
    - Implementation of a multi-step proposal process (discussion, formalization, voting),
    - Implementation of security mechanisms, including RealT's veto right,
    - Limitation on access to proposal creation.
3.  Community engagement:
    - Education campaign on DAO mechanisms,
    - Creation of dedicated communication channels for discussions and feedback,
    - Organization of governance events to stimulate participation,
    - Incentive program.
4.  Iteration and optimization:
    - Continuous collection and analysis of participation metrics,
    - Regular adjustments to governance parameters,
    - Experimentation with different governance approaches,
    - Debate on various governance changes and approaches.
5.  Progressive expansion:
    - Definition of a plan to expand the DAO's scope of action,
    - Negotiations with RealT for the progressive transfer of responsibilities,
    - Preparation for the transition to Phase 2 with the integration of NFTs.

This process is designed to be flexible and adaptable, allowing the community to actively influence the evolution of the DAO while maintaining the stability and security of the ecosystem. It also gives REG holders the opportunity to understand and familiarize themselves with DAO mechanisms, create and discover vocations, generate ideas and new use cases. The progressive implementation of the DAO thus allows a natural transition and a progressive evolution towards the decentralization of the ecosystem.

## **3.3. Short and medium-term objectives**

#### **⭐ For beginners**

In the short term, we want everyone to understand how the DAO works and start participating according to their understanding and skills. In the medium term, we want to make more important decisions together and decentralize the governance of services offered by the DAO.

#### **⭐⭐ For the initiated**

Short-term objectives:

1.  Achieve a high participation rate in votes,
2.  Educate the community on DAO mechanisms (security, budget balance, etc.),
3.  Identify the first projects to fund or develop,
4.  Test and adjust governance parameters,
5.  Establish basic processes for governance.

Medium-term objectives:

1.  Progressively expand the DAO's scope of actions,
2.  Develop new use cases for REG,
3.  Improve governance tools based on feedback,
4.  Prepare for the transition to Phase 2 with the integration of NFTs.

#### **⭐⭐⭐ For experts**

Short-term objectives (3-6 months):

1.  Governance:
    - Achieve a stable quorum of 50% on votes,
    - Implement and test different proposal processes,
    - Establish an effective process for debating and refining proposals,
    - Elect the first community members who can create proposals,
    - Define the framework for a valid proposal for submission to votes.
2.  Community engagement:
    - Launch voting incentive campaigns with rewards,
    - Organize regular educational events on DAO governance,
    - Create a mentorship program for new active members,
    - Encourage initiatives beneficial to the DAO and RealToken ecosystem.
3.  Technical development:
    - Develop/refine analysis tools to monitor DAO health,
    - Integrate basic functionalities with the existing ecosystem.

Medium-term objectives (6-18 months):

1.  DAO expansion:
    - Negotiate and implement a plan to expand the scope of actions with RealT,
    - Develop more advanced governance mechanisms,
    - Create specialized committees for different aspects of the ecosystem,
    - Broader implementation of access to proposal creation.
2.  Innovation and development:
    - Launch community-driven research and development initiatives,
    - Experiment with new incentive and participation models,
    - Develop deeper integrations with the ecosystem,
    - Beginnings of phase 2 with the integration of NFTs.
3.  Preparation for Phase 2:
    - Design and test mechanisms for integrating NFTs into governance,
    - Develop a detailed roadmap for the transition to Phase 2,
    - Form working groups for each major aspect of the new phase,
    - Conduct thorough testing with the community for phase 2.
4.  Ecosystem and partnerships:
    - Establish strategic partnerships with other DeFi projects and DAOs,
    - Develop innovative use cases for REG beyond governance,
    - Integration of the first providers other than RealT.

These objectives aim to establish a solid foundation for the DAO, while preparing the ground for continuous expansion and innovation in the RealToken ecosystem.

## **3.4. How to participate in phase 1**

#### **⭐ For beginners**

To participate in the initial phase of the RealToken DAO:

1.  Follow official announcements on RealToken communication channels,
2.  Give your opinion in community discussions on the governance forum,
3.  Own REG tokens to have voting power,
4.  Participate in votes when they are open,
5.  Attend educational events to learn more about the DAO,
6.  Get informed and educated on the subject of DAOs.

#### **⭐⭐ For the initiated**

Here's how you can actively participate in the initial phase:

1.  Propose improvement ideas in governance forums,
2.  Participate in debates on proposals before votes on the governance forum,
3.  Vote regularly on proposals,
4.  Help educate new members on how the DAO works,
5.  Participate in incentive campaigns to earn rewards,
6.  Test new features and give your feedback,
7.  Contribute to the development of governance processes,
8.  Take initiatives and submit them to the community,
9.  Write, improve, translate, and share content about the DAO.

#### **⭐⭐⭐ For experts**

For in-depth participation in the initial phase:

1.  Governance:
    - Analyze proposals in depth and share your analyses,
    - Actively contribute to the development of metrics to evaluate DAO health,
    - Propose adjustments to governance parameters, based on data and argue with a short, medium, and long-term vision.
2.  Technical development:
    - Actively participate in Testnets,
    - Propose technical improvements for governance tools,
    - Contribute to the development of analysis tools for the DAO,
    - Continuous review and audit of smart contracts, stay informed about potential hacks and vulnerabilities.
3.  Community engagement:
    - Organize educational sessions for the community,
    - Create explanatory content on how the DAO works,
    - Actively participate in mentorship programs,
    - Explore new concepts for Live events, videos, articles, etc.
4.  Innovation:
    - Propose new argued use cases for REG,
    - Participate in working groups on future NFT integration,
    - Explore potential synergies with other DeFi projects.
5.  Preparation for Phase 2:
    - Contribute to the design of NFT integration mechanisms,
    - Participate in thorough testing of new features,
    - Help develop the roadmap for the transition to Phase 2.

Your active participation at all these levels will contribute to shaping the future of the RealToken DAO and ensuring its long-term success.

[Next Page](/en/DAO/Functioning)
