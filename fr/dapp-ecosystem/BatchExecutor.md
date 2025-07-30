---
title: Executeur d'actions groupées pour la DAO
description: 
published: true
date: 2025-07-30T16:59:11.553Z
tags: 
editor: markdown
dateCreated: 2025-07-30T16:59:11.553Z
---

# 📜 Documentation du `MerkleGovernanceExecutor`

## Introduction : Qu'est-ce que c'est ?

Le `MerkleGovernanceExecutor` est le "bras armé" de notre DAO. C'est un contrat intelligent sur la blockchain qui a une seule mission principale : permettre l'exécution de batch de transactions complexes approuvées par la DAO de manière sécurisée et vérifiable.

Imaginez que la DAO vote pour financer un projet, modifier un paramètre dans le RMM, récupérer des fonds perdus sur plusieurs adresses qui ont déposés dans le RMM, ou lancer une nouvelle fonctionnalité. Ce contrat est celui qui appuie sur les boutons et s'assure que seule la volonté de la DAO, et rien d'autre, est mise en œuvre, le tout de façon décentralisée, sécurisée et transparente par n'importe qui qui voudrait executé les transactions.

---

! Avant de continuer vous devez comprendre le fonctionnement d'un arbre de Merkle -> [merkle-tree](merkle-tree.md)

### ⭐ : Pour Tous (La Vision d'Ensemble) 🔭

Pensez à ce contrat comme à un **notaire ultra-sécurisé et automatisé**.

1. **La Décision de la DAO** : La communauté vote sur un ensemble de décisions (par exemple : "Payer 100€ à Alice", "Mettre à jour le logo du site web", "Allouer 1000 tokens au projet X, récupérer les RealTokens de 20 utilisateurs sur le RMM").
2. **Le Sceau d'Approbation** : Une fois le vote terminé et approuvé, toutes ces décisions sont regroupées et scellées dans une "enveloppe numérique" unique et infalsifiable. Ce sceau est ce que nous appelons la **racine de Merkle** (Merkle Root). C'est une empreinte digitale unique de l'ensemble des décisions approuvées (transactions a executer).
3. **L'Exécution par le Notaire** : N'importe qui peut ensuite se présenter au "notaire" (le contrat `MerkleGovernanceExecutor`) avec une des décisions approuvées (par exemple, "Payer 100€ à Alice"). Pour prouver que cette décision faisait bien partie du vote, la personne présente une "preuve de Merkle", qui est un peu comme une clé qui ne fonctionne qu'avec le sceau de l'enveloppe.
4. **Vérification et Action** : Le "notaire" (le contrat `MerkleGovernanceExecutor`) vérifie que la clé (la preuve) correspond bien au sceau (la racine de Merkle). Si tout est correct, il exécute la décision sans poser de questions. Si la preuve est fausse, il refuse catégoriquement.

**En bref : Ce contrat s'assure que seules les actions validées par un vote de la DAO peuvent être exécutées, et ce, de manière totalement transparente, sécurisée et autonome par n'importe qui.**

---

### ⭐⭐ : Pour les Curieux (Comment ça marche ?) 🤔

À ce niveau, nous introduisons quelques concepts techniques, mais sans plonger dans le code.

Le contrat fonctionne sur un principe de **validation par preuve cryptographique**. Il ne stocke pas chaque décision individuelle votée par la DAO, ce qui serait très coûteux en frais de gaz sur la blockchain. À la place, il utilise une structure de données optimisée appelée **Arbre de Merkle**.
Le contrat stocke la racine de Merkle de chaque vote approuvé par la DAO, il ne connais aucune des informations des transactions votées avant leurs execution, ce processus permet donc de programmer un nombre illimité de transactions avec un seul vote.

**Le processus, étape par étape :**

1. **Vote et Création de l'Arbre** :
   * Le processus de vote classique de la DAO s'aplique avec les échanges et préparatifs sur le forum de gouvernance
   * Un processus hors-chaîne (off-chain) prend toutes les transactions à inclure dans le vote, les "hache" (crée une empreinte digitale pour chacune) et les organise dans un Arbre de Merkle.
   * La DAO vote sur le contenu de tout l'arbre de merkle, qui contient un ou plusieurs "batch" (un lot) de transactions à exécuter.
   * La cime de cet arbre est la **racine de Merkle** (`merkleRoot`). C'est la seule information que la DAO doit publier et enregistrer dans notre contrat `MerkleGovernanceExecutor`, c'est donc cette information que le vote écris sur la blockchain à l'execution final du vote.

2. **L'Appel à l'Exécution** :
   * Un utilisateur (ou un bot) peut exécuter une ou plusieurs transactions de ce lot sans demander d'autorisation.
   * Il appelle la fonction `execute` du contrat.
   * Il doit fournir quatre éléments clés :
     1. La `merkleRoot` (le sceau d'approbation du vote).
     2. Les `transactions` qu'il souhaite exécuter (enssemble de groupe de transaction).
     3. Un `batchId` (un identifiant pour s'assurer que ce lot n'est exécuté qu'une seule fois).
     4. La `merkleProof` (la "clé" cryptographique qui prouve que ses transactions font bien partie de l'arbre validé par la `merkleRoot`).

3. **La Vérification par le Contrat** :
   * Le contrat vérifie d'abord si la `merkleRoot` est bien une racine active et approuvée.
   * Le contrat vérifie si le `batchId` n'a pas déjà été exécuté.
   * Ensuite, il utilise la `merkleProof` pour recalculer la racine à partir des transactions fournies.
   * Si la racine recalculée correspond à la `merkleRoot` attendue, la preuve est valide ! Le contrat exécute alors les transactions.
   * Enfin, il marque ce `batchId` comme "exécuté" pour empêcher toute ré-exécution.

**L'avantage majeur** : C'est extrêmement efficace. Le contrat n'a besoin de connaître qu'une seule empreinte (`merkleRoot`) pour valider potentiellement des milliers de transactions et permettre à n'importe qui d'executer les transactions sans demander d'autorisation, pas de dépendance dans une entité centralisée à qui il faut faire confiance.

---

### ⭐⭐⭐ : Pour les utilisateurs avancé (En Profondeur) 👩‍💻

Le `MerkleGovernanceExecutor` est un contrat *upgradeable* (via le pattern UUPS), *pausable* et sécurisé contre les attaques de réentrance. Il utilise l' `AccessControl` d'OpenZeppelin pour gérer les permissions.
Son objectif principal est de permettre l'exécution de batch de transactions complexes impossible à faire entrer dans une transaction de vote à cause de la limitation de taille de block, le vote approuve la racine de merkle de l'arbre de merkle qui peux contenir un nombre illimité de transactions rassemblées dans des batch qui respectent la limite gas, l'execution des transactions est totalement libre et limité aux transactions et paramètres approuvés par la DAO de manière sécurisée, vérifiable et infalcifiable.

**Rôles principaux :**

* `GOVERNANCE_ROLE` : Ce rôle (détenu par la DAO elle-même) est le seul à pouvoir ajouter ou retirer des `merkleRoots` via les fonctions `addMerkleRoots` et `removeMerkleRoot`. C'est lui qui "officialise" les résultats d'un vote.
* `DEFAULT_ADMIN_ROLE` : Gère les mises à jour du contrat et peut assigner des rôles (également détenu par la DAO).
* `EMERGENCY_ROLE` : Peut uniquement mettre le contrat en pause en cas d'urgence (détenu par le commiter de sécurité).

#### Focus sur la fonction `execute` (fonction principale)

Voici la signature de la fonction la plus importante :

```solidity
function execute(
    bytes32 expectedMerkleRoot,
    bytes32[] calldata merkleProof,
    uint256 batchId,
    TransactionPayload[][] calldata transactions
) external payable whenNotPaused nonReentrant;
```

**Analyse des paramètres :**

le contrat gère plusieurs merkle root actif en paralèle, cela permet à la DAO d'avoir des votes séparés, d'enregister le résultat à tout moment dans le contrat, et meme si les executions précédentes ne sont pas encore intégralement executées.

* `expectedMerkleRoot`: La racine de Merkle qui autorise ce lot. Le contrat vérifie qu'elle a bien été ajoutée au préalable par le `GOVERNANCE_ROLE`.
* `merkleProof`: Le tableau de `bytes32` qui constitue la preuve cryptographique.
* `batchId`: Un `uint256` qui sert de "nonce" ou d'identifiant unique pour ce batch de groupe de transaction à executer. Il est combiné avec la "feuille" de l'arbre de Merkle, garantissant qu'un même batch ne peut pas être exécuté deux fois avec des `batchId` différents.
* `transactions`: Un tableau de tableaux de `TransactionPayload` (groupe de transaction). Cette structure permet de regrouper des transactions qui doivent etre executées ensemble avec succès. Si un groupe de transactions échoue, seul le groupe de transaction revert et cela n'empêche pas les autres groupes valides d'être exécutés. c'est appelé soft revert.

Dans le cas d'un soft Revert, la DAO devra examiner le groupe de transaction qui a revert et décider si elle veut le reexecuter ou non, pour cela elle devra a nouveau effectuer un vote pour ce groupe de transactions.

**Logique interne de `execute` :**

1. **Vérification de la `merkleRoot`** : `require($.merkleRoots[expectedMerkleRoot], INVALID_MERKLE_ROOT());` vérifie que la `merkleRoot` est bien une racine active et approuvée.
2. **Construction de la feuille (`leaf`)** : Le contrat calcule un hash des `transactions` et du `batchId`. C'est cette feuille qui doit être prouvée.

   ```solidity
   bytes32 leaf = keccak256(abi.encode(batchId, keccak256(abi.encode(transactions))));
   ```

3. **Vérification de la non-exécution** : Il vérifie que cette feuille (batch) n'a pas déjà été exécutée : `require(!$.executed[expectedMerkleRoot][leaf], ALREADY_EXECUTED());`
4. **Vérification de la preuve de Merkle** : C'est le cœur de la sécurité qui va vérifier que l'adresse qui execute la tranaction ne tente pas de tricher en executant des transactions modifiées.

   ```solidity
   require(MerkleProof.verify(merkleProof, expectedMerkleRoot, leaf), INVALID_PROOF());
   ```

5. **Marquage et Exécution** : Si tout est bon, il marque la feuille comme exécutée *avant* de lancer les transactions pour se prémunir des attaques de réentrance. Puis, il boucle sur les groupes de transactions et les exécute via un appel interne `executeGroup`.
6. **Gestion des fonds** : Le contrat suit la valeur en XDAI envoyée et s'assure à la fin que la balance du contrat correspond bien à ce qui était attendu, offrant une sécurité supplémentaire (si il y as transfère de XDAI).

Ce design permet une gouvernance décentralisée, robuste et peu coûteuse, où la validation des votes est dissociée de l'exécution des transactions, offrant une grande flexibilité.

---

### ⭐⭐⭐ : Exemple concret de transaction

soon

### ⭐ : FAQ

soon