---
title: 3. Phase1, Version simplifiée
description: 
published: true
date: 2024-09-30T09:10:22.330Z
tags: 
editor: markdown
dateCreated: 2024-09-26T12:17:52.404Z
---

## **3.1. Explication de la version simplifiée**

#### **⭐ Pour les novices**

La Phase 1 de la DAO RealToken est une version simplifiée du système final. C'est comme une période d'essai où nous mettons en place les bases de la DAO. Pendant cette phase, nous apprenons comment fonctionne la gouvernance et nous commençons à prendre des décisions ensemble, mais de manière plus simple qu'à l'avenir. Le pouvoir de la DAO est limité pour le moment afin de limiter les risques et permettre un apprentissage progressif, le champs du pouvoir décisionnel augmentera au fil du temps.

#### **⭐⭐ Pour les initiés**

La Phase 1 est une étape cruciale dans le déploiement de la DAO RealToken. Elle vise à :

1.  Mettre en place une structure de gouvernance de base,
2.  Tester les mécanismes de vote et de proposition,
3.  Identifier les besoins et les défis de la communauté,
4.  Commencer à prendre des décisions collectives sur des sujets simples,
5.  Préparer le terrain pour des fonctionnalités plus avancées dans les phases futures,
6.  Etablir les bases de fonctionnement de la DAO,
7.  Eduquer la communauté sur les mécanismes de la DAO,
8.  Expérimenter divers fonctions et approches de gouvernance,
9.  Démontrer le potentiel future de la DAO et du REG.

Cette phase utilise un ensemble limité de smart contracts et de fonctionnalités pour faciliter l'adoption et l'apprentissage. Le choix technologique a été fait pour que cette première phase permette un déploiement rapide et simple avec un cout en développement limité. Cette phase permettra donc d'accumuler de la connaissance et d'établir les principes de base fondamentaux pour le future de la DAO et la nouvelle version en phase 2 avec l'arrivée de NFT.

#### **⭐⭐⭐ Pour les experts**

La Phase 1 de la DAO RealToken est conçue comme une implémentation minimaliste mais fonctionnelle, avec les caractéristiques suivantes :

1.  Gouvernance simplifiée :
    -   Mécanisme de vote basique repris du standard OpenZeppelin governor,
    -   Processus de proposition évolutif et moins rigide pour permettre une plus grande participation et flexibilité d'exploration,
    -   Quorum et seuils de vote ajustés régulièrement pour s'adapter à la participation,
    -   Pas de délégation de pouvoir de vote,
    -   Limitation sur l'accès à la création de proposition afin de limiter le risque de création de proposition malveillante.
2.  Smart contracts de base :
    -   Contrat de gouvernance, avec fonctions de vote et de proposition, repris des standards d'Open Zeppelin,
    -   Contrat de trésorerie simple pour la gestion des fonds de la DAO,
    -   Contrat d'incentive pour la gestion des récompenses incitative au vote,
    -   Contrat PowerVotingRegistry, permettant d'enregistrer le pouvoir de vote de chaque holders de REG.
3.  Intégration limitée avec l'écosystème :
    -   Interactions limitées avec les DApps existantes (RealT restant le tiers de confiance pour les DApps le temps nécessaire pour la montée en compétence de la DAO),
    -   Mécanismes d'incitation simples pour encourager la participation.
4.  Mécanismes de sécurité :
    -   Timelock sur les exécutions de propositions, donnant un délais supplémentaire pour détecter les potentielles problèmes ou erreurs d'une proposition,
    -   Limites sur les montants de trésorerie gérables,
    -   Droits de veto de la part de la RealT sur des propositions extrême ou metant en danger la DAO.
5.  Collecte de données :
    -   Métriques sur la participation et l'engagement des membres,
    -   Feedback continu de la communauté pour informer les phases futures.

Cette phase servira de base pour l'évolution future de la DAO, permettant d'identifier les ajustements nécessaires et les fonctionnalités à développer pour les phases suivantes. En utilisant cette phase comme un laboratoire, nous pourrons expérimenter diverses approches de gouvernance et collecte de données avant d'intégrer celles-ci dans la prochaine version de la DAO avec l'arrivée de NFT. Bien que piloter par le partenaire de confiance RealT, la DAO restera indépendante et les décisions seront toujours prises par les membres de la DAO dans l'espace du champs d'application progressivement élargi. Plusieurs points seront donc a débattre entre les REG holders et RealT afin d'établir les étapes d'élargissement du champs d'application de la DAO afin de garantir la stabilité et la sécurité de l'écosystème.

## **3.2. Processus de mise en place**

#### **⭐ Pour les novices**

La mise en place de la DAO se fait étape par étape. D'abord, nous créons les outils de base pour voter et faire des propositions. Ensuite, nous commençons à les utiliser pour prendre des décisions simples. Au fur et à mesure, nous apprenons et améliorons le système ensemble.

#### **⭐⭐ Pour les initiés**

Le processus de mise en place de la Phase 1 comprend plusieurs étapes clés :

1.  Déploiement des smart contracts de base (déjà réalisé),
2.  Création d'une interface utilisateur simple pour voter et proposer (Utilisation d'une site déjà existante : Tally)
3.  Formation de la communauté sur l'utilisation des outils de gouvernance,
4.  Présentation des premiers projets auxquels la DAO pourra voter (RealT a déjà préparé une série de propositions),
5.  Mise en place du système de discutions et débat pour les propositions (les devs de la communauté ont déjà travaillé sur une solution),
6.  Lancement des premières propositions,
7.  Lancement de la première campagne d'incitation au vote,
8.  Ajustement des paramètres de gouvernance en fonction des retours,
9.  Élargissement graduel du champ d'action de la DAO.

#### **⭐⭐⭐ Pour les experts**

Le processus de mise en place de la Phase 1 est structuré pour maximiser l'apprentissage et l'adaptation :

1.  Infrastructure technique :
    -   Déploiement des smart contracts (gouvernance, trésorerie, incentive, PowerVotingRegistry),
    -   Tests net en groupe restreint (déjà réalisé),
    -   Audits de sécurité et tests approfondis (réaliser en interne RealT),
    -   Test net public (déjà réalisé),
    -   Developpement des outils pour le calcul du powerVoting (Realiser par RealT).
2.  Gouvernance initiale :
    -   Définition des paramètres initiaux (quorum, seuils de vote, timelock : RealT)
    -   Mise en place d'un processus de proposition en plusieurs étapes (discussion, formalisation, vote),
    -   Implémentation de mécanismes de sécurité, y compris le droit de veto de RealT,
    -   Limitation de l'accès a la création de proposition.
3.  Engagement communautaire :
    -   Campagne d'éducation sur les mécanismes de la DAO,
    -   Création de canaux de communication dédiés pour les discussions et le feedback,
    -   Organisation d'événements de gouvernance pour stimuler la participation,
    -   Programme d'incitation.
4.  Itération et optimisation :
    -   Collecte et analyse continues des métriques de participation,
    -   Ajustements réguliers des paramètres de gouvernance,
    -   Expérimentation avec différentes approches de gouvernance,
    -   Débat sur les divers changement et approches de gouvernance.
5.  Expansion progressive :
    -   Définition d'un plan d'élargissement du champ d'action de la DAO,
    -   Négociations avec RealT pour le transfert progressif des responsabilités,
    -   Préparation de la transition vers la Phase 2 avec l'intégration des NFT.

Ce processus est conçu pour être flexible et adaptable, permettant à la communauté d'influencer activement l'évolution de la DAO tout en maintenant la stabilité et la sécurité de l'écosystème. Cela donne également l'opportunité aux REG holders de comprendre et de se familiariser avec les mécanismes de la DAO, de créer et découvrir des vocations, faire émerger des idées et nouveaux cas d'usages. La mise en place progressive de la DAO permet ainsi une transition naturelle et une évolution progressive vers la décentralisation de l'écosystème.

## **3.3. Objectifs à court et moyen terme**

#### **⭐ Pour les novices**

À court terme, nous voulons que tout le monde comprenne comment fonctionne la DAO et commence à participer à hauteur de sa compréhension et compétence. À moyen terme, nous voulons prendre des décisions plus importantes ensemble et décentraliser la gouvernance des services proposés par la DAO.

#### **⭐⭐ Pour les initiés**

Objectifs à court terme :

1.  Atteindre un taux de participation élevé dans les votes,
2.  Éduquer la communauté sur les mécanismes de la DAO (sécurité, équilibre budgétaire, etc.),
3.  Identifier les premiers projets à financer ou à développer,
4.  Tester et ajuster les paramètres de gouvernance,
5.  Mise en place des processus de base pour la gouvernance.

Objectifs à moyen terme :

1.  Élargir progressivement le champ d'actions de la DAO,
2.  Développer de nouveaux cas d'usage pour le REG,
3.  Améliorer les outils de gouvernance basés sur le feedback,
4.  Préparer la transition vers la Phase 2 avec l'intégration des NFT.

#### **⭐⭐⭐ Pour les experts**

Objectifs à court terme (3-6 mois) :

1.  Gouvernance :
    -   Atteindre un quorum stable de 50% sur les votes,
    -   Implémenter et tester différents processus de proposition,
    -   Établir un processus efficace de débat et de raffinement des propositions,
    -   Elire les premiers membres de la communauté pouvant créer des propositions,
    -   Définir le cadre d'une proposition valide pour soumission aux votes.
2.  Engagement communautaire :
    -   Lancer des campagnes d'incitation au vote avec des récompenses,
    -   Organiser des événements éducatifs réguliers sur la gouvernance DAO,
    -   Créer un programme de mentorat pour les nouveaux membres actifs,
    -   Encourager les initiatives bénéfiques pour la DAO et l'écosystème RealToken.
3.  Développement technique :
    -   Développer/perfectionner les outils d'analyse pour suivre la santé de la DAO,
    -   Intégrer des fonctionnalités de base avec l'écosystème existant.

Objectifs à moyen terme (6-18 mois) :

1.  Expansion de la DAO :
    -   Négocier et mettre en œuvre un plan d'élargissement du champ d'actions avec RealT,
    -   Développer des mécanismes de gouvernance plus avancés,
    -   Créer des comités spécialisés pour différents aspects de l'écosystème,
    -   Mise en place plus large de l'accès à la création de proposition.
2.  Innovation et développement :
    -   Lancer des initiatives de recherche et développement pilotées par la communauté,
    -   Expérimenter avec de nouveaux modèles d'incitation et de participation,
    -   Développer des intégrations plus profondes avec l'écosystème,
    -   Prémices de la phase 2 avec l'intégration des NFT.
3.  Préparation de la Phase 2 :
    -   Concevoir et tester les mécanismes d'intégration des NFT dans la gouvernance,
    -   Développer une feuille de route détaillée pour la transition vers la Phase 2,
    -   Former des groupes de travail pour chaque aspect majeur de la nouvelle phase,
    -   Effectuer des tests approfondis avec la communauté pour la phase 2.
4.  Écosystème et partenariats :
    -   Établir des partenariats stratégiques avec d'autres projets DeFi et DAO,
    -   Développer des cas d'usage innovants pour le REG au-delà de la gouvernance,
    -   Intégration des premiers prestataires autre que RealT.

Ces objectifs visent à établir une base solide pour la DAO, tout en préparant le terrain pour une expansion et une innovation continues dans l'écosystème RealToken.

## **3.4. Comment participer à la phase 1**

#### **⭐ Pour les novices**

Pour participer à la phase initiale de la DAO RealToken :

1.  Achetez des tokens REG si vous n'en avez pas déjà,
2.  Suivez les annonces officielles sur les canaux de communication de RealToken,
3.  Participez aux votes quand ils sont ouverts,
4.  Donnez votre avis dans les discussions de la communauté,
5.  Assistez aux événements éducatifs pour en apprendre davantage sur la DAO,
6.  S'informer et se former sur le sujet des DAO.

#### **⭐⭐ Pour les initiés**

Voici comment vous pouvez participer activement à la phase initiale :

1.  Votez régulièrement sur les propositions,
2.  Participez aux débats sur les propositions avant les votes,
3.  Proposez des idées d'amélioration dans les forums de discussion,
4.  Aidez à éduquer les nouveaux membres sur le fonctionnement de la DAO,
5.  Participez aux campagnes d'incitation pour gagner des récompenses,
6.  Testez les nouvelles fonctionnalités et donnez votre feedback,
7.  Contribuez à l'élaboration des processus de gouvernance,
8.  Prendre des initiatives et les soumettre a la communauté,
9.  Rédigez, améliorez, traduisez et partagez le contenu sur les DAO.

#### **⭐⭐⭐ Pour les experts**

Pour une participation approfondie à la phase initiale :

1.  Gouvernance :
    -   Analysez en profondeur les propositions et partagez vos analyses,
    -   Contribuez activement à l'élaboration de métriques pour évaluer la santé de la DAO,
    -   Proposez des ajustements des paramètres de gouvernance, basés sur les données et argumenter avec une visons cours, moyen et long terme.
2.  Développement technique :
    -   Participez activement aux Testnet,
    -   Proposez des améliorations techniques pour les outils de gouvernance,
    -   Contribuez au développement d'outils d'analyse pour la DAO,
    -   Revue et audite des smart contracts continu, rester informer des hack et failles potentielles.
3.  Engagement communautaire :
    -   Organisez pour la communauté des sessions d'éducation,
    -   Créez du contenu explicatif sur le fonctionnement de la DAO,
    -   Participez activement aux programmes de mentorat,
    -   Explorer de nouveaux concepts de Live, vidéo, article, etc.
4.  Innovation :
    -   Proposez de nouveaux cas d'usage argumenté pour le REG,
    -   Participez aux groupes de travail sur l'intégration future des NFT,
    -   Explorez des synergies potentielles avec d'autres projets DeFi.
5.  Préparation de la Phase 2 :

	-   Contribuez à la conception des mécanismes d'intégration des NFT,
	-   Participez aux tests approfondis des nouvelles fonctionnalités,
	-   Aidez à élaborer la feuille de route pour la transition vers la Phase 2.

Votre participation active à tous ces niveaux contribuera à façonner l'avenir de la DAO RealToken et à assurer son succès à long terme.

[Page suivante](/fr/DAO/Fonctionnement)