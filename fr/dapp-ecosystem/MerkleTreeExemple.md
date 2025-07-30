---
title: Exemple de Merkle Tree
description: 
published: true
date: 2025-07-30T16:47:04.794Z
tags: 
editor: markdown
dateCreated: 2025-07-30T16:47:04.794Z
---

# Exemple d'usage d'un Merkel Tree lors de la réclamation des REG (en simplifiant)

## ⭐ Imagine que tu es un utilisateur qui veut réclamer ses REG (suite à une réévaluation par exemple)

### Étape 1 : Tu te connectes à l’interface

L'interface c'est l'application https://claim.realtoken.network/reg , à laquelle tu te connextes avec ton portefeuille crypto.

### Étape 2 : L’interface prépare ta preuve d’éligibilité

En arrière-plan, l’interface utilise la liste officielle des bénéficiaires. Cette liste est très longue et regroupée dans une structure particulière appelée **Merkle Tree**.

Le Merkle Tree est un peu comme un résumé sécurisé de toute la liste des personnes qui ont droit à des USDREG. Il permet de prouver très facilement que tu es bien dans la liste, sans avoir besoin de montrer toute la liste complète.

L’interface va rechercher dans le Merkel Tree les informations te concernant : 
- ton adresse, 
- le montant d'USDREG auqel tu as droit,
- et une **preuve**, appelée la "preuve Merkle" (une série de petits éléments de données, invisibles pour toi en général), qui prouve que ton adresse et le montant auquel tu as droit font partie de cette liste officielle.

### Étape 3 : Tu soumets ta demande de claim avec la preuve

Quand tu cliques sur “Réclamer”, l’interface envoie à la blockchain :
- Ton adresse,
- Le montant que tu demandes,
- Cette preuve Merkle qui prouve que tu as bien ce droit

### Étape 4 : La blockchain vérifie ta preuve

Le contrat intelligent (Vault) enregistre uniquement le **sommet** du Merkle Tree (appelé la racine), qui est le résumé officiel de toute la liste.
Grâce à la preuve que l’interface a envoyée avec ta demande, le contrat peut vérifier facilement, en quelques calculs rapides, que tu fais partie de la liste sans stocker tous les détails.
Si ta preuve est valide, cela signifie que tu es bien éligible au claim.

### Étape 5 : Ton claim est validé et tu reçois tes REG

Le contrat marque ce que tu as déjà réclamé, pour éviter que tu ne réclames plusieurs fois la même chose. Ensuite, il te délivre tes REG.

## En résumé, pourquoi l'usage du Merkle Tree est pratique  ?

- **Sécurité :** Tu ne peux pas demander plus que ce à quoi tu as droit.
- **Rapidité :** La vérification de ton droit se fait en un instant, même si la liste est très longue.
- **Économie :** Le système évite d’enregistrer des tonnes d’informations sur la blockchain, ce qui rend les transactions moins coûteuses.

## ⭐⭐ Voyons maintenant, comment est constitué le Merkel Tree

Le Merkle tree est un fichier qui est construit par RealT et qui l'actualise à chaque réévaluation.
Il est disponible à l'adresse suivante : https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json
Comme il est très volumineux, il faut cliquer sur "View raw" pour le lire.
Voila un exemple de ce qu'il contient : 

	"merkleRoot": "0x7600588c716d7c1e3157369e8d71a43e1049fa5f65ed78bd1bfd667736ccf697",
	"tokenTotal": "0x182dd106b32a8ac023000",
	"claims": {
    "0x5fc96c182bb7e0413c08e8e03e9d7efc6cf0b099": {
      "amount": "0x7794362e3f5ec2e3000",
      "proof": [
        "0x79fa2e8520e46c313c3adb9559688b3092b25d8f7d59576f5717893bd4f8ff3d",
        "0xa0c7dfe0135f30390b0d7002ce5ef5179a3ff6411211c6b46359bba9643ef81f",
        "0xc22c90eff57be6ea4d49d258179a5b4bb3d1ee2fbddc366e5c5492ed43a43819",
        "0xc0a3ad5c235ece884b9ddde6aa348c34e4c195ff1c70d3af41e7bbbcfad87090",
        "0x08e11058f59c7d7fab751b1715bcf95b71b5f9f5314d50005588fcc9025c7e1a",
        "0x8758c7db9b5e944a2314af956cf8564c6a75a29b438dcef4c1c272f59ecd0635",
        "0x607be7690b4ee889e191f9f7a69ae56bba7915c6c1604370074a930f70172175",
        "0x4bfd41fabd1caff0fbef9c762bf011bf8e32153ef7656ce2d459ab1d9071874c",
        "0xf892c05488e77b7478594f2ef10e92b8b39b1e8006c72b6bb77a9aaa7b85d90e",
        "0x0decec6c5017c73465c238126fe7f9a7b499a4df5575538b754113e058007b5f",
        "0x32d88461ca135f2e5c1b9bed49462dda99de88d5c476678e2d74850b5081b8de",
        "0xa30dacaedce1007c66e347a81e1a39b6cdbdb05d5d33dafc642d258c8b59ff25",
        "0x1d9ccbe1d5a8de0906890a41c21c78605b3e9fa9475a0579641eb6cc1f03056e",
        "0x802061aa7424653daaf294dea7462302357b3c9dceb2d35dbb1c472a76b6e08b" ]
    },
    "0xc2eba74e4afebe88149b3f1ce9ac97beb2c7d1a6": {
      "amount": "0x14eb1ad47000",
      "proof": [
        "0xa2450395032033080b75c249f1394503fc295d144d349c2581b86273c1bc2cdb",
        "0x983d3cd739a2644fc1ff504f95645074ca22cf93a712d052a4a82e679882c5f9",
        "0xda5b95c7a3c216a398a0a56ef541f89fcb3bf09105efdb4431e7f94d0826b9c2",
        "0xb6032b2b751842e9f89049f39745841a607af383d540f5b5c634f7c319e5d3e6",
		....

La présentation des informations est un peu particulière (format JSON), car organisée pour etre lue par un programme informatique. C'est néanmoins assez facile à comprendre pour un humain.
A partir de la troisième ligne se succèdent les informations contenues dans les "feuilles" du Merkel Tree : 
- l'adresse de l'utilisateur : "0x5fc96c182bb7e0413c08e8e03e9d7efc6cf0b099" pour le premier, "0xc2eba74e4afebe88149b3f1ce9ac97beb2c7d1a6" pour le suivant...
- le montant reclamable : "0x7794362e3f5ec2e3000" pour le premier, "0x14eb1ad47000" pour le second..(*)
- et enfin la preuve :
C'est une succession d'empreintes (hash), qui permetent de vérifier que l'adresse et le montant font bien partie de cette liste.
Lorsque l'interface va envoyer ces informations (celles de la feuille correspondant à l'utilisateur qui fait la réclamation) au smart contract de distribution (Vault), ce dernier va faire l'opération suivante : 
  - il va assembler l'adresse et le montant et hasher le résultat,
  - puis il va combiner le hash obtenu au premier hash de preuve, puis faire cela jusqu'au dernier,
  - et enfin comparé le résultat finale à la racine du Merkel Tree qu'il possède.
Si ils sont identiques, c'est que vous avez le droit de faire cette réclamation et le smart contrat va l'executer.  

(*) Nota concenrant le montant : il est exprimé en hexadecimal. Vous pouvez le convertir en decimal avec par exemple l'application suivante :https://www.rapidtables.com/convert/number/hex-to-decimal.html
Le montant que vous obtenez est exprimé avec 18 décimales (précision de l'USDREG), donc il faut le diviser par 10 puissance 18 (ou décaler la virgule de 18 chiffres vers la gauche) pour obtenir le résultat 35293,477 USDREG !.