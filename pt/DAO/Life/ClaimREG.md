---
title: Réclamation des REG
description: 
published: true
date: 2025-03-19T12:57:17.669Z
tags: 
editor: markdown
dateCreated: 2025-03-19T09:02:40.132Z
---

# Préambule : Pourquoi des REG vous sont alloués ⭐

  
A chaque tokenisation d’un actif émis dans l'écosystème,  la DAO permet à ses partenaires de tokenisation d'attribuer un montant d'USDREG ($ convertible uniquement en REG) aux clients qui ont acheté et conservé les tokens pour une durée variable en fonction des actifs. Ces USDREG sont convertibles en REG, à tout moment au prix du marché.

Actuellement le montant d'USDREG correspond aux frais prélevés par RealT pour le travail de sourcing des biens et est attribué au moment de la première réévaluation du bien immobilier.  
RealT à annoncé sa volonté de faire évoluer les critères d’attribution des droit en USDREG, pour être applicables à tout les types de tokenisations que RealT propose et pas uniquement aux biens immobilier classiques (la tokenisation de prêt, n'ayant par exemple pas de réévaluation).

Avant la création du REG, ce montant était versé en token SOON, dont la valeur était 1 USDREG.  
  
Une fois l’application de réclamation des REG mise en service, les SOON disparaîtront et l’allocation lors de la première réévaluation se fera directement en USDREG (cf RIP00009). 

![valuation.png](/imag-en/regconvertor/valuation.png){.align-right .img40}

Les réévaluations des biens sont faites par des sociétés indépendantes de RealT.  
Le cout de cette action devient élevé (notamment pour les multi-family) et conduit RealT à décaler ces réévaluations, qui devaient être annuelles à l’origine.

A l’avenir, la distribution d'USDREG ne devrait plus être liée à la réévaluation, mais à un délai après la tokenisation.  
 
# L'application pour réclamer vos REG ⭐

[https://claim.realtoken.network/](https://claim.realtoken.network/)
![claim1.png](/imag-en/regconvertor/claim1.png){.align-right .img35}

1.  L’application fonctionne sur la blockchain Gnosis,
2.  Vous devez connecter l'un des wallets suivants :    
     - celui déclaré comme recevant les Realtokens, sur [Realt.co](https://realt.co/) (au moment du lancement de la conversion) : pour réclamer les USDREG ($ convertible en REG) qui correspondent aux Soon reçus depuis plusieurs années,  
    - celui possédant les Realtokens, qui seront réévalués par le suite (une fois la distribution de Soon stoppée).
3.  Choisissez votre mode de présentation (langue, fond),
4.  Vérifier que vous êtes bien en « REG Claim » et pas « Genesis ».

Nota : Les utilisateurs en *Walletless* devront migrer vers le *Realtoken Wallet ou autre wallet compatible* pour se connecter à l'application, puis demander au support d'effectuer la réclamation sur le nouveau Wallet pour les tokens attribués au Walletless.  
[Lien d'aide](https://community-realt.gitbook.io/tuto-community/site-realt/option-realtoken-wallet-account-abstraction)  
 
![claim2.png](/imag-en/regconvertor/claim2.png){.align-right .img30}
Une fois connecté, vous voyez : 
1.  Le montant en USDREG qui vous est alloué (20 sur l’image),
2.  La parité du REG en $ du moment (1,93),
3.  Le nombre de REG correspondant (10,34), que vous pouvez claimer (qu'en totalité),
4.  Le montant d'USDREG que vous avez déjà claimé (100), avant une nouvelle attributionn,
5.  L’option de délégation (voir chapitre suivant),
6.  L’option d’Auto Claim  
    Vous pouvez autoriser l’exécution de claim automatique à votre attention (par un robot), dès que des USDREG vous seront alloués (délais max 24h).   
    Des frais pourront être prélevés pour ce service (0 % actuellement).  
<br>
     
## Claim délégué

![delegateclaim.png](/imag-en/regconvertor/delegateclaim.png){.align-right .img45}

 Il est possible de déléguer une autre adresse, pour réclamer et percevoir vos REG (par exemple en cas de wallet corrompu) : 
1.  Connection à l'application avec le wallet initial (0x69…94a sur l'image),
2.  Cocher la réclamation avec une autre adresse, et indiquer l'adresse déléguée (0x…C08 sur l'image). Puis signer cette autorisation de délégation avec l'adresse initiale.
3.  L'application indique que vous devez changer d'adresse de connexion pour claimer,
4.  Connectez vous avec l'adresse déléguée (0x…C08 sur l'image),
5.  Lancer le claim,
6.  Le claim enverra les REG, alloués à l'adresse initiale, vers l'adresse déléguée. 
<br>

# Comment ça marche ⭐⭐

## Avant la création du contrat de réclamation

Pendant la période des Soon (2021 à mars 2025) et avant la création du contrat de réclamation des REG (Vault): 


1. A l’issu de chaque réévaluation d’un Realtoken, RealT a lancé le Mint des Soon correspondants,
![ccm1.png](/imag-en/regconvertor/ccm1.png){.align-right .img50}
2. Ces Soon ont été attribués aux wallets des propriétaires du Realtoken au moment de la réévaluation.

Pour initialiser le contrat de réclamation, les Soon sont convertis en USDREG, puis détruits : 
3. RealT lance la conversion des Soon,
4. Collecte de tous les wallets ayant des Soon avec leur quantité,
5. Les Soon sont convertis en USDREG, 
Ce montant pour chaque wallet, est inscrit dans un fichier off-chain ([Merkel Tree)](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json), les Soon convertis sont attribués pour chaque adresse à la dernière adresse de livraison des Realtokens connu sur le site realt.co de l'utilisateur,
6. Les tokens Soon sont détruits (burn).  

L’ensemble des actions ci-avant sont exécutées qu’une seule fois, à la création du contrat de réclamation.
 
## Réclamation par l'utilisateur

Une fois le fichier « Merkle Tree » initialisé (la première fois à partir des Soon accumulés) : 

1.  La réclamation des REG peut être demandée :   
    \- à partir de l’application de claim qui affiche le nombre d'USDREG,  
    \- ou directement sur le contrat partir du wallet utilisateur (solution plus complexe).
![ccm2.png](/imag-en/regconvertor/ccm2.png){.align-right .img50}
2.  La réclamation est exécutée par le smart contract Vault de conversion,
3.  Il vérifie à partir de la racine du Merkle Tree, qui lui a été transmise, que l’utilisateur à les droits correspondants,
4.  Il calcul le nombre d'USDREG à partir des données du Merkle Tree et des USDREG déjà réclamés (dont il stocke la valeur) pour déterminer le nombre d'USDREG qui peuvent être réclamé, ce montant est en suite converti en nombre de REG à partir du prix de l'Oracle,
5.  Il demande au contrat REG de créer (mint) les REG,
6.  Les nouveaux REG sont transférés sur wallet de l’utilisateur.
7.  Les réévaluations/distributions qui suivront (après disparition des Soon) se feront directement en USDREG, par mise à jour du Merkel Tree. Cette mise à jour fera apparaître de nouveaux USDREG à réclamer dans l’application.  
    Les nouveaux USDREG seront attribués à l'adresse qui détient des Realtokens au moment du snapshot (contrairement au droit en USDREG issus de la conversion Soon, qui sont attribués à la dernière adresse de livraison de RealTokens enregistrée sur votre compte realt.co)

Si l'adresse dotée d'USDREG a été corrompue, il est possible de simplement signer avec l'adresse corrompue le fait de déléguer la réclamation à une autre adresse. Cette autre adresse, pourra alors réclamer les REG et les recevoir. 

## Réclamation « automatique »

L’exécution de la réclamation par le smart contract Vault peut être déclenché : 
- soit manuellement, à tout moment, par l’utilisateur (la parité étant alors choisie), 
- soit automatiquement par un automate (bot) après l'attribution de nouveaux USDREG (la parité n'est alors pas choisie). Ce dernier mode, necessite une autorisation préalable.

1.  L’automate va surveiller les mises à jour du Merkel Tree pour les utilisateurs inscrits, puis déclencher une réclamation des REG pour les utilisateurs concernés,
![ccm3.png](/imag-en/regconvertor/ccm3.png){.align-right .img50}
2.  Suivant le paramétrage du smart contract Vault de conversion, des frais de réclamation automatique pourront être appliqués (0 % au départ), afin d'encourager la création de tel automate et prendre en charge les frais de transaction associés (l'activation de frais, devra faire l'objet d'un vote de gouvernance)
3.  Le Vault demandera le mint de deux quantités de REG : une pour les frais et une pour l’utilisateur,
4.  Le contrat REG mint les REG de frais vers le wallet de l’automate,
5.  Le contrat REG mint les REG de l’utilisateur vers le wallet de l'utilisateur.

Nota : Cette fonction n'est pas cumulable avec une réclamation déléguée (qui s'apparente à une réclamation par l'utilisateur).

## Adresse

Smart contract :

-   Soon : [0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88](https://gnosisscan.io/token/0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88)
-   REG : [0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce](https://gnosisscan.io/token/0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce)
-   Vault : [0x71eb2b2518556700f5b778b9cc313bfe8de5086e](https://gnosisscan.io/address/0x71eb2b2518556700f5b778b9cc313bfe8de5086e)
-   Oracle : [0x86339b40e588f774bd766eB70D47bEFBe68B6F64](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced)

Merkel Tree (OffChain) : [https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg\_convertion/current.json](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json)

Ce fichier peux être sauvegardé par la communauté pour rester disponible dans toutes les situations. La dernière version, dont le root est inscrit dans le contrat de claim, reste valable même si l'interface ou le fichier source n'existe plus.

#  Détail de fonctionnement du contrat Vault ⭐⭐⭐
  
L’adresse du smart contract ci-dessus est celle du Proxy (invariable) qui pointe sur l’implementation suivante  (qui peut varier, suivant les mises à jour)  : [https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code)

Le programme gère la conversion et la distribution de tokens REG, basés sur des montants en USDREG enregistrés sur un Merkel Tree et la valeur du REG fournis par l'Oracle.

## Principales fonctionnalités

1.  **Rôles et Permissions** : Le contrat utilise le système de contrôle d'accès d'OpenZeppelin pour gérer différents rôles (administrateur, opérateur, pauser, et upgradeur) afin de sécuriser les opérations critiques.
2.  **Gestion des Tokens** : Il interagit avec le token REG, en permettant aux utilisateurs de réclamer des tokens REG en fonction de montants en USDREG, tout en vérifiant les preuves de Merkle pour garantir l'éligibilité.
3.  **Fonctionnalités de Réclamation** : Les utilisateurs peuvent réclamer des tokens, soit directement, soit par l'intermédiaire d'un délégué. Le contrat gère également des fonctionnalités de réclamation automatique. La réclamation Walletless existe, mais n'est pas implémenté offChain : les comptes Walletless devront migrer sur un nouveau wallet.
4.  **Gestion des Timestamps** : Le contrat inclut des mécanismes pour vérifier que les réclamations se produisent dans une période de temps définie, empêchant les réclamations en dehors de cette période.
5.  **Bridge et Transferts** : Il permet également de transférer des tokens vers d'autres chaînes (bridge) et de gérer les frais associés à ces transferts.
6.  **Mises à Jour** : Le contrat est conçu pour être mis à jour via un mécanisme d'upgrade, permettant d'ajouter de nouvelles fonctionnalités ou de corriger des bugs sans perdre l'état du contrat.
7.  **Événements** : Il émet des événements pour suivre les actions importantes, comme les réclamations de tokens, les mises à jour de paramètres, et les erreurs.

## Validation avec preuve de Merkel

La validation avec preuve de Merkle fonctionne en vérifiant que les informations d'un utilisateur (adresse et montant) sont bien incluses dans une structure de données appelée arbre de Merkle. 

Execution dans le programme :

1. **Génération de la Feuille** : Pour chaque réclamation, une « feuille » (leaf) est générée en hachant les données de l'utilisateur (adresse et montant) avec la fonction keccak256. Cela crée un hachage unique qui représente cette réclamation. 
![](/imag-en/regconvertor/rc2.png){.align-right .img50}

<br>

2. **Vérification de la Preuve** : La fonction *\_validateMerkleProof* prend en entrée l'adresse de l'utilisateur, le montant, la racine de Merkle attendue et un tableau de preuves de Merkle. Elle vérifie que la preuve de Merkle est valide pour l'utilisateur et le montant spécifié.
![](/imag-en/regconvertor/rc1.png){.align-right .img50}
<br>
[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L669)
 <br>
 <br>

3. **Vérification de l'Arbre de Merkle** : La fonction *\_verifyAsm* utilise une approche d'assemblage pour parcourir les preuves de Merkle. Elle prend chaque nœud de la preuve et le combine avec la feuille pour reconstruire le hachage jusqu'à atteindre la racine de Merkle. Si la racine reconstruite correspond à la racine de Merkle attendue, cela signifie que la réclamation est valide.
![](/imag-en/regconvertor/rc3.png){.align-right .img50}
<br>
[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L719)
<br>
  
4. **Rejet des Réclamations Invalides** : Si la vérification échoue, le contrat rejette la réclamation en lançant une erreur, garantissant ainsi que seules les réclamations valides, qui correspondent à la structure de l'arbre de Merkle, sont acceptées.  
 

## Mode de réclamation

Les principaux modes de réclamation dans le contrat, sont :

### Réclamation par l'utilisateur (lignes 176 à 200)

-   Fonction : *claim*
-   Description : Ce mode permet à un utilisateur de réclamer des tokens REG en fonction d'un montant en USDREG.
-   Processus :

	1.  L'utilisateur appelle la fonction claim avec le montant qu'il souhaite réclamer et une preuve de Merkle.
	2.  La fonction vérifie que le contrat n'est pas en pause et que les timestamps sont valides.
	3.  Elle vérifie que la preuve de Merkle est valide pour l'utilisateur et le montant spécifié.
	4.  Elle vérifie le montant déjà réclamé par l'utilisateur pour éviter les doubles réclamations.
	5.  Si toutes les vérifications passent, le contrat calcule le montant de REG à émettre et effectue la minti des tokens REG pour l'utilisateur.

![](/imag-en/regconvertor/rc4.png){.align-right .img50}

[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L176)
<br>
<br>

### Réclamation par un délégué (lignes 302 à 336)

-   Fonction : *claimByDelegate*
-   Description : Ce mode permet à un utilisateur de désigner un délégué pour réclamer des tokens en son nom.
-   Processus :

	1.  L'utilisateur fournit des informations de réclamation (adresse du compte, montant, destinataire) et une signature pour prouver l'autorisation.
	2.  La fonction vérifie que le contrat n'est pas en pause et que les timestamps sont valides.
	3.  Elle valide la signature pour s'assurer que le délégué est autorisé à agir au nom de l'utilisateur.
	4.  Comme dans le mode de réclamation par l'utilisateur, la preuve de Merkle est validée, et le montant déjà réclamé est vérifié.
	5.  Si tout est valide, le contrat mint les tokens REG pour le destinataire.

> L'interface pour cette fonction envoie les REG à l'adresse déléguée.
Dans une future mise à jour de l'interface : il sera possible de déléguer le claim, sans faire envoyer les tokens au délégué, mais à une adresse déterminée au moment de la signature.
{.is-warning}

![](/imag-en/regconvertor/rc5.png){.align-right .img50}
<br>
[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L302)  
<br>
<br>


### Réclamation automatique (lignes 552 à 640)

-   Fonction : claimByAutoClaim
-   Description : Ce mode permet aux utilisateurs de réclamer automatiquement des tokens REG sans avoir à initier manuellement la réclamation.
-   Processus :

	1.  L'utilisateur appelle la fonction avec un tableau de structures de réclamation.
	2.  La fonction vérifie que le contrat n'est pas en pause et que les timestamps sont valides.
	3.  Pour chaque réclamation, elle vérifie si l'auto-réclamation est activée pour le destinataire.
	4.  Elle valide la preuve de Merkle et vérifie le montant déjà réclamé.
	5.  Les tokens REG sont mintés pour le destinataire, et des frais peuvent être appliqués si configurés.

![](/imag-en/regconvertor/rc6.png){.align-right .img50}

[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L552)
<br>
<br>

## Oracle de prix REG

Le contrat de l'Oracle de prix du REG est basé sur le code de Chainlink, il comprend :

-   le smart contract [https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code) 
-   une infrastructure Chainlink pour automatiser les relevés de prix onchain et offchain sur plusieurs sources et effectuer un calcule qui a pour objectif d’éviter les manipulations de prix.  
    Le calcul prend en compte les volumes d’échanges, le temps passé à un prix, les différences entre les sources de prix, il crée une pondération afin d'éviter les manipulations, le taux d'actualisation avec la valeur calculée est mise à jours onchain tout les 24h par un automate Chainlink.
    