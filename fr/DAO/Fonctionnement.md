---
title: 4. Fonctionnement technique de la DAO REG
description: 
published: true
date: 2024-10-10T12:44:04.519Z
tags: 
editor: markdown
dateCreated: 2024-10-08T08:37:31.066Z
---

## **4.1. Smart Contracts principaux**

#### **Adresses des smart contracts :**

-   [Governor: 0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63](https://gnosisscan.io/address/0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63#code)
-   [REGTreasuryDAO: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c](https://gnosisscan.io/address/0x3f2d192F64020dA31D44289d62DB82adE6ABee6c#code)
-   [PowerVotingRegistry: 0x6382856a731Af535CA6aea8D364FCE67457da438](https://gnosisscan.io/address/0x6382856a731Af535CA6aea8D364FCE67457da438#code)
-   [Incentive: 0xe1877d33471e37fe0f62d20e60c469eff83fb4a0](https://gnosisscan.io/address/0xe1877d33471e37fe0f62d20e60c469eff83fb4a0#code)

#### **⭐ Pour les novices**

La DAO RealToken fonctionne grâce à des programmes informatiques spéciaux appelés "smart contracts". Ces contrats gèrent automatiquement les règles de la DAO, les votes, et les récompenses. Il y a quatre contrats principaux :

1.  Le contrat de gouvernance : C'est le cerveau de la DAO. Il gère les propositions et les votes,
2.  Le contrat de trésorerie : Il gère les fonds de la DAO et applique un délai de sécurité avant d'exécuter les décisions,
3.  Le contrat de pouvoir de vote : Il calcule le pouvoir de vote de chaque membre,
4.  Le contrat d'incitation : Il distribue des récompenses aux membres actifs de la DAO qui effectuent des votes et bloquent leurs REG (si une période est activée par la DAO).  
     

#### **⭐⭐ Pour les initiés**

La DAO RealToken repose sur un ensemble de smart contracts interconnectés :

1.  Contrat de gouvernance : Basé sur le standard OpenZeppelin Governor, avec des modifications mineures, il gère le cycle de vie des propositions, le processus de vote, et l'exécution des décisions on-chain,
2.  Contrat de trésorerie (REGTreasuryDAO) : Implémente un mécanisme de timelock pour sécuriser l'exécution des décisions de la DAO et gérer ses fonds,
3.  Contrat de pouvoir de vote (PowerVotingRegistry) : Enregistre le pouvoir de vote des détenteurs de REG en fonction de leur solde, de boost et de leur activité, il permet ainsi une plus grande flexibilité que le modèle classique de vote par token, 1 token = 1 vote,
4.  Contrat d'incitation : Gère la distribution des récompenses pour encourager la participation active à la gouvernance, pour ce faire il faut voter et bloquer ses REG pour une durée déterminée (epoch).

Ces contrats interagissent pour assurer un fonctionnement transparent et sécurisé de la DAO tout en encourageant la participation active à la gouvernance.  
 

#### **⭐⭐⭐ Pour les experts**

L'architecture technique de la DAO RealToken est composée de plusieurs smart contracts clés :

##### **4.1.1. Contrat de gouvernance**

-   Basé sur OpenZeppelin Governor avec des modifications personnalisées,
-   Gère le cycle de vie complet des propositions : création, vote et exécution,
-   Intègre les interactions avec le timelock pour augmenter la sécurité des exécutions,
-   Implémente des contrôles d'accès pour la création de propositions, il permet l'activation de 4 mode différents pour autoriser les propositions à être créées,
-   Interagit directement avec le PowerVotingRegistry pour créer les snapshots des pouvoirs de vote basés sur les valeurs enregistrées dans le PowerVotingRegistry.  
     

##### **4.1.2. Contrat de trésorerie (REGTreasuryDAO)**

-   Basé sur le TimelockControllerUpgradeable d'OpenZeppelin,
-   Implémente un mécanisme de timelock pour ajouter un délai de sécurité avant l'exécution des décisions on-chain,
-   Gère les fonds de la DAO et contrôle leur utilisation,
-   Intègre des rôles spécifiques (UPGRADER\_ROLE) pour la gestion des mises à niveau du contrat,
-   Utilise le pattern UUPS (Universal Upgradeable Proxy Standard) pour permettre des mises à niveau futures,
-   Interagit avec les contrats externes appelés par les propositions.  
     

##### **4.1.3. Contrat de pouvoir de vote (PowerVotingRegistry)**

-   Enregistre le pouvoir de vote en fonction du solde REG et d'autres facteurs (par exemple, durée de détention, lock, pool de liquidité, etc.),
-   Implémente une logique d'auto-délégation pour simplifier l'activation du pouvoir de vote.

##### **4.1.4. Contrat d'incitation**

-   Intègre une logique d'état qui modifie les actions possibles, le cycle complet s'appelle “epoch”, pour chaque epoch il faut voter et bloquer ses REG pour recevoir des récompenses,
-   Distribue des récompenses aux participants actifs de la gouvernance (qui votent),
-   Utilise des mécanismes de verrouillage pour encourager l'engagement à long terme (verrouillage de la durée de l'epoch),
-   Intègre des calculs de récompenses basés sur l'activité de gouvernance (votes et verrouillage des REG).

Ces contrats sont conçus pour être modulaires et évolutifs, permettant des mises à jour et des améliorations futures sans perturber l'ensemble du système de gouvernance. Le contrat REGTreasuryDAO, en particulier, ajoute une couche de sécurité supplémentaire en imposant un délai entre la proposition d'une action et son exécution, permettant ainsi à la communauté de réagir si nécessaire.

A terme tous les contrats de l'écosystème RealToken seront contrôlés, upgradables uniquement par le contrat REGTreasuryDAO via des proposals, seules quelques fonctions d'urgence pourront être exécuté en dehors du cycle de la DAO par un comité de sécurité qui aura des droits limités à des actions d'urgence pour sécuriser les contrats.

## **4.2. Mécanismes de vote et de proposition**

#### **⭐ Pour les novices**

La DAO RealToken vous permet de participer aux décisions importantes de l'écosystème d'applications lié à la DAO. Voici comment ça marche :

1.  Sur le forum de la DAO, les membres peuvent proposer des idées pour améliorer la DAO et l'écosystème,
2.  Un débat à lieu autour des propositions afin de mesurer la faisabilité et l'intérêt des propositions par la communauté,
3.  Si une proposition obtient un nombre de voix suffisant, elle est transformée en proposal et soumise au vote de la DAO,
4.  Les membres de la DAO peuvent voter pour ou contre la proposition,
5.  Si la proposition est approuvée, elle est exécutée par le REGTreasuryDAO.

#### **⭐⭐ Pour les initiés**

Le système de vote et de proposition de la DAO RealToken comprend :

1.  Le forum de la DAO : Où les membres peuvent proposer des idées et débattre afin de mesurer l'intérêt des propositions,
2.  Durant la période de débat, la communauté doit mesurer l'intérêt et la faisabilité des propositions,
3.  Si une proposition est jugée comme faisable et pertinente, une proposition concrète est travaillée avec l'établissement de tous les paramètres liés à la proposition, comme les actions à exécuter on-chain, le cout de développement, les gains pour la DAO, etc...
4.  Création de propositions : Processus contrôlé par des règles d'accès définies dans le contrat de gouvernance, selon les paramètres il peut y avoir des restrictions à la création de proposition, cela a pour but d'éviter les attaques de gouvernance, et permettre une mise en place progressive des règles de fonctionnement de la DAO en toutes sécurité,
5.  Cycle de vie des propositions : Création, période de vote, exécution (si approuvée).
6.  Mécanisme de timelock : Délai de sécurité avant l'exécution des décisions approuvées.

#### **⭐⭐⭐ Pour les experts**

Les mécanismes de vote et de proposition sont implémentés comme suit :

1.  Forum de la DAO :
    -   Où les utilisateurs peuvent proposer des idées et débattre afin de mesurer l’intérêt des propositions,
    -   Un débat a lieu autour des propositions afin de mesurer la faisabilité et l’intérêt des propositions par la communauté ou l'un des prestataires de la DAO,
    -   Si une proposition est jugée comme faisable et pertinente, une proposition concrète est travaillée avec l’établissement de tous les paramètres liés à la proposition, comme les actions à exécuter on-chain, le cout de développement, les gains pour la DAO, étude d'impacte et de risque, les besoins ou non d'un audite de sécurité, etc...
2.  Création de propositions :
    -   Contrôlées par le contrat de gouvernance,
    -   Quatre modes d'autorisation sont possibles pour la création de propositions, ces modes de proposition vise à optimiser la gestion des risques de la DAO, et à limiter l'exposition des fonds, avec un objectif de décentralisation à moyen terme en fonction de la maturité de la DAO,
    -   Inscription on-chain de la proposition avec tous les codes permettant l'exécution de la proposition si elle est approuvée,
    -   Interaction avec le PowerVotingRegistry pour créer des snapshots des pouvoirs de vote.
3.  Processus de vote :
    - Basé sur le standard OpenZeppelin Governor avec des modifications personnalisées,
    - Utilisation du pouvoir de vote enregistré dans le PowerVotingRegistry,
    - Possibilité de vote : pour, contre ou abstention,
    - Seuls les membres enregistrés dans le PowerVotingRegistry peuvent voter,
    -	Un délais de 1 jours est appliqué entre le début du vote et la validation de la transaction de création de la proposal (par sécurité, pour une annulation éventuelle),
    - Le vote se fait sur une durée déterminée par le contrat de gouvernance, en général 7 jours,
    - Les votants vérifient que la proposition est conforme à la description et aux échanges sur le forum de la DAO (vérification du code d'exécution on-chain).
4.  Calcul du pouvoir de vote
    -   Enregistré dans le PowerVotingRegistry,
    -   Prend en compte le solde REG, la durée de détention, le verrouillage, et potentiellement d'autres facteurs, selon l'algorithme de calcul validé par la DAO,
    -   Implémente une logique d'auto-délégation pour simplifier l'activation du pouvoir de vote.
5.  Exécution des propositions :
    -   Utilisation du REGTreasuryDAO comme timelock pour ajouter un délai de sécurité,
    -   Exécution on-chain des actions approuvées après le délai,
    -   Possibilité d'annulation pendant la période de timelock si nécessaire, par un groupe de sécurité.
6.  Système d'incitation :
    -   Géré par le contrat d'incitation,
    -   Récompenses basées sur la participation aux votes et le verrouillage de REG,
    -   Système d'epochs pour structurer les périodes de récompenses,
    -   Activé par la DAO qui définie la durée des epochs et les récompenses associés.
7.  Mode de proposition :
    -   ProposerWithRole : L’objectif est de limiter strictement les adresses pouvant soumettre un vote, afin de prévenir le spam et les propositions abusives. Ce mode sera activé uniquement pendant les premières semaines ou mois du déploiement initial de la gouvernance (version 1), pour établir une base solide avant d’élargir le nombre d’auteurs de propositions,
    -   ProposerWithVotingPower : Ce mode exige que l’auteur de proposition ait un pouvoir de vote minimum. Sans ce pouvoir, aucune proposition ne peut être créée. C’est l’étape ultime de la décentralisation, où tous ceux ayant suffisamment de pouvoir de vote peuvent soumettre des propositions. Il n’y a plus de rôles privilégiés; tous les détenteurs suivent les mêmes règles,
    -   ProposerWithRoleAndVotingPower : Ce mode combine deux restrictions, l’auteur de proposition doit avoir un rôle spécifique et un pouvoir de vote minimum. Si l’une des conditions n’est pas remplie, la proposition échouera. Ce mode élargit les propositions aux membres les plus engagés de la communauté, exigeant un solde minimum de REG pour garantir leur engagement et des enjeux financiers. Ils doivent être élus et posséder suffisamment de REG. Cela permet de démarrer la décentralisation, tester les processus et limiter les risques,
    -   ProposerWithRoleOrVotingPower : Ce mode permet la création de propositions si l’auteur de proposition possède soit le rôle requis, soit un pouvoir de vote minimum. Si aucune de ces conditions n’est remplie, la proposition échouera. C’est l’avant-dernière étape de la décentralisation de la gouvernance. Ce mode exige un nombre important de REG pour proposer librement des améliorations, sans passer par un rôle spécifique. Cela permet à quiconque de proposer des améliorations en engageant un nombre significatif de REG, tout en maintenant les auteurs de proposition ayant un rôle spécifique, qui n’ont pas besoin de REG minimum. Le rôle peut être retiré en cas de non-respect des processus.
8.  Délégation :
    -   Délégation de pouvoir de vote a été retirée pour permettre l'activation des incitations pour les votants et les lockers de REG,
    -   Les utilisateurs ne peuvent pas déléguer leur pouvoir de vote à d'autres adresses, mais bénéficient d'une auto-délégation par défaut,
    -   Dans une v2 la délégation fera sont retour et sera même une partie importante du système.

Ces mécanismes sont conçus pour assurer une gouvernance équitable, sécurisée et incitative, tout en permettant une évolution future du système.

Dans les premières semaines/mois la DAO est implémentée avec des restrictions afin de permettre la mise en place des règles de fonctionnement, d’expérimenter les processus et de limiter les risques.

Afin de donner une bonne vision des étapes d'évolution, un débat devra avoir lieu pour déterminer les paliers déclenchant une décentralisation de niveau supérieur, donnant une plus grande autonomie et ce libérant de la confiance apporté aux divers intervenants.

  
 

## **4.3. Système d'incitation et de récompense**

#### **⭐ Pour les novices**

La DAO RealToken vous récompense pour votre participation active. Voici comment ça marche :

1.  Vous gagnez des récompenses sous diverses formes en votant sur les propositions,
2.  Vous pouvez augmenter vos récompenses en bloquant vos tokens REG pendant une certaine période,
3.  La participation dans certaines pools de liquidité ou autre de l'écosystème RealToken peuvent vous donner des récompenses supplémentaires,
4.  Les récompenses peuvent être sous forme de REG, d'ERC20, ou encore sous forme de boost sur le pouvoir de vote,
5.  La DAO décide des récompenses et des paramètres du système d'incitation.

#### **⭐⭐ Pour les initiés**

Le système d'incitation de la DAO RealToken est conçu pour encourager la participation active :

1.  La DAO définie les récompenses et les paramètres du système d'incitation, le type et les montants des récompenses,
2.  Récompenses basées sur la participation aux votes et le verrouillage de REG, participation dans certaines pools de liquidité ou autre de l'écosystème RealToken peuvent vous donner des récompenses supplémentaires,
3.  Système d'epochs pour structurer les périodes de récompenses et optimiser la participation,
4.  Attribution on-chain automatique des récompenses en stable coin claimable à la fin de chaque epoch,
5.  Possibilité pour la DAO d'ajuster les paramètres du système d'incitation afin d'optimiser la participation et répondre à l'évolution de la DAO.

#### **⭐⭐⭐ Pour les experts**

Le système d'incitation et de récompense est implémenté comme suit : 

1.  Contrat d'incitation :
    -   Gère l'état du système (actif/inactif) et les cycles d'epochs,
    -   Calcule les récompenses en fonction de l'activité de vote et du verrouillage de REG,
    -   Affecte automatiquement les récompenses à la fin de chaque epoch.
2.  Mécanisme de verrouillage :
    -   Les utilisateurs peuvent verrouiller leurs REG pour la durée d'un epoch dans le contrat d'incitation,
    -   Le verrouillage peut augmenter le pouvoir de vote et les récompenses potentielles selon les paramètres définis par la DAO,
    -   Le verrouillage permet de marquer un engagement à long terme de la part du votant, diminue la liquidité des REG et limite les risques d'attaque de gouvernance.
3.  Paramètres ajustables :
    -   Durée des epochs,
    -   Quantité de récompenses par epoch,
    -   Type de récompenses,
    -   Ces paramètres sont appliqués par la DAO dans la proposal d'activation d'une epoch,
    -   Pour les boost de pouvoir de vote, la DAO peut ajuster en fonction des besoins : la formule et le poids accordé dans le système de calcul du powerVoting qui génère les pouvoirs de vote.
4.  Intégration avec le PowerVotingRegistry :
    -   Le verrouillage des REG affecte le pouvoir de vote enregistré,
    -   Synergie entre la participation à la gouvernance et les récompenses.       
5.  Sécurité et évolutivité :
    -   Contrat upgradable pour permettre des améliorations futures,
    -   Mécanismes de pause en cas d'urgence.
6.  Calcul des récompenses :
    -   Comptabilise le nombre total de propositions émises durant une epoch,
    -   Comptabilise le nombre de votes effectués par chaque votant durant l'epoch,
    -   Pondéré par la quantité de REG verrouillés.

```plaintext
userBonus = (userState.depositAmount * userState.voteAmount * epochState.totalBonus) / epochState.totalWeights;
```

-   \`userState.depositAmount\` : Le montant déposé par l'utilisateur pour cette époque en REG,
-   \`userState.voteAmount\` : Le nombre de votes effectués par l'utilisateur durant cette époque,
-   \`epochState.totalBonus\` : Le montant total de bonus alloués pour cette époque,
-   \`epochState.totalWeights\` : La somme totale des poids pour tous les utilisateurs dans cette époque.

Cette formule est appliquée dans le contrat aux lignes suivantes :

[REGIncentiveVault.sol](https://gnosisscan.io/address/0x4b79755d1ea8937c027408e3aa72d69a260f6237#code#F1#L342) de la ligne 342 à 346.

Le total des poids s'incrémente du montant déposé par le user à chacun de ses votes (cf ligne 245 du contrat).
  
Ce système vise à créer un cercle vertueux d'engagement dans la gouvernance, en récompensant la participation active tout en renforçant la stabilité à long terme de l'écosystème RealToken.

La mécanique de récompense sera entièrement revu avec la mise en service de la gouvernance V2 utilisant les NFT afin d'améliorer le système d'incitation, permettre une plus grande granularité, et précision des actions à encourager en fonction des besoins de la DAO et des profils des holders.

De nouveaux types de récompenses seront introduits comme comme la matière noire des NFT Cityzen, des points de soutien pour les votes permettant la mise en avant des contenus liés au NFT Activity, etc...

L'écosystème RealToken est pensé pour disposer de nombreux outils d'optimisation de participation aux votes, et encourager les actions et contributions divers qui sont bénéfiques à la DAO tout en permettant un contrôle précis des finances de la DAO afin de ne pas dilapider la trésorerie ou dévaluer le token REG.  
 

[Page suivante](/fr/DAO/Guide_Pratique)