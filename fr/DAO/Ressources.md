---
title: 7. Ressources et support
description: 
published: true
date: 2024-10-01T13:35:13.887Z
tags: 
editor: markdown
dateCreated: 2024-09-26T12:17:57.562Z
---

## 7.1. Documentation technique

### 7.1.1. Smart Contracts v1 ⭐⭐⭐

La documentation des smart contracts est essentielle pour comprendre le fonctionnement interne de la DAO RealToken. Elle comprend :

#### Spécifications techniques des principaux contrats :

1. **REGGovernor.sol** : Contrat de gouvernance principal, ce contrat gère le processus de gouvernance, y compris la création de propositions, le vote et l'exécution.  
    Fonctions principales non standard ajoutées :
    
    - **`function setProposerMode(ProposerMode proposerMode) external `**
    
    	**Description :** Permet à la gouvernance de définir le mode de proposant, déterminant les critères que les adresses doivent respecter pour créer des propositions.    
    	**Paramètres :**    
    	**proposerMode** : Le nouveau mode de proposant à définir, qui peut être l'un des suivants :
		- **`ProposerWithRole`** : Seules les adresses avec le rôle **`PROPOSER_ROLE`** peuvent proposer.
		-   **`ProposerWithVotingPower`** : Seules les adresses avec une puissance de vote suffisante peuvent proposer.
		-   **`ProposerWithRoleAndVotingPower`** : Seules les adresses avec le rôle **`PROPOSER_ROLE`** et une puissance de vote suffisante peuvent proposer.
		-    **`ProposerWithRoleOrVotingPower`** : Les adresses avec soit le rôle **`PROPOSER_ROLE`**, soit une puissance de vote suffisante peuvent proposer.
    
    	**Contrôle d'accès :** Ne peut être appelée que via une proposition de gouvernance réussie (`onlyGovernance`).    
    	**Événement Émis :** `SetProposerMode`
      
    -  **`function setIncentiveEnabled(bool status) external `**    
    	**Description :** Active ou désactive le mécanisme d'incitation, qui enregistre les votes dans le coffre d'incitation pour récompenser les participants.    
    	**Paramètres :**  `status` Un booléen indiquant s'il faut activer (`true`) ou désactiver (`false`) le mécanisme d'incitation.    
    	**Contrôle d'accès :** Ne peut être appelée que via une proposition de gouvernance réussie.   
    	**Événement Émis :** `SetIncentiveEnabled`
      
    - **`function setRegIncentiveVault(IREGIncentiveVault regIncentiveVault) external `**    
    	**Description :** Définit l'adresse du contrat du coffre d'incitation REG utilisé pour enregistrer les votes et distribuer les incitations.    
    	**Paramètres :** `regIncentiveVault` L'adresse du nouveau contrat du coffre d'incitation REG. 
    	**Contrôle d'accès :** Ne peut être appelée que via une proposition de gouvernance réussie.   
    	**Événement Émis :** `SetRegIncentiveVault`
    
    - **`function getProposerMode() external view returns (ProposerMode) `**    
    	**Description :** Renvoie le mode de proposant actuel, indiquant les critères requis pour qu'une adresse puisse créer des propositions.    
    	**Retourne :** Le `ProposerMode` actuel.
      
    - **`function getIncentiveEnabled() external view returns (bool) `**    
    	**Description :** Indique si le mécanisme d'incitation est actuellement activé.         
    	**Retourne :**  `true` si le mécanisme d'incitation est activé, `false` sinon.
    
    - **`function getRegIncentiveVault() external view returns (IREGIncentiveVault) `**    
    	**Description :** Renvoie l'adresse du contrat du coffre d'incitation REG.    
    	**Retourne :** L'adresse du contrat `IREGIncentiveVault`.
    
    - **`function cancelByAdmin(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) external returns (uint256)`**  
   	 **Description :** Permet à un administrateur avec le rôle `CANCELLER_ROLE` d'annuler une proposition correspondant aux paramètres donnés.    
   	 **Paramètres :**    
  	  -   `targets` : Un tableau des adresses cibles pour les actions de la proposition.
 	   -   `values` : Un tableau des valeurs ETH à envoyer avec chaque action.
 	   -   `calldatas` : Un tableau des données d'appel pour chaque action.
 	   -   `descriptionHash` : Le hachage keccak256 de la description de la proposition.
    
    	**Retourne :** L'ID de la proposition annulée. 
    	**Contrôle d'accès :** Restreint aux adresses avec le rôle `CANCELLER_ROLE`.
    
2. **REGTreasuryDAO.sol** : Gestion de la trésorerie de la DAO, ce contrat gère les fonds de la DAO et exécute les transactions approuvées après un délai de timelock de sécurité.  
    Il n'y a pas de fonction principale non standard ajouté.  
     
3. **REGIncentiveVault.sol** : Système d'incitation et de récompenses, ce contrat gère le calcul et la distribution des récompenses aux participants de la DAO si activé par la DAO.  
    Contrat entièrement créé par RealT, les fonctions principales sont :    
   - **`function setNewEpoch(uint256 subscriptionStart,uint256 subscriptionEnd,uint256 lockPeriodEnd,address bonusToken,uint256 totalBonus ) external onlyRole(DEFAULT_ADMIN_ROLE)`**
			**Description :** Définit un nouvel epoch (période) pour le programme d'incitation. Cette fonction initialise un nouvel epoch avec les paramètres spécifiés. 
			**Paramètres :**    
   	 -   `subscriptionStart` : Timestamp du début de la période de souscription.
  	 -   `subscriptionEnd` : Timestamp de la fin de la période de souscription.
 	   -   `lockPeriodEnd` : Timestamp de la fin de la période de verrouillage.
 	   -   `bonusToken` : Adresse du token utilisé comme récompense.
 	   -   `totalBonus` : Montant total des tokens de bonus à distribuer pour cet epoch.
    
    	**Contrôle d'accès :** Seul le rôle `DEFAULT_ADMIN_ROLE` peut appeler cette fonction.
    	**Événement Émis :** `SetNewEpoch`
   
   - **`function deposit(uint256 amount) public whenNotPaused;`**
 			**Description :** Permet aux utilisateurs de déposer des tokens REG dans le vault pendant la période de souscription pour participer au programme d'incitation.
      **Paramètres :** `amount` : Montant de tokens REG à déposer.    
   		**Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause et pendant la période de souscription.    
    	**Événement Émis :** `Deposit`
      
    - **`function depositWithPermit(uint256 amount, uint256 deadline, uint8 v, bytes32 r,     bytes32 s ) external whenNotPaused;`**    
		 	 **Description :** Permet aux utilisateurs de déposer des tokens REG en utilisant l'approbation EIP-2612 (permit), ce qui permet le dépôt en une seule transaction sans approbation préalable.
        **Paramètres :**    
		- 	`amount` : Montant de tokens REG à déposer.
		- 	`deadline` : Timestamp après lequel la signature n'est plus valide.
		-  	 `v, r, s` : Composants de la signature pour l'approbation.
    
  	  **Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause et pendant la période de souscription.   
  	  **Événement Émis :** `Deposit`
  
   - **`function withdraw(uint256 amount) external whenNotPaused;`**    
			 **Description :** Permet aux utilisateurs de retirer leurs tokens REG déposés après la fin de la période de verrouillage.    
   		 **Paramètres :** `amount` : Montant de tokens REG à retirer.
   		 **Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause et après la fin de la période de verrouillage.
  	  **Événement Émis :** `Withdraw`
  
   - **`function recordVote(address user, uint256 proposalId) external onlyGovernance;`** 
    	**Description :** Enregistre le vote d'un utilisateur sur une proposition pendant la période de verrouillage afin de calculer les récompenses d'incitation.
 	    **Paramètres :**
    
   	 -   `user` : Adresse de l'utilisateur qui a voté.
   	 -   `proposalId` : Identifiant de la proposition sur laquelle l'utilisateur a voté.
    
    	**Contrôle d'accès :** Seul le contrat de gouvernance peut appeler cette fonction.
      **Événement Émis :** `RecordVote` ou `RecordVoteNotActive`
  
   - **`function calculateBonus(address user) external view returns (address[] memory, uint256[] memory)`**
     **Description :** Calcule le montant du bonus qu'un utilisateur peut réclamer pour chaque epoch passé.
     **Paramètres :**
   	 -   `user` : Adresse de l'utilisateur pour lequel calculer le bonus.
     
	    **Retourne :**
		-  `address[]` : Tableau des adresses des tokens de bonus.
 	  -   `uint256[]` : Tableau des montants de bonus correspondants.
  
   - **`function claimBonus() public whenNotPaused;`**
     **Description :** Permet aux utilisateurs de réclamer leurs bonus accumulés pour tous les epochs éligibles.
     **Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause.
     **Événement Émis :** `ClaimBonus`
  
   - **`function getRegGovernor() external view returns (address)`**
      **Description :** Renvoie l'adresse du contrat de gouvernance REG associé.
   
   - **`function getRegToken() external view returns (IERC20)`** 
   	 **Description :** Renvoie l'adresse du token REG utilisé par le vault.
    
   - **`function getCurrentTotalDeposit() external view returns (uint256)`**
      **Description :** Renvoie le montant total de tokens REG actuellement déposés dans le vault.
    
   - **`function getCurrentEpoch() external view returns (uint256)`**
     **Description :** Renvoie le numéro de l'epoch actuel.
    
   - **`function getCurrentEpochState() external view returns (EpochState memory)`**
     **Description :** Renvoie les informations sur l'état de l'epoch actuel.
    
   - **`function getEpochState(uint256 epoch) external view returns (EpochState memory)`**
     **Description :** Renvoie les informations sur l'état d'un epoch spécifique.
     **Paramètres :**`epoch` Numéro de l'epoch pour lequel obtenir les informations.
    
   - **`function getUserEpochState(address user, uint256 epoch) external view returns (UserEpochState memory)`**
 	   **Description :** Renvoie les informations sur l'état d'un utilisateur pour un epoch spécifique.
     **Paramètres :**
 	   -   `user` : Adresse de l'utilisateur.
  	 -   `epoch` : Numéro de l'epoch.
    
   - **`function getUserGlobalState(address user) external view returns (UserGlobalState memory)`**
     **Description :** Renvoie les informations globales sur l'état d'un utilisateur.
     **Paramètres :**
  	 -   `user` : Adresse de l'utilisateur.
             
5. **REGPowerVotingRegistry.sol** : Enregistrement des pouvoirs de vote, ce contrat enregistre le pouvoir de vote des participants calculé off-chain par RealT selon l'algorithme défini et validé par la DAO.  
Contrat entièrement créé par RealT, les fonctions principales sont :
	- `registerVotingPower(VotingPower[] calldata votingPower) external override onlyRole(REGISTER_ROLE)` Vous noterez que pour des raisons de compatibilité avec les standards et le fonctionnement du contrat Governor, le contrat PowerVotingRegistry est basé sur le standard ERC20 avec une modification des comportements des fonctions de base comme :

		- transfer, transferFrom, approve, etc. sont override pour renvoyer false
		- delegate est override pour renvoyer une erreur
    
	- `REGVotingPowerRegistryErrors.DelegateToOtherNotAllowed()` si un utilisateur essaie de déléguer son vote à un autre utilisateur.
               

#### Guides d'interaction avec les contrats :

-   Méthodes principales :
    -   `propose()` dans REGGovernor pour créer une proposition,
    -   `castVote()` dans REGGovernor pour voter sur une proposition,
    -   `queue()` dans REGGovernor pour mettre en file d'attente une proposition,
    -   `execute()` dans REGTreasuryDAO pour exécuter une proposition approuvée.  
         
-   Événements importants :
    -   `ProposalCreated` émis lors de la création d'une proposition,
    -   `VoteCast` émis lorsqu'un vote est enregistré,
    -   `callExecuted()` émis lorsqu'une action de la proposition est exécutée.  
         

#### Diagrammes d'architecture :

\[Insérer ici un diagramme montrant les interactions entre les contrats\]  
 

#### Rapports d'audit de sécurité :

Il n'y a pas de rapport d'audit de sécurité pour le moment. Les contrats étant proches des versions standards (déjà audité) et la gouvernance étant exercée avec des restrictions de droits, il a été jugé plus pertinent de conserver les ressources pour des audits futurs de la v2. La v1 étant une version provisoire expérimentale destinée à initialiser la DAO et donc le champ d'action est limité.

### 7.1.2. Mécanismes de gouvernance ⭐⭐

Cette section détaille les processus de gouvernance de la DAO :

- Cycle de vie des propositions :
    -   Discussion et rédaction de la proposition sur le forum de gouvernance,
    -   Création et soumission,
    -   Période de vote,
    -   Exécution et timelock.
- Calcul du pouvoir de vote :
    -   Formules utilisées off-chain par RealT,
    -   Facteurs influençant le pouvoir de vote (verrouillage, durée de détention, usage).
- Système d'incitation :
	- Mécanisme de distribution des récompenses
	- Calcul des bonus basé sur la participation aux votes et verrouillage des REG

### 7.1.3. Interfaces utilisateur ⭐

Les interactions se font principalement avec les interfaces suivantes :

- Discussions de Gouvernance ([URL](https://forum.realtoken.community)):
    -   Forum de gouvernance,
    -   Hébergé et géré par la communauté,
    -   Permet de discuter des idées, déterminer l'intérêt, les priorités, et de préparer les propositions.
- Interface de vote ([URL](https://www.tally.xyz/gov/realtoken-ecosystem-governance)):
    -   Utilisation de la plateforme Tally comme interface de vote,
    -   Accès aux fonctions principales de vote, visualisation, résultat, vote, création de proposition, statut des propositions,
    -   L'usage de Tally permet de gagner plusieurs mois de développement,
    -   Implique quelques contraintes dans le fonctionnement et les informations affichées.
- Interface d'incentive ([URL](https://vote.realtoken.network)):
    -   Interface dédiée créée par RealT,
    -   Permet de déposer des REG, retirer des REG, réclamer des récompenses.

## 7.2. Canaux de communication

### 7.2.1. Canaux officiels ⭐

- Site web officiel DAO : SOON
- Twitter : @RealTokenDAO

### 7.2.2. Ressources pour les développeurs ⭐⭐⭐

-   GitHub : Dépôt des smart contracts → [Governance, incentive, treasury](https://github.com/real-token/reg-governance-core)
-   GitHub : interfaces incentive → [https://github.com/real-token/voting-front/tree/incentive](https://github.com/real-token/voting-front/tree/incentive)

## 7.3. FAQ

### 7.3.1. Bases de la DAO ⭐

> Q : Qu'est-ce que le REG et comment l'obtenir ?  
{.is-question}

> R : Le REG (RealToken Ecosystem Governance) est le token de gouvernance de la DAO RealToken. Il peut être obtenu en participant à certaines activités de l'écosystème RealToken : en achetant des RealTokens sur [realt.co](http://realt.co) ou en l'achetant sur des échanges décentralisés comme 1inch sur Gnosis Chain.
{.is-result}

> Q : Comment participer aux votes de la DAO ?  
{.is-question}

> R : Pour participer aux votes, vous devez détenir des tokens REG au moment du snapshot, ils peuvent être dans : votre portefeuille, pool de liquidité, vault de verrouillage, etc. Connectez-vous à l'interface de gouvernance avec le portefeuille contenant ou qui a lock des REG au moment du snapshot, puis vous pourrez voter sur les propositions actives utilisant le snapshot.
{.is-result}

> Q : Comment puis-je gagner des bonus ?  
{.is-question}

> R : Vous pouvez gagner des bonus en participant aux votes de la DAO durant les périodes pour lesquelles la DAO à activé le mécanisme d'incitation. Il vous faut aussi verrouiller des REG dans le vault d'incentive et voter au maximum de propositions durant la période pour maximiser vos gains.
{.is-result}

> Q : Qui peut proposer des idées ?  
{.is-question}

> R : Tout le monde peut proposer des idées. Il faut pour cela être actif dans la communauté, créer un sujet sur le forum de gouvernance, et obtenir suffisamment de soutien pour déclencher le processus de création des propositions.
{.is-result}

> Q : Qui peut créer des propositions ?  
{.is-question}

> R : Seules les personnes ayant le rôle Proposer peuvent créer des propositions dans la première phase de la v1. Cette restriction permet de s'assurer que les propositions sont de qualité, réfléchies, et d'éviter les attaques de gouvernance.
{.is-result}

> Q : Quels sont les sujets sur lesquels la DAO peut voter ?  
{.is-question}

> R : La DAO peut voter sur les sujets liés à l'écosystème RealToken. Dans un premier temps, les sujets sont limités à la méthodologie de fonctionnement de la gouvernance et les périodes d'incitation. Pour d'autres sujets, il faudra déterminer si les ressources et les priorités sont d'actualité pour la DAO. Pour cela, ouvrez un sujet sur le forum de gouvernance et obtenez suffisamment de soutien pour déclencher le processus de création de propositions.
{.is-result}

> Q : Qui développe les applications et nouvelles fonctionnalités pour la DAO ?  
{.is-question}

> R : RealT est le prestataire principal pour la DAO. Il développe les applications, les nouvelles fonctionnalités, et s'occupe de l'écosystème RealToken sous mandat de la DAO. D'autres prestataires pourront fournir des services à la DAO sous réserve d’acceptation de la DAO.
{.is-result}

> Q : La DAO peut-elle dépenser tous les fonds de la trésorerie ?  
{.is-question}

> R : En principe oui, la DAO a le contrôle total des fonds de la trésorerie. Toutefois, des dépenses inadaptées pourraient avoir des conséquences importantes sur l'avenir de la DAO, la valeur des REG, et la continuité des DApps. Il en va donc de la responsabilité des membres de la DAO d'avoir des réflexions approfondies avant de voter les dépenses et de planifier les priorités pour que la DAO puisse perdurer dans le temps et distribuer une part de ses revenus tout comme une entreprise qui distribue des dividendes.
{.is-result}

> Q : Pourquoi devons nous fournir un mail pour participé au forum de gouvernance ?  
{.is-question}

> R : Comme toutes plateforme et pour limité le spam, les adresses a usage unique sont bloquer, une adresse valide permet de limité les spam, de créer un compte identifiable et de vous communiquer les informations / notifiaction lier au forum. La communauté s'engage ne pas utiliser les mails dans d'autre but que ceux directement lier au formu de gouvernance et ne vous obligera jamais à lier un mail avec une adresse on-chain. Pour la gouvernance V2 la mécanique vas évoluer pour permettre aux utilisateur de créer un compte sur le forum directement identifier par les NFT Citizen, Activity et Collector, qui jourons le rols de carte d'identité de l'écosystème Realtoken
{.is-result}

### 7.3.2. Mécanismes avancés ⭐⭐

> Q : Comment fonctionne le mécanisme de verrouillage des REG ?  
{.is-question}

> R : Dans la v1, le verrouillage est une condition pour pouvoir obtenir des bonus avec la participation aux votes. Chaque session (epoch) est activée et définie par la DAO. Il y a une première phase de souscription durant laquelle vous pouvez déposer et retirer vos REG, suivie d'une période de verrouillage durant laquelle vous ne pouvez plus retirer les REG et devez voter. Au terme de la période de verrouillage, les REG sont libérés et vous pouvez les retirer ou les laisser dans le vault. Ils seront automatiquement verrouillés pour la prochaine session au moment du lock des tokens (vous pouvez donc retirer jusqu'à la fin de la periode de souscription suivante).
{.is-result}

> Q : Comment sont choisies les adresses autorisées à créer des propositions ?  
{.is-question}

> R : Durant les premières semaines de fonctionnement de la v1, les adresses autorisées seront uniquement des adresses de RealT, le temps que la communauté s'organise pour élire des personnes de confiance au sein de la communauté pour devenir Proposer.
{.is-result}

> Q : Quel est le but du rôle de Proposer ?  
{.is-question}

> R : Les Proposers sont des personnes de confiance au sein de la communauté qui ont pour rôle de s'assurer que les propositions soumises sur le forum de gouvernance sont de qualité, pertinentes, et réfléchies, qu'elles respectent la méthodologie définie par la DAO pour la création de propositions. Ils peuvent également être sollicités pour des tâches spécifiques liées à la gouvernance et doivent créer les propositions on-chain.
{.is-result}

> Q : La DAO peut-elle forcer un changement sans le consentement de RealT ?  
{.is-question}

> R : Les propositions soutenues par une majorité importante de la DAO peuvent être forcées. Toutefois, à l'heure actuelle, la DAO ne possède concrètement les DApps créées par RealT pour la future DAO. Il devra donc y avoir une reprise formelle de la responsabilité de la DApp ou des DApps par la DAO pouvant entraîner un paiement à RealT pour la prestation fournie.
La DAO ne peux imposer ou forcé des choses qui sont sous la responsabilité de RealT ou autre prestataire
{.is-result}

> Q : Quelles sont les DApps sur lesquelles la DAO peut agir ?  
{.is-question}

> R : À l'heure actuelle, les DApps sur lesquelles la DAO peut agir sont : les contrats de gouvernance, le vault de verrouillage des REG, le contrat de treasury DAO. D'autres DApps s'ajouteront progressivement, et les nouveaux contrats développés par RealT, financés par la DAO, seront directement sous le contrôle et la responsabilité de la DAO.
{.is-result}

> Q : Pourquoi la DAO n'a pas un contrôle total des DApps développées par RealT jusqu'à présent ?  
{.is-question}

> R : La DAO doit en premier lieu s'organiser, définir des processus, gagner en maturité, et acquérir des experts qui pourront exercer les rôles de contrôle et de gestion des DApps. Le contrôle d'une DApp demande une expertise et une compréhension du fonctionnement des smart contracts et de la blockchain. C'est pourquoi, en tant que prestataire, RealT assume pour la DAO jusqu'à présent la responsabilité de ces DApps afin de garantir la continuité et la stabilité nécessaires pour durer dans le temps.
{.is-result}

> Q : Quelles sont les pools de liquidité pris en charge pour le calcule du powervoting  
{.is-question}

> R : La plus part des pools de liquidité sont pris en charge par l'outils de calcule des balances et du pouvoir de vote, toute fois cela demande une intégration dans les outils pour qu'il soie reconnu. Afin d'etre intégre il doit y avoir un graph TheGraph pour le pool afin de fournir les données, et une demande doit etre fait a RealT pour implémenter l'intégration (il sera possible de contribuer a l'outils sur Github dans un future proche)
{.is-result}

### 7.3.3. Aspects techniques ⭐⭐⭐

> Q : Pourquoi ne pas utiliser le REG comme token de gouvernance mais utiliser le `PowerVotingRegistry` ?
{.is-question}

> R : Le REG est un token utilisé pour le calcul du power voting. Les pouvoirs de vote sont calculés off-chain par RealT et enregistrés sur le `PowerVotingRegistry`. Cette approche permet de ne pas pénaliser les participants aux pools de liquidité ou autres services qui utilisent des REG. Ainsi, le REG peut avoir plusieurs cas d'usage, en plus du vote, sans pénaliser le pouvoir de vote. De plus, la DAO peut modifier les mécanismes de calcul du power voting en changeant l'algorithme off-chain, sans avoir à modifier le REG ou autre smart contract, ce qui lui permet d'accorder un boost de vote en fonction de ses objectifs. Par exemple : pour soutenir l'apport de liquidité sur un ou plusieurs DEX, la DAO peut décider de compter les REG verrouillés dans le pool ainsi que la liquidité déposée en contrepartie comme des REG supplémentaires, voire même d'accorder plus de pouvoir de vote pour un REG déposé dans un pool de liquidité. La DAO pourrait aussi décider que les REG sur les wallets, qui ne bénéficient donc pas à l'écosystème, pourraient valoir moins d'une unité de pouvoir de vote. Par exemple, un REG sur un wallet pourrait donner 0,5 de pouvoir de vote, contre 1 REG dans un pool de liquidité qui pourrait donner 1,2 de pouvoir de vote.
{.is-result}

> Q : Pourquoi avoir désactivé le mécanisme de délégation ?  
{.is-question}

> R : Le mécanisme de délégation a été désactivé pour des raisons de simplification de la mise en place de la v1 et pour inciter le plus de participation à l'élaboration des bases de la gouvernance. De plus, avec la mécanique d'incentive et les faibles revenus de la DAO actuellement, il aurait été contre-productif de distribuer des récompenses sans une contrepartie des holders de REG envers la DAO.
{.is-result}