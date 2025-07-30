---
title: Qu'est ce qu'un Merkel Tree
description: 
published: true
date: 2025-07-30T16:42:55.674Z
tags: 
editor: markdown
dateCreated: 2025-07-30T16:42:55.674Z
---

# Les arbres de Merkle expliqué simplement

Imaginez que vous voulez prouver que votre photo fait partie d'un album familial sans avoir à montrer toutes les autres photos, afin de préserver la vie privée des autres personnes présentes sur cet album. C'est exactement ce que font les arbres de Merkle : ils permettent de **vérifier qu'une information fait partie d'un ensemble sans avoir besoin de tout révéler**

Pour la DAO Realtoken et dans la blockchain les arbres de mekle sont également utilisés pour permettre des actions sur un smart contract qui ne connaît pas les actions avant que l'utilisateur n'exécute la transaction, c'est un peu comme si le smart contract était la personne à qui vous voulez prouver que la photo est dans l'album sans lui fournir autre chose que votre photo et des indices (hash).
La seule chose que connais le smart contract, c'est le hash "root" de l'arbre de Merkle tree (votre photo ici serait les actions que vous demandez d'exécuter au smart contract)

## Qu'est-ce qu'une empreinte digitale numérique (hash) ?

### L'analogie de l'empreinte digitale

Tout comme chaque personne a une empreinte digitale unique, chaque donnée numérique peut avoir sa propre "empreinte digitale" appelée **hash** (ou empreinte). Cette empreinte présente des caractéristiques remarquables :

- **Unique** : Deux données différentes auront toujours des empreintes différentes (peut être faux avec certains algorithmes vulnérables comme md5)
- **Toujours la même taille** : Que vous ayez un mot ou un livre entier, l'empreinte fait toujours la même longueur
- **Irréversible** : On ne peut pas retrouver la donnée originale à partir de son empreinte
- **Très sensible** : Le moindre changement dans la donnée modifie complètement l'empreinte

Pour tester vous pouvez écrire un mot, une lettre et une page de texte dans ce site par exemple : <https://devbox.tools/fr/utils/md5-sha1-sha256-hash-generator/>

### Démonstration avec des exemples concrets

Voici comment de petits changements, produisent des empreintes totalement différentes basées sur SHA256

| Texte original | Empreinte (premiers caractères) | Changement effectué |
|---|---|---|
| "Bonjour" | 9172e8eec99f144f72eca9a568759580edadb2cfd154857f07e657569493bc44 | Texte original |
| "bonjour" | 2cb4b1431b84ec15d35ed83bb927e27e8967d75f4bcd9cc4b25c8d879ae23e18 | Juste la majuscule en minuscule |
| "Bonjour!" | 083de31ac1fa14f95671a6e39cc6c72d8fed1590b2a51759bc3f54a76b4169c4 | Ajout d'un simple point d'exclamation |
| "Bonjour Alice" | 44a0fc349f47f9774122f1de8eca398bda30eb145ea2bb552b7961411601f0f0 | Ajout d'un prénom |
| "Bonjour Alice." | d57bdd277c9301838b2db4d7764afbac2e903c493ae893c3c259486475bbf665 | Ajout d'un point final |

Comme vous pouvez le voir, même changer une seule lettre de majuscule à minuscule transforme complètement l'empreinte. C'est cette propriété qui rend les empreintes si utiles pour détecter la moindre modification.

Dans le cas d'un smart contract cela signifie que si l'utilisateur tente de mettre une donnée différente de celle prévu dans le merkle tree, le contrat pourra savoir que l'utilisateur tente de tricher sur l'exécution de la transaction, sans jamais connaître réellement les données du merkle tree

## L'arbre de Merkle : Comme un arbre généalogique pour les données

### L'analogie de l'arbre généalogique

Pensez à un arbre généalogique de votre famille. Au sommet, vous avez les grands-parents (la racine / root), puis leurs enfants (les branches), et enfin les petits-enfants (les feuilles). Dans un arbre de Merkle, c'est pareil, mais avec des empreintes numériques :

- **Les feuilles / leaf** = les empreintes de vos données originales (comme des photos)
- **Les branches / proofs** = les empreintes combinées de plusieurs feuilles  
- **La racine / root** = l'empreinte finale qui représente tout l'ensemble

### Exemple concret : L'album photo familial

Imaginons que vous avez 4 photos de famille que vous voulez protéger contre toute modification, et prouver à des personnes que ces photos sont bien dans l'album

**Étape 1 : Créer les feuilles**
Chaque photo reçoit son empreinte unique (comme sa carte d'identité numérique).

1. **Photo de Marie** à son anniversaire → Empreinte : b03a70221b0799301928f74ae1194372e6b7ee8d6d1d1ef6d1aeed9bc7d09ae5
2. **Photo de Paul** au mariage → Empreinte : 6c4ad6287b8599400e76ac8542fe542fd699ff644da253932dd76bd12c9a0d94  
3. **Photo de Sophie** à la plage → Empreinte : 1d1b6f002f11dd0b99f98ac981e6654d3d887038cb4cd1a3f985331fe3c7ad52
4. **Photo de Jean** au ski → Empreinte : 28344c68575b44fdd51a06ae48e33bc20d1e8eecd462b13073a18d0d13630446

**Étape 2 : Créer les branches**

- On combine les empreintes de Marie et Paul : c798a57a13f131e957ae236c3b608574f9c3f8dc53c443af214b478bee7158f9
- On combine les empreintes de Sophie et Jean : 33a5d59250e94276f5b63e50b32fd4842a53f6ac7d93234db3c39a646294fae7

**Étape 3 : Créer la racine**
On combine les deux branches pour obtenir une empreinte finale : e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

Cette empreinte finale (la racine) est comme un "résumé" de tout l'album. Si quelqu'un change ne serait-ce qu'un pixel dans une photo, l'ordre des photos, ou quoi que ce soit, la racine sera complètement différente.

## Comment vérifier sans tout révéler : La magie de la preuve de Merkle

### Le problème à résoudre

Supposons que Sophie veut prouver que sa photo fait partie de l'album familial, mais elle ne veut pas révéler les autres photos (pour respecter la vie privée)

### La solution : La preuve de Merkle

Au lieu de montrer toutes les photos, Sophie n'a besoin que de quelques éléments

1. **Son empreinte** : 1d1b6f002f11dd0b99f98ac981e6654d3d887038cb4cd1a3f985331fe3c7ad52
2. **L'empreinte de Jean** (son "voisin" dans l'arbre) : 28344c68575b44fdd51a06ae48e33bc20d1e8eecd462b13073a18d0d13630446
3. **L'empreinte de la branche Marie+Paul** : c798a57a13f131e957ae236c3b608574f9c3f8dc53c443af214b478bee7158f9
4. **La racine publique** (connue de tous) : e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

### Le processus de vérification

**Étape 1** : Combiner Sophie + Jean = 33a5d59250e94276f5b63e50b32fd4842a53f6ac7d93234db3c39a646294fae7

**Étape 2** : Combiner cette nouvelle branche avec la branche Marie+Paul: c798a57a13f131e957ae236c3b608574f9c3f8dc53c443af214b478bee7158f9 + 33a5d59250e94276f5b63e50b32fd4842a53f6ac7d93234db3c39a646294fae7 = e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

**Étape 3** : Comparer avec la racine publique : e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee === e7f064c2e7584f99814491d9d1eb0d4a8bf0df88c85214b7a398aa31d983b7ee

Si les deux racines sont identiques, c'est la preuve que la photo de Sophie fait bien partie de l'album original.
La personne qui vérifie la preuve peut donc : hasher la photo de Sophie, réaliser les étapes de combinaison et prouver que le root est bien celui de l'album.

Dans notre cas d'usage avec un smart contract, il peut donc vérifier que l'action demandée est bien celle inscrite dans le merkle tree, qui a été voté et approuvé par la DAO. A aucun moment un utilisateur peux demander au contrat d'exécuter autre chose que ce qui est dans le merkle tree, rappelez-vous du teste de hashage, la moindre modification peu importe son ampleur ou sa localisation donnera un résultat totalement différent, l'exécution d'action librement accessible est donc sécurisée sans devoir enregistrer dans le contrat une grande quantité de données.

## Pourquoi c'est révolutionnaire ?

### Les avantages dans la vie quotidienne :
1. **Efficacité** : Pas besoin de vérifier tout l'album, juste quelques empreintes
2. **Confidentialité** : Les autres photos restent privées
3. **Sécurité** : Impossible de tricher sans que cela se voit
4. **Rapidité** : La vérification est quasi instantanée

### Analogie avec la vie réelle

C'est comme si vous pouviez prouver que votre nom figure sur une liste d'invités à un mariage sans avoir à révéler tous les autres noms de la liste. Vous donnez juste votre nom et quelques "indices" qui permettent de reconstituer un "code secret" qui correspond à celui de la liste officielle

## Applications concrètes : Cette technologie est utilisée partout autour de nous

- **Bitcoin** : Pour vérifier que votre transaction est bien dans un bloc et ne peut pas être modifiée dans le futur
- **Sauvegardes** : Pour s'assurer que vos fichiers n'ont pas été corrompus
- **Mises à jour logicielles** : Pour garantir qu'un programme n'a pas été modifié
- **Systèmes de vote électronique** : Pour vérifier l'intégrité des résultats
- **Vie privée**: pour prouver une information sans divulguer l'ensemble des informations

## En résumé: Un arbre de Merkle est comme un **système d'empreintes digitales pour les données**

Il permet de:

- Créer une "carte d'identité" unique pour un ensemble de données
- Détecter instantanément toute modification
- Vérifier qu'une donnée fait partie d'un ensemble sans tout révéler
- Protéger la vie privée tout en garantissant la sécurité

La prochaine fois que vous entendrez parler d'arbres de Merkle, pensez à cet album photo familial : c'est une façon simple et sûre de prouver que vos données sont authentiques sans avoir à les montrer en entier.

## En conclusion

Pour la DAO l'usage de merkle tree pour l'exécution sécurisée de transactions permettra en un seul vote de valider une multitude de transactions, qui n'auraient pas pu entrer entièrement dans la transaction de vote à cause de l'espace limité dans les block.
L'usage de merkle tree est donc adapté pour les votes qui demandent un nombre important de transactions dépassent la capacité d'un block, pour les autres cas les transactions resteront inclus directement dans la transaction de vote.
