---
title: Proposition utilisant Sablier.com
description: 
published: false
date: 2025-01-02T18:25:02.048Z
tags: sablier, stream, vesting
editor: markdown
dateCreated: 2025-01-02T16:29:46.001Z
---

# Création d'un stream Sablier

## Ressources

- site : https://sablier.com/
- docs : https://docs.sablier.com/
- docs Vesting : https://docs.sablier.com/apps/features/vesting
- docs Courbe : https://docs.sablier.com/concepts/lockup/stream-shapes#lockup-linear
- docs contrats déployés : https://docs.sablier.com/guides/lockup/deployments#mainnets
- sepolia exemple :https://sepolia.etherscan.io/tx/0x86239fed5f924b61c7f5656e0e9322d7809097f2c2153b478fb38c07f85bdb4d
- tally exemple test net : https://www.tally.xyz/gov/reg-dao-beta/proposal/6332954951702383415567855302130620076860949854692323526582374508844353542470

## Instructions communes à tous les types de stream

1. Créer une nouvelle proposal
1. Compléter le formulaire de description de la proposal
1. Ajouter une Action
   - custom action
     - Contrat du token envoyé
     - Use the imported ABI (si réussi, si non il faut fournir l'abi)
     - Contract Method -> approve
1. Ajouter une Action
   - custom action
     - Contrat Sablier SablierV2BatchLockup ([docs déploiement](https://docs.sablier.com/guides/lockup/deployments))
     - Use the imported ABI (si réussi, si non il faut fournir l'abi)
     - Contract Method -> [Selectionner la methode qui est adapter] (de préférance celle avec timestamps sont plus simple a configurer)
     - Calldatas -> [Renseigner les paramètres] la partie batch peut etre compliquée à compléter, surtout les éléments de type tulpe[]
1. Effectuer la simulation pour vérifier que tout est bon

## Types de stream

Il existe plusieurs types de stream qui donnent des comportements de déverrouillage différents :

### Stream Linéaire (Linear)

- Distribution à taux constant par seconde
- La courbe forme une ligne droite en diagonale
- Idéal pour des distributions simples et prévisibles

#### variante

Linear avec Cliff

- Similaire au stream linéaire mais avec une période de blocage initial
- Aucun token n'est disponible pendant la période de cliff
- Après le cliff, la distribution devient linéaire
- Parfait pour les plans de vesting avec période de blocage

Déverrouillage et Linear

- Similaire au stream linéaire mais avec un déverrouillage initial
- Un nombre de token est déverrouillé instantanément
- Solde de la distribution devient linéaire
- Parfait pour les plans incitatifs à court et moyen terme

Déverrouillage, Cliff et Linear

- Combinaison de déverrouillage, cliff et linéaire
- Un nombre de token est déverrouillé instantanément
- Après le premier déverrouillage, une periode de lock est appliqué
- Solde de la distribution devient linéaire après la periode de lock
- Parfait pour les plans de lancement de token

### Stream Exponentiel

- La distribution s'accélère avec le temps
- Les bénéficiaires reçoivent plus de tokens vers la fin de la période
- Idéal pour les airdrops et les incitations à long terme

#### variante

Cliff Exponentiel

- Combine trois éléments :
  1. Une période de cliff (blocage initial)
  2. Un déblocage instantané d'un montant spécifique à la fin du cliff
  3. Une distribution exponentielle pour le reste des tokens
- Particulièrement adapté pour les plans de vesting d'équipe
- Encourage la rétention à long terme

### Timelock

- Bloque tous les tokens pendant une période définie
- Libération totale en une fois à la fin de la période

### Unlock in Steps

- Déverrouillage par paliers réguliers
- Permet des libérations périodiques régulières
- Les tokens non réclamés s'accumulent

#### variante

Unlock Monthly

- Palier mensuel
- Déverrouillage mensuel à date fixe
- Idéal pour les salaires ou plans ESOP

BackWeighted

- Déverrouillage par paliers progressifs
- Palier avec valeur indépendante
- Les tokens non réclamés s'accumulent
- Idéal pour les plans de vesting avec des paliers de déverrouillage

## Informations spécifiques

### Structure technique des segments

Cette structure est utilisé par le stream dynamique (LD)
Chaque `segment` dans un `stream dynamique` est défini par trois paramètres :

- `amount` : montant de tokens à distribuer (uint128)
- `exponent` : l'exposant définissant la courbe de distribution (UD2x18)
- `timestamp` : horodatage de fin du segment (uint40)

La formule de distribution est : f(x) = x^exp \* csa + Σ(esa)
où :

- x = temps écoulé / temps total du segment
- exp = exposant du segment
- csa = montant du segment actuel
- Σ(esa) = somme des montants des segments précédents

#### Structure du tuple

Dans Tally, la partie Batch des fonctions de type LD sont composées d'un champ `segments` qui est un tableau de type `tuple[]`.
Attention les explications suivantes sont basées sur la fonction `createWithTimestampsLD`, pour d'autre fonctions de type LD, il faudra adapter les types de tuple.

```solidity
struct Segment {
        uint128 amount;
        UD2x18 exponent;
        uint40 timestamp;
}
```

le champs `segments` dans la fonction `createWithTimestampsLD` est composer de 3 segment

- segment 0 : Représente la periode entre le startTime et la fin du segment 0
- segment 1 : Représente la periode entre la fin du segment 0 et la fin du segment 1
- segment 2 : Représente la periode entre la fin du segment 1 et la fin du stream

### Exemple complet de paramètre pour la fonction `createWithTimestampsLD`

Cet exemple est basé sur les paramètres de la proposal [RIP000xx] - Liberation des REG du budget Team RealT
Le stream est de type Cliff Exponential, cela signifie que le stream commence après une période de cliff, puis distribue les tokens de manière exponentielle, avec des déblocages plus importants vers la fin de la période.

Retrouver la proposition sur Tally : https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposals

```
Fonction/Signature: createWithTimestampsLD(address,address,tuple[])
Constact Targate : 0x0F324E5CB01ac98b2883c8ac4231aCA7EfD3e750

Calldata (batch):
lockupDynamic -> address : 0x555eb55cbc477Aebbe5652D25d0fEA04052d3971
asset -> address : 0x0AA1e96D2a46Ec6beB2923dE1E61Addf5F5f1dce
batch -> tuple[]:
  sender -> address : 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c
  recipient -> address : 0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099
  amount -> uint256 : 50000000000000000000000000
  transferable -> bool : true
  cancelable -> bool : false
  startTime -> uint40 : 1714521600
  segments -> tuple[]: [
    ["0", "1000000000000000000", "1746057600"],
    ["500000000000000000000000", "1000000000000000000", "1746057601"],
    ["49500000000000000000000000", "3000000000000000000", "1872288000"]
  ]
  broker -> tuple [address, uint256] : [0x0000000000000000000000000000000000000000, 0]
```

#### Détail des segments

```json
[
  ["0", "1000000000000000000", "1746057600"],
  ["500000000000000000000000", "1000000000000000000", "1746057601"],
  ["49500000000000000000000000", "3000000000000000000", "1872288000"]
]
```

- segment 0 : ["0", "1000000000000000000", "1746057600"]
- segment 1 : ["500000000000000000000000", "1000000000000000000", "1746057601"]
- segment 2 : ["49500000000000000000000000", "3000000000000000000", "1872288000"]

##### Explications :

- segment 0 :

  - 0 = amount distribuer durant le segment
  - 1000000000000000000 = exposant du segment = 1 en décimal (1000000000000000000 = 10^18)
  - 1746057600 = timestamp de fin du segment (1er mai 2025 00:00:00 UTC)

- segment 1 :

  - 500000000000000000000000 = amount distribuer durant le segment (500k ^18)
  - 1000000000000000000 = exposant du segment = 1 en décimal (1000000000000000000 = 10^18)
  - 1746057601 = timestamp de fin du segment (1er mai 2025 00:00:01 UTC)

- segment 2 :

  - 49500000000000000000000000 = amount distribuer durant le segment (49.5 Millions ^18)
  - 3000000000000000000 = exposant du segment (3000000000000000000 = 10^18)
  - 1872288000 = timestamp de fin du segment (1er mai 2029 00:00:00 UTC)

**Le segment 1**, définit une date de fin de segment avec une distribution de 0 depuis le paramètre `startTime`, ce qui crée une periode de lock dur sans distribution.

**Le segment 2**, définit une date de fin de segment avec une distribution de 500k tokens, ce qui libere une somme définie dans amount disponnibe instantanément à la date de fin du segment.

**Le segment 3**, définit une date de fin de segment avec une distribution de 49.5 Millions tokens, ce qui crée la courbe exponentielle à partir de la fin du segment 2 jusqu'à la date de fin du segment 3, il va distribuer le montant amount sur la periode avec un exposant de 3
