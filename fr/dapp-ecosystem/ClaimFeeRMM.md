---
title: Reclamation des frais du RMM
description: 
published: true
date: 2025-07-30T16:54:51.577Z
tags: 
editor: markdown
dateCreated: 2025-07-30T16:54:51.577Z
---

# Documentation Technique - Contrat RmmClaimSplitFee

[![Solidity](https://img.shields.io/badge/Solidity-0.8.27-blue.svg)](https://soliditylang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Network](https://img.shields.io/badge/Network-Gnosis-green.svg)](https://gnosis.io/)

## 📋 Informations de Déploiement

- **Réseau** : Gnosis Chain (Chain ID: 100)
- **Adresse du Contrat** : `0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137`
- **Déployeur** : `0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099`

https://gnosisscan.io/address/0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137#code

### 🔑 Rôles Configurés

| Rôle           | Adresse                                      | Description                                                                     |
| -------------- | -------------------------------------------- | ------------------------------------------------------------------------------- |
| **Admin**      | `0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099` | Gestion de la réserve et du contrôleur (sera transféré à la DAO ultérieurement) |
| **Governance** | `0x3f2d192F64020dA31D44289d62DB82adE6ABee6c` | Gestion des tokens et répartitions et reprise après pause (unpause)                                   |
| **Emergency**  | `0xa26851621Ae64c66D09742c43591ad620A5B8256` | Contrôles d'urgence (pause)                                                     |

## 🎯 Vue d'Ensemble

Le contrat `RmmClaimSplitFee` est un système de distribution automatique de tokens qui permet de répartir les frais collectés dans une réserve (RMMv3) vers plusieurs destinataires selon des pourcentages prédéfinis.

### 🔧 Fonctionnalités Principales

- ✅ **Distribution Multi-tokens** : Supporte plusieurs tokens ERC20 simultanément (armmToken)
- ✅ **Répartition Configurable** : Pourcentages personnalisables pour chaque destinataire ( à 2 décimales)
- ✅ **Sécurité Renforcée** : Protection contre la réentrance et contrôle d'accès
- ✅ **Gestion d'Urgence** : Mécanisme de pause pour situations critiques

## 📊 Structure des Données

### RepartitionInfo  
La distribution est effectuée sur l'ensemble des tokens configurés pour tout les destinataires configurés sans distinction.

```solidity
struct RepartitionInfo {
  address user; // Adresse du destinataire
  uint64 repartitionPercentage; // Pourcentage (base 10000)
}
```

#### Processus de mise à jour des pourcentages et destinataires

**Ajouter un destinataire**  
Pour ajouter un destinataire, il faudra mettre à jour les autres destinataires pour avoir un total de 100% (10000) sur tous les destinataires cumulés.
il faut appeler la fonction `addRepartitionInfo` avec le tableau de destinataires et les pourcentages.

- Commencer par retirer le(s) destinataire(s) pour lesquels il faut mettre à jours les pourcentages (removeRepartitionInfo).
- Ajouter le/les nouveau(x) destinataire(s) avec le pourcentage (addRepartitionInfo).

**Supprimer un destinataire**  
Pour supprimer un destinataire, il faudra mettre à jour les autres destinataires pour avoir un total de 100% (10000) sur tous les destinataires cumulés.
il faut appeler la fonction `removeRepartitionInfo` avec l'index du destinataire à supprimer.

- Commencer par retirer le(s) destinataire(s) pour lesquels il faut mettre à jours les pourcentages (removeRepartitionInfo).
- Ajouter le/les nouveau(x) destinataire(s) avec le pourcentage (addRepartitionInfo).

**Détails Importants :**

- Le pourcentage est exprimé en base 10000 (ex: 5000 = 50%)
- Maximum autorisé : 10000 (100%)
- Ne pas enregistrer un destinataire avec un pourcentage de 0%
- Les fonctions `addRepartitionInfo` et `removeRepartitionInfo` sont appelées par la DAO via un vote, il faut impérativement effectuer toutes les étapes de mise à jour en une seule transaction.

exemple de transaction : https://gnosisscan.io/tx/0x0bea7110df6c16439f542fbc81f79781c9482f48ae71bac9c45857704e459b77

la structure du paramètre tulpe[] :

```
[
  ["0x3f2d192F64020dA31D44289d62DB82adE6ABee6c",8000],
  ["0x3e652E97ff339B73421f824F5b03d75b62F1Fb51",2000]
]
```

## 🚀 Fonctions Principales

### 1. 📤 `claim()` - Distribution des Tokens

**Signature :**

```solidity
function claim() external whenNotPaused nonReentrant
```

**Description :**
Fonction publique (accècible par n'importe quelle adresse) qui déclenche la distribution de tous les tokens configurés vers tous les destinataires selon leurs pourcentages respectifs.

**Processus d'Exécution :**

1. Récupère la liste des tokens et des destinataires
2. Pour chaque token :
   - Lit le solde disponible dans la réserve
   - Calcule et transfère la part de chaque destinataire
3. Utilise la formule : `montant * pourcentage / 10000`

**Conditions Préalables :**

- ✅ Contrat non pausé
- ✅ Au moins un token configuré
- ✅ Au moins un destinataire configuré
- ✅ Solde disponible dans la réserve

**Sécurité :**

- 🛡️ Protection contre la réentrance (`nonReentrant`)
- 🛡️ Respect de la pause d'urgence (`whenNotPaused`)

**Exemple d'Utilisation :**

```typescript
// Via ethers.js
await rmmClaimSplitFee.claim();

// Via viem
await walletClient.writeContract({
  address: '0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137',
  abi: contractAbi,
  functionName: 'claim',
});
```

---

### 2. 🪙 `addTokens()` - Ajout de Tokens

**Signature :**

```solidity
function addTokens(IERC20[] calldata newTokens) external onlyRole(GOVERNANCE_ROLE)
```

**Description :**
Ajoute de nouveaux tokens ERC20 à la liste des tokens à distribuer.

**Paramètres :**

- `newTokens` : Tableau d'adresses de contracts ERC20

**Validations :**

- ✅ Seul le rôle GOVERNANCE peut appeler cette fonction
- ✅ Chaque adresse doit être un contrat valide (code.length > 0)

**Événement Émis :**

```solidity
event TokensSet(IERC20[] newTokens);
```

**Exemple d'Utilisation :**

```typescript
// Ajouter un seul token
const tokenAddresses = ['0x...tokenAddress'];
await rmmClaimSplitFee.connect(governanceWallet).addTokens(tokenAddresses);

// Ajouter plusieurs tokens
const multipleTokens = ['0x...token1Address', '0x...token2Address', '0x...token3Address'];
await rmmClaimSplitFee.connect(governanceWallet).addTokens(multipleTokens);
```

**Coût en Gaz :**

- **Minimum** : ~27,884 gas
- **Maximum** : ~126,897 gas
- **Moyenne** : ~81,440 gas

---

### 3. 👥 `addRepartitionInfo()` - Configuration des Destinataires

**Signature :**

```solidity
function addRepartitionInfo(RepartitionInfo[] calldata newRepartitionInfo) external onlyRole(GOVERNANCE_ROLE)
```

**Description :**
Ajoute de nouveaux destinataires avec leurs pourcentages de répartition respectifs.

**Paramètres :**

- `newRepartitionInfo` : Tableau de structures RepartitionInfo

**Validations :**

- ✅ Seul le rôle GOVERNANCE peut appeler cette fonction
- ✅ Chaque pourcentage doit être ≤ 10000 (100%)

**Événement Émis :**

```solidity
event RepartitionInfoSet(RepartitionInfo[] newRepartitionInfo);
```

**Exemples d'Utilisation :**

#### Exemple 1 : Répartition Simple (2 destinataires)

```typescript
const repartitions = [
  {
    user: '0x...address1',
    repartitionPercentage: 7000, // 70%
  },
  {
    user: '0x...address2',
    repartitionPercentage: 3000, // 30%
  },
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(repartitions);
```

#### Exemple 2 : Répartition Complexe (5 destinataires)

```typescript
const complexRepartitions = [
  {
    user: '0x...treasuryAddress',
    repartitionPercentage: 4000, // 40% - Trésorerie
  },
  {
    user: '0x...developmentAddress',
    repartitionPercentage: 2500, // 25% - Développement
  },
  {
    user: '0x...marketingAddress',
    repartitionPercentage: 1500, // 15% - Marketing
  },
  {
    user: '0x...stakingAddress',
    repartitionPercentage: 1500, // 15% - Staking Rewards
  },
  {
    user: '0x...burnAddress',
    repartitionPercentage: 500, // 5% - Burn
  },
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(complexRepartitions);
```

**Coût en Gaz :**

- **Minimum** : ~27,839 gas
- **Maximum** : ~120,987 gas
- **Moyenne** : ~81,108 gas

## 📋 Gestion des Destinataires (Recipient Info)

### 🔍 Consultation des Destinataires

**Fonction de Vue :**

```solidity
function getRepartitionInfo() external view returns (RepartitionInfo[] memory)
```

**Exemple d'Utilisation :**

```typescript
// Récupérer tous les destinataires actuels
const currentRepartitions = await rmmClaimSplitFee.getRepartitionInfo();

console.log('Destinataires configurés :');
currentRepartitions.forEach((repartition, index) => {
  console.log(`${index + 1}. ${repartition.user} - ${repartition.repartitionPercentage / 100}%`);
});
```

### ➕ Ajout de Nouveaux Destinataires

Les nouveaux destinataires s'ajoutent à la liste existante (pas de remplacement).

**Stratégies d'Ajout :**

#### 1. Ajout Incrémental

```typescript
// Ajouter un nouveau destinataire à la configuration existante
const newRecipient = [
  {
    user: '0x...newAddress',
    repartitionPercentage: 1000, // 10%
  },
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(newRecipient);
```

#### 2. Reconfiguration Complète

```typescript
// 1. Supprimer tous les destinataires existants
const currentRepartitions = await rmmClaimSplitFee.getRepartitionInfo();
for (let i = currentRepartitions.length - 1; i >= 0; i--) {
  await rmmClaimSplitFee.connect(governanceWallet).removeRepartitionInfo(i);
}

// 2. Ajouter la nouvelle configuration
const newConfiguration = [
  { user: '0x...address1', repartitionPercentage: 5000 },
  { user: '0x...address2', repartitionPercentage: 3000 },
  { user: '0x...address3', repartitionPercentage: 2000 },
];

await rmmClaimSplitFee.connect(governanceWallet).addRepartitionInfo(newConfiguration);
```

### ➖ Suppression de Destinataires

**Fonction :**

```solidity
function removeRepartitionInfo(uint256 index) external onlyRole(GOVERNANCE_ROLE)
```

**⚠️ Important :** La suppression utilise la technique "swap and pop" :

- L'élément à supprimer est remplacé par le dernier élément
- Le dernier élément est supprimé
- **Les indices changent après chaque suppression**

**Exemple de Suppression Sécurisée :**

```typescript
// Supprimer plusieurs destinataires (toujours commencer par la fin)
const indicesToRemove = [2, 1, 0]; // Ordre décroissant !

for (const index of indicesToRemove) {
  await rmmClaimSplitFee.connect(governanceWallet).removeRepartitionInfo(index);
}
```

### 📊 Validation des Pourcentages

**Règles de Validation :**

- ✅ Chaque pourcentage individuel : `≤ 10000` (100%)
- ⚠️ **Pas de validation automatique de la somme totale**
- 🎯 **Recommandation** : La somme devrait égaler 10000 pour une distribution complète

**Exemple de Validation Manuelle :**

```typescript
function validateRepartitions(repartitions) {
  const total = repartitions.reduce((sum, rep) => sum + rep.repartitionPercentage, 0);

  if (total !== 10000) {
    console.warn(`⚠️ La somme des pourcentages est ${total / 100}% (recommandé: 100%)`);
  }

  return total;
}

// Utilisation
const repartitions = [
  { user: '0x...', repartitionPercentage: 6000 },
  { user: '0x...', repartitionPercentage: 4000 },
];

validateRepartitions(repartitions); // ✅ Total: 100%
```

## 🔧 Fonctions de Gestion

### 📤 Suppression de Tokens

```solidity
function removeToken(uint256 index) external onlyRole(GOVERNANCE_ROLE)
```

**Exemple :**

```typescript
// Supprimer le premier token (index 0)
await rmmClaimSplitFee.connect(governanceWallet).removeToken(0);
```

### 🔍 Fonctions de Vue

```solidity
// Récupérer tous les tokens configurés
function getTokens() external view returns (IERC20[] memory)

// Récupérer l'adresse de la réserve
function getReserve() external view returns (address)

// Récupérer le contrôleur de réserve
function getReserveController() external view returns (IReserveController)
```

## 🚨 Fonctions d'Urgence

### ⏸️ Pause d'Urgence

```solidity
function pause() external onlyRole(EMERGENCY_ROLE)
```

**Effet :** Bloque la fonction `claim()` jusqu'à la reprise.

### ▶️ Reprise

```solidity
function unpause() external onlyRole(GOVERNANCE_ROLE)
```

**Note :** Seul le rôle GOVERNANCE peut reprendre (pas EMERGENCY).

## 🛡️ Sécurité et Bonnes Pratiques

### ✅ Contrôles de Sécurité Intégrés

- **Protection contre la Réentrance** : `ReentrancyGuardTransient`
- **Contrôle d'Accès** : Système de rôles OpenZeppelin
- **Validation des Entrées** : Vérification des adresses de contrats
- **Mécanisme de Pause** : Arrêt d'urgence disponible

### 🎯 Recommandations d'Utilisation

1. **Validation des Pourcentages** : Toujours vérifier que la somme = 10000
2. **Test en Environnement de Développement** : Tester la configuration avant le déploiement
3. **Monitoring** : Surveiller les événements émis pour les changements de configuration
4. **Sauvegarde de Configuration** : Conserver une copie des configurations pour rollback

### ⚠️ Points d'Attention

- **Suppression d'Éléments** : Les indices changent après chaque suppression
- **Pas de Validation de Somme** : Le contrat ne vérifie pas que les pourcentages totalisent 100%
- **Ordre d'Exécution** : Les destinataires sont traités dans l'ordre de la liste

## 📞 Support et Maintenance

Pour toute question technique ou problème d'intégration, référez-vous à :

- **Documentation Complète** : `README.fr.md`
- **Tests de Référence** : `test/RmmClaimSplitFee.test.ts`
- **Code Source** : `contracts/RmmClaimSplitFee.sol`

---

**🔗 Liens Utiles :**

- [Gnosis Chain Explorer](https://gnosisscan.io/address/0x01a5e8a1685a0479d0a782bfbacb8d6d21f32137)
- [OpenZeppelin AccessControl](https://docs.openzeppelin.com/contracts/4.x/access-control)
- [Solidity Documentation](https://docs.soliditylang.org/)
