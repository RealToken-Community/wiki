---
title: 7. Ressources et support
description: 
published: true
date: 2024-10-18T08:07:51.436Z
tags: 
editor: markdown
dateCreated: 2024-10-18T06:01:19.928Z
---



## 7.1. Documentation technique

### 7.1.1. Smart Contracts v1 ⭐⭐⭐

La documentation des smart contracts est essentielle pour comprendre le fonctionnement interne du DAO RealToken. Elle comprend :

#### Spécifications techniques des principaux contrats :

1. **REGGovernor.sol** : contrat de gouvernance principal, ce contrat gère le processus de gouvernance, y compris la création, le vote et l'exécution des propositions.
	Principales fonctions non standard ajoutées :  

   - **`function setProposerMode(ProposerMode proposerMode) external `**
		**Description :** permet à la gouvernance de définir le mode de proposant, en déterminant les critères que les adresses doivent respecter pour créer des propositions.
		**Paramètres :** **ProposeurMode**,  le nouveau mode de proposant à définir, qui peut être l'un des suivants :
		- **`ProposerWithRole`** : Seules les adresses avec le **`PROPOSER_ROLE`** peuvent proposer,
		- **`ProposerWithVotingPower`** : Seules les adresses disposant d'un pouvoir de vote suffisant peuvent proposer,
		- **`ProposerWithRoleAndVotingPower`** : Seules les adresses disposant du **`PROPOSER_ROLE`** et d'un pouvoir de vote suffisant peuvent proposer,
		- **`ProposerWithRoleOrVotingPower`** : les adresses disposant soit du **`PROPOSER_ROLE`**, soit d'un pouvoir de vote suffisant peuvent proposer.
		**Contrôle d'accès :** Ne peut être appelé que via une proposition de gouvernance réussie (`onlyGovernance`).
		**Événement émis :** `SetProposerMode`
 
	- **`function setIncentiveEnabled(bool status) external `**
    	**Description :** Active ou désactive le mécanisme d'incitation, qui enregistre les votes dans le coffre d'incitation pour récompenser les participants.
			**Paramètres :** `status` : Un booléen indiquant s'il faut activer (`true`) ou désactiver (`false`) le mécanisme d'incitation.
			**Contrôle d'accès :** Ne peut être appelé que via une proposition de gouvernance réussie.
		**Événement émis :** `SetIncentiveEnabled`
    
	- **`function setRegIncentiveVault(IREGIncentiveVault regIncentiveVault) external `**
	**Description :** Définit l'adresse du contrat de coffre d'incitation REG utilisé pour enregistrer les votes et distribuer les incitations.
	**Paramètres :** `regIncentiveVault` L'adresse du nouveau contrat de coffre d'incitation REG.
	**Contrôle d'accès :** Ne peut être appelé que via une proposition de gouvernance réussie.
	**Événement émis :** `SetRegIncentiveVault`

	- **`function getProposerMode() external view returns (ProposerMode) `**
	**Description :** Renvoie le mode de proposant actuel, indiquant les critères requis pour qu'une adresse créer des propositions.
	**Retourne :** Le `ProposerMode` actuel.
  
	- **`function getIncentiveEnabled() external view returns (bool) `**
	**Description :** Indique si le mécanisme d'incitation est actuellement activé.
	**Retourne:** `true` si le mécanisme d'incitation est activé, `false` sinon.

	- **`function getRegIncentiveVault() external view returns (IREGIncentiveVault) `**
	**Description:** Renvoie l'adresse du contrat de coffre d'incitation REG.
	**Retourne:** L'adresse du contrat `IREGIncentiveVault`.

	- **`function cancelByAdmin(address[] memory goals, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) external returns (uint256)`**
	**Description:** Permet à un administrateur avec le `CANCELLER_ROLE` d'annuler une proposition correspondant aux paramètres donnés.
	**Paramètres:**

		- `targets`: Un tableau d'adresses cibles pour les actions de la proposition,
		- `values`: Un tableau de valeurs ETH à envoyer avec chaque action,
		- `calldatas` : Un tableau de données d'appel pour chaque action,
		- `descriptionHash` : Le hachage keccak256 de la description de la proposition.

		**Retourne :** l'ID de la proposition annulée.
		**Contrôle d'accès :** limité aux adresses avec le `CANCELLER_ROLE`.
<br>
2. **REGTreasuryDAO.sol** : Gestion de la trésorerie de la DAO, ce contrat gère les fonds de la DAO et exécute les transactions approuvées après un délai de sécurité.
Aucune fonction principale non standard n'a été ajoutée.
<br>
3. **REGIncentiveVault.sol** : Système d'incitation et de récompense, ce contrat gère le calcul et la distribution des récompenses aux participants de la DAO s'il est activé par la DAO.
Contrat entièrement créé par RealT, les principales fonctions sont :
	- **`function setNewEpoch(uint256 subscriptionStart,uint256 subscriptionEnd,uint256 lockPeriodEnd,address bonusToken,uint256 totalBonus ) external onlyRole(DEFAULT_ADMIN_ROLE)`**
**Description :** Définit une nouvelle période (epoch) pour le programme d'incitation. Cette fonction initialise une nouvelle période/session avec les paramètres spécifiés.
**Paramètres :**
		- `subscriptionStart` : Timestamp du début de la période d'abonnement.
		- `subscriptionEnd` : Timestamp de la fin de la période d'abonnement.
		- `lockPeriodEnd` : Timestamp de la fin de la période de verrouillage.
		- `bonusToken` : Adresse du jeton utilisé comme récompense.
		- `totalBonus` : Montant total des jetons bonus à distribuer pour cette epoch.

		**Contrôle d'accès :** Seul le rôle « DEFAULT_ADMIN_ROLE » peut appeler cette fonction.
		**ÉvénementÉmis :** `SetNewEpoch`

	- **`function deposit(uint256 amount) public whenNotPaused;`**
	**Description :** Permet aux utilisateurs de déposer des jetons REG dans le coffre pendant la période de souscription pour participer au programme d'incitation.
	**Paramètres :** `amount` : Montant de jetons REG à déposer.
	**Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas suspendu et pendant la période de souscription.
	**Événement émis :** `Deposit`
  
	- **`function depositWithPermit(uint256 amount, uint256 deadline, uint8 v, bytes32 r, bytes32 s ) external whenNotPaused;`**
	**Description :** Permet aux utilisateurs de déposer des jetons REG à l'aide de l'approbation EIP-2612 (permit), qui permet le dépôt en une seule transaction sans approbation préalable.
	**Paramètres :**
		- `amount` : Montant de jetons REG à déposer.
		- `deadline` : Timestamp après lequel la signature n'est plus valide.
		- `v, r, s` : Composants de la signature pour l'approbation.

		**Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause et pendant la période d'abonnement.
		**Événement émis :** `Deposit`

	- **`function remove(uint256 amount) external whenNotPaused;`**
	**Description :** Permet aux utilisateurs de retirer leurs jetons REG déposés après la fin de la période de verrouillage.
	**Paramètres :** `amount` : Montant de jetons REG à retirer.
	**Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause et après la fin de la période de verrouillage.
	**Événement émis :** `Withdraw`

	- **`function recordVote(address user, uint256 proposalId) external onlyGovernance;`**
	**Description :** Enregistre le vote d'un utilisateur sur une proposition pendant la période de verrouillage pour calculer les récompenses incitatives.
	**Paramètres :**
		- `user` : Adresse de l'utilisateur qui a voté.
		- `proposalId` : Identifiant de la proposition sur laquelle l'utilisateur a voté.
    
		**Contrôle d'accès :** Seul le contrat de gouvernance peut appeler cette fonction.
		**Événement émis :** `RecordVote` ou `RecordVoteNotActive`

	- **`function calculateBonus(address user) external view returns (address[] memory, uint256[] memory)`**
	**Description :** Calcule le montant du bonus qu'un utilisateur peut réclamer pour chaque époch passée.
	**Paramètres :**
		- `user` : Adresse de l'utilisateur pour lequel calculer le bonus.

		**Retourne :**
		- `address[]` : Tableau d'adresses des jetons de bonus.
		- `uint256[]` : Tableau de montants de bonus correspondants.

	- **`function claimBonus() public whenNotPaused;`**
	**Description :** Permet aux utilisateurs de réclamer leurs bonus accumulés pour toutes les époch éligibles.
	**Conditions :** La fonction ne peut être appelée que lorsque le contrat n'est pas en pause.
	**Événement émis :** `ClaimBonus`

	- **`function getRegGovernor() external view returns (address)`**
	**Description :** Renvoie l'adresse du contrat de gouvernance REG associé.

	- **`function getRegToken() external view returns (IERC20)`**
	**Description :** Renvoie l'adresse du jeton REG utilisé par le coffre.

	- **`function getCurrentTotalDeposit() external view returns (uint256)`**
	**Description :** Renvoie le montant total des jetons REG actuellement déposés dans le coffre.

	- **`function getCurrentEpoch() external view returns (uint256)`**
	**Description :** Renvoie le numéro de l'époch actuelle.

	- **`function getCurrentEpochState() external view returns (EpochState memory)`**
	**Description :** Renvoie des informations sur l'état de l'époch actuelle.

	- **`function getEpochState(uint256 epoch) external view returns (EpochState memory)`**
	**Description :** Renvoie des informations sur l'état d'une époch spécifique.
	**Paramètres :** `epoch` Numéro de l'époch pour laquelle obtenir des informations.

	- **`function getUserEpochState(address user, uint256 epoch) external view returns (UserEpochState memory)`**
	**Description :** Renvoie des informations sur l'état d'un utilisateur pour une époch spécifique.
	**Paramètres :**
		- `user` : Adresse de l'utilisateur.
		- `epoch` : Numéro de l'époque.

	- **`function getUserGlobalState(address user) external view returns (UserGlobalState memory)`**
	**Description :** Renvoie des informations globales sur l'état d'un utilisateur.
	**Paramètres :**
		- `user` : adresse de l'utilisateur.
<br>
4. **REGPowerVotingRegistry.sol** : Enregistrement des pouvoirs de vote, ce contrat enregistre le pouvoir de vote des participants calculé hors chaîne par RealT selon l'algorithme défini et validé par le DAO.
Contrat entièrement créé par RealT, les principales fonctions sont : 
	- `registerVotingPower(VotingPower[] calldata votingPower) external override onlyRole(REGISTER_ROLE)` Vous remarquerez que pour des raisons de compatibilité avec les standards et le fonctionnement du contrat Gouvernor, le contrat PowerVotingRegistry est basé sur la norme ERC20 avec une modification des comportements des fonctions de base telles que :
		- transfer, transferFrom, approve, etc. sont modifiés pour renvoyer faux,
		- delegate est modifié pour renvoyer une erreur, `REGVotingPowerRegistryErrors.DelegateToOtherNotAllowed()` si un utilisateur essaie de déléguer son vote à un autre utilisateur

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

\[*Un diagramme montrant les interactions entre les contrats*\] 

#### Considérations de sécurité :
- Contrôle d'accès : utilisation d'AccessControl d'OpenZeppelin pour la gestion des rôles,
- Timelock : implémentation d'un délai avant l'exécution des propositions approuvées,
- Évolutivité : utilisation du modèle UUPS pour les futures mises à niveau.

### 7.1.2. Documentation de l'API ⭐⭐

La documentation de l'API fournit des informations sur la manière d'interagir avec le DAO RealToken par programmation. Elle comprend :

- Points de terminaison pour interroger les données du DAO,
- Méthodes de soumission de propositions et de votes,
- Exigences d'authentification,
- Limites de débit et directives d'utilisation.

### 7.1.3. SDK et bibliothèques ⭐⭐

Des SDK et des bibliothèques sont disponibles pour faciliter l'intégration avec le DAO RealToken :

- SDK JavaScript/TypeScript,
- Bibliothèque Python,
- Exemples d'intégration dans différents langages de programmation.

## 7.2. Guides d'utilisation

### 7.2.1. Guide de participation ⭐

Un guide étape par étape sur la façon de participer au DAO RealToken :

1. Acquérir des jetons REG,
2. Connecter un portefeuille à la plateforme de vote DAO,
3. Voter sur les propositions,
4. Participer aux discussions sur le forum.
5. Créer et soumettre des propositions,

### 7.2.2. Guide du processus de gouvernance ⭐⭐

Une explication détaillée du processus de gouvernance :

1. Création et soumission de propositions,
2. Période de discussion et de débat,
3. Période de vote,
4. Exécution des propositions approuvées,
5. Bonnes pratiques pour une participation efficace.

### 7.2.3. Guide des fonctionnalités avancées ⭐⭐⭐

Informations sur les fonctionnalités avancées de la DAO :

1. Utilisation de la délégation,
2. Participation à des comités spécialisés,
3. Contribution aux développements techniques,
4. Analyse des données en chaîne pour une prise de décision éclairée.

## 7.3. Ressources communautaires

### 7.3.1. Forum officiel ⭐

Lien vers le forum officiel de RealToken DAO, où se déroulent les discussions sur les propositions et la gouvernance.

### 7.3.2. Réseaux sociaux ⭐

Liens vers les réseaux sociaux officiels :

- Twitter
- Discord
- Telegram

### 7.3.3. Contenu créé par la communauté ⭐⭐

Une liste organisée de contenus de haute qualité créés par la communauté :

- Tutoriels vidéo
- Articles de blog
- Rapports analytiques

## 7.4. Assistance et soutien

### 7.4.1. FAQ ⭐

Une liste complète des questions fréquemment posées sur le DAO RealToken.

### 7.4.2. Canaux d'assistance ⭐

Informations sur la façon d'obtenir de l'aide :

- Forum d'assistance communautaire
- E-mail d'assistance officiel
- Assistance par chat en direct (si disponible)

### 7.4.3. Rapports de bugs ⭐⭐

Consignes sur la façon de signaler des bugs ou des vulnérabilités de sécurité :

- Programme de primes aux bogues
- Politique de divulgation responsable

## 7.5. Ressources pédagogiques

### 7.5.1. Principes de base du DAO ⭐

Contenu pédagogique pour les nouveaux arrivants :

- Qu'est-ce qu'un DAO ?
- Principes de base de la blockchain et de la cryptomonnaie
- Introduction aux jetons de gouvernance

### 7.5.2. Concepts avancés de DAO ⭐⭐

Contenu plus approfondi pour les participants expérimentés :

- Théorie des jeux dans les DAO
- Modèles économiques des jetons de gouvernance
- Analyse comparative de différentes structures de DAO

### 7.5.3. Ateliers techniques ⭐⭐⭐

Contenu technique pour les développeurs et les utilisateurs avancés :

- Développement de contrats intelligents pour les DAO
- Meilleures pratiques de sécurité dans le développement de DAO
- Mécanismes de gouvernance avancés

## 7.6. Mises à jour et annonces

### 7.6.1. Journal des modifications ⭐⭐

Un journal détaillé des modifications et des mises à jour du DAO :

- Mises à niveau des contrats intelligents
- Modifications des paramètres de gouvernance
- Nouvelles versions de fonctionnalités

### 7.6.2. Feuille de route ⭐⭐

Le futur plan de développement du DAO RealToken :

- Fonctionnalités à venir
- Vision et objectifs à long terme

### 7.6.3. Annonces officielles ⭐

Un flux d'annonces officielles de la DAO :

- Décisions de gouvernance importantes
- Annonces de partenariat
- Événements communautaires