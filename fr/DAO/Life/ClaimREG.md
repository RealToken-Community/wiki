---
title: Réclamation des REG
description: 
published: true
date: 2025-03-26T07:29:55.092Z
tags: 
editor: markdown
dateCreated: 2025-03-19T13:49:52.142Z
---

# Préambule : Pourquoi des REG vous sont-ils attribués ? ⭐

A chaque tokenisation d’un actif émis dans l'écosystème, la DAO permet à ses partenaires de tokenisation d'attribuer un montant d'USDREG ($ convertible uniquement en REG) aux clients qui ont acheté et conservé les tokens pour une durée variable en fonction des actifs. Ces USDREG sont convertibles en REG, à tout moment au prix du marché.

Actuellement, le montant d’USDREG correspond aux frais prélevés par RealT pour son travail de recherche immobilière et est alloué lors de la première réévaluation du bien.

RealT à annoncé sa volonté de faire évoluer les critères d’attribution des droit en USDREG, pour être applicables à tout les types de tokenisations que RealT propose et pas uniquement aux biens immobilier classiques (la tokenisation de prêt, n'ayant par exemple pas de réévaluation).

Avant la création de REG, ce montant était payé en tokens SOON, dont la valeur était de 1 USDREG.

Une fois l’application de réclamation des REG mise en service, les SOON disparaîtront et l’allocation lors de la première réévaluation se fera directement en USDREG (cf RIP00009).

![valuation.png](/imag-en/regconvertor/valuation.png){.align-right .img40}

Les réévaluations des biens sont faites par des sociétés indépendantes de RealT.
Le cout de cette action devient élevé (notamment pour les multi-family) et conduit RealT à décaler ces réévaluations, qui devaient être annuelles à l’origine.

A l’avenir, la distribution d'USDREG ne devrait plus être liée à la réévaluation, mais à un délai après la tokenisation.

# L'application pour réclamer vos REG ⭐

[https://claim.realtoken.network/](https://claim.realtoken.network/)
![claim1.png](/imag-en/regconvertor/claim1.png){.align-right .img35}

1. L'application fonctionne sur la blockchain Gnosis.
2. Vous devez connecter l'un des portefeuilles suivants :
- celui déclaré comme destinataire des Realtokens, sur [Realt.co](https://realt.co/) (au lancement de la conversion) : pour réclamer les USDREG ($ convertibles en REG) correspondant aux Soon reçus sur plusieurs années.
- celui contenant les Realtokens, qui seront réévalués ultérieurement (une fois la distribution des Soon arrêtée).
3. Choisissez votre mode de présentation (langue, arrière-plan).
4. Assurez-vous d'être en mode « REG Claim » et non « Genesis ».

Remarque : Les utilisateurs de *Walletless* devront migrer vers *Realtoken Wallet ou un autre portefeuille compatible* pour se connecter à l'application, puis demander au support de faire la réclamation sur le nouveau portefeuille pour les jetons alloués au Walletless. [Lien d'aide](https://community-realt.gitbook.io/tuto-community/site-realt/option-realtoken-wallet-account-abstraction)

![claim2.png](/imag-en/regconvertor/claim2.png){.align-right .img30}
Une fois connecté, vous verrez :
1. Le montant d'USDREG qui vous est alloué (20 sur l'image),
2. La parité REG actuelle en $ (1,93),
3. Le nombre de REG correspondant (10,34) que vous pouvez réclamer (uniquement en totalité),
4. Le montant d'USDREG que vous avez déjà réclamé (100), avant une nouvelle attribution,
5. L'option de délégation (voir chapitre suivant),
6. L'option de réclamation automatique
Vous pouvez autoriser l'exécution automatique des réclamations pour vous (par un robot), dès que des USDREG vous sont alloués (délai maximal de 24 heures). Ce service peut être payant (actuellement 0 %).
<br>

## Réclamation déléguée

![delegateclaim.png](/imag-en/regconvertor/delegateclaim.png){.align-right .img45}

Il est possible de déléguer une autre adresse pour réclamer et récupérer vos REG (par exemple, en cas de portefeuille corrompu) :
1. Connectez-vous à l'application avec le portefeuille initial (0x69…94a sur l'image).
2. Cochez la case « Réclamation avec une autre adresse » et indiquez l'adresse déléguée (0x…C08 sur l'image). Signez ensuite cette autorisation de délégation avec l'adresse initiale. 
3. L'application vous indique que vous devez modifier votre adresse de connexion pour effectuer la réclamation.
4. Connectez-vous avec l'adresse déléguée (0x…C08 sur l'image).
5. Lancez la réclamation.
6. La réclamation enverra les REG attribués à l'adresse initiale vers l'adresse déléguée.
<br>

# Fonctionnement ⭐⭐
## Avant la création du contrat de récupération

Pendant la période Soon (2021 à mars 2025) et avant la création du contrat de récupération REG (Vault) :

1. Après chaque réévaluation d'un Realtoken, RealT a lancé la création des Soon correspondants,
![ccm1.png](/imag-en/regconvertor/ccm1.png){.align-right .img50}
2. Ces Soon ont été alloués aux portefeuilles des détenteurs des Realtokens au moment de la réévaluation.

Pour initialiser le contrat de récupération, les jetons Soon sont convertis en USDREG, puis détruits :
3. RealT initie la conversion des jetons Soon.
4. Collecte tous les portefeuilles contenant des jetons Soon et leur quantité.
5. Les jetons Soon sont convertis en USDREG.
Ce montant pour chaque portefeuille est enregistré dans un fichier hors chaîne ([Arbre Merkel](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json). Les jetons Soon convertis sont alloués pour chaque adresse à la dernière adresse de livraison Realtoken connue sur le site web realt.co de l'utilisateur.
6. Les jetons Soon sont détruits (burn).

Toutes les actions ci-dessus ne sont exécutées qu'une seule fois, lors de la création du contrat de récupération.

Nota : La solution du "merkel tree" a été retenue, pour le stockage des droits en USDREG de chacun, car c'est la solution généralement utilisée pour des airdrop et des distributions par claim. Cette solution, offchain, est notamment bien plus économique lorsque la quantité de bénéficiaire est importante. 

## Réclamation de l'utilisateur

Une fois le fichier « Merkle Tree » initialisé (la première fois avec les Soons accumulés) :

1. Les réclamations REG peuvent être demandées :
- depuis l'application de réclamation, qui affiche le nombre de USDREG,
- ou directement sur le contrat depuis le portefeuille de l'utilisateur (solution plus complexe).
![ccm2.png](/imag-en/regconvertor/ccm2.png){.align-right .img50}
2. La réclamation est exécutée par le contrat intelligent de conversion Vault.
3. Il vérifie, en fonction de la racine de l'arbre de Merkle qui lui est transmise, que l'utilisateur dispose des droits correspondants.
4. Il calcule le nombre d'USDREG à partir des données de l'arbre de Merkle et des USDREG déjà réclamés (dont il stocke la valeur) afin de déterminer le nombre d'USDREG pouvant être réclamés. Ce montant est ensuite converti en REG selon le prix de l'Oracle.
5. Il demande au contrat REG de créer (mint) les REG.
6. Les nouveaux REG sont transférés vers le portefeuille de l'utilisateur.
7. Les réévaluations/distributions ultérieures (après la disparition des Soons) seront effectuées directement en USDREGs, en mettant à jour l'arbre de Merkle. Cette mise à jour fera apparaître les nouveaux USDREG disponibles pour la réclamation dans l'application.
Les nouveaux USDREG seront attribués à l'adresse détenant les Realtokens au moment du snapshot (contrairement aux USDREG issus de la conversion Soon, qui sont attribués à la dernière adresse de livraison des RealTokens enregistrée sur votre compte realt.co).

Si l'adresse détenant les USDREGs est corrompue, il est possible de simplement signer l'autorisation de délégation avec l'adresse corrompue, déléguant la réclamation à une autre adresse. Cette autre adresse pourra alors réclamer et recevoir les REGs.

## Réclamation « Automatique »

L'exécution de la réclamation par le contrat intelligent Vault peut être déclenchée :
- soit manuellement, à tout moment, par l'utilisateur (la parité est alors choisie),
- soit automatiquement par un robot après l'attribution des nouveaux USDREG (la parité n'est alors pas choisie). Ce dernier mode nécessite une autorisation préalable.

1. L'automate surveillera les mises à jour de l'Arbre Merkel pour les utilisateurs enregistrés, puis déclenchera une réclamation REG pour les utilisateurs concernés.
![ccm3.png](/imag-en/regconvertor/ccm3.png){.align-right .img50}
2. Selon la configuration du contrat intelligent de conversion du Vault, des frais de réclamation automatiques peuvent être appliqués (0 % initialement) pour encourager la création d'un tel automate et couvrir les frais de transaction associés (l'activation des frais de réclamation automatique nécessitera un vote de gouvernance).
3. Le Vault demandera la création (mint) de deux montants de REG : un pour les frais et un pour l'utilisateur.
4. Le contrat REG crée les REG de frais dans le portefeuille de l'automate.
5. Le contrat REG crée les REG de l'utilisateur dans son portefeuille.

Remarque : Cette fonctionnalité ne peut pas être combinée avec une réclamation déléguée (qui est similaire à une réclamation utilisateur).

## Adresse

Contrat intelligent :

- Soon : [0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88](https://gnosisscan.io/token/0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88)
- REG : [0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce](https://gnosisscan.io/token/0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce)
- Vault : [0x71eb2b2518556700f5b778b9cc313bfe8de5086e](https://gnosisscan.io/address/0x71eb2b2518556700f5b778b9cc313bfe8de5086e)
- Oracle : [0x86339b40e588f774bd766eB70D47bEFBe68B6F64](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced)

Arbre de Merkel (hors chaîne) : [https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json)

Ce fichier peut être sauvegardé par la communauté afin de rester disponible en toutes circonstances. La dernière version, dont la racine est enregistrée dans le contrat de revendication, reste valide même si l'interface ou le fichier source n'existe plus.

# Détails du fonctionnement du contrat Vault ⭐⭐⭐

L'adresse  ci-dessus du contrat intelligent est l'adresse du proxy (invariable), qui pointe vers l'adresse d'implémentation suivante (qui elle varie en fonction des mises à jour) : [https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code)

Le programme gère la conversion et la distribution des jetons REG, en fonction des montants USDREG enregistrés dans un arbre Merkel et de la valeur REG fournie par l'Oracle.

## Fonctionnalités clés

1. **Rôles et autorisations** : Le contrat utilise le système de contrôle d’accès d’OpenZeppelin pour gérer les différents rôles (administrateur, opérateur, intervenant en pause et intervenant en mise à niveau) afin de sécuriser les opérations critiques.
2. **Gestion des jetons** : Il interagit avec le jeton REG, permettant aux utilisateurs de réclamer des jetons REG en fonction des montants USDREG, tout en vérifiant les preuves Merkle pour garantir l’éligibilité.
3. **Fonctionnalités de réclamation** : Les utilisateurs peuvent réclamer des jetons directement ou par l’intermédiaire d’un délégué. Le contrat prend également en charge les fonctionnalités de réclamation automatisée. Les réclamations *"Walletless"* existent, mais ne sont pas implémentées hors chaîne : les comptes correspondants  devront migrer vers un nouveau portefeuille.
4. **Gestion de l’horodatage** : Le contrat inclut des mécanismes permettant de vérifier que les réclamations ont lieu dans un délai défini, empêchant ainsi toute réclamation en dehors de cette période.
5. **Bridge et transferts** : Il permet également le transfert de jetons vers d’autres blockchaînes (pont) et la gestion des frais associés à ces transferts. 
6. **Mises à jour** : Le contrat est conçu pour être mis à jour via un mécanisme de mise à niveau, permettant l'ajout de nouvelles fonctionnalités ou la correction de bugs sans perdre l'état du contrat. 
7. **Événements** : Il émet des événements pour suivre les actions importantes, telles que les réclamations de jetons, les mises à jour de paramètres et les erreurs.

## Validation avec preuve Merkle

La validation avec preuve Merkle consiste à vérifier que les informations d'un utilisateur (adresse et montant) sont incluses dans une structure de données appelée arbre Merkle.

Exécution dans le programme :

1. Génération de *"feuilles"* : Pour chaque réclamation, une feuille (leaf) est générée en hachant les données de l'utilisateur (adresse et montant) avec la fonction keccak256. Cela crée un hachage unique qui représente cette réclamation.

![](/imag-en/regconvertor/rc2.png){.align-right .img50}

<br>

2. Vérification de la preuve : La fonction *\_validateMerkleProof* prend en entrée l'adresse de l'utilisateur, le montant, la racine Merkle attendue et un tableau de preuves Merkle. Elle vérifie que la preuve Merkle est valide pour l'utilisateur et le montant spécifié. 
![](/imag-en/regconvertor/rc1.png){.align-right .img50}
<br>
[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L669)
<br>
<br>

3. **Vérification de l'arbre de Merkle** : La fonction *\_verifyAsm* utilise une approche d'assemblage pour parcourir les preuves de Merkle. Elle prend chaque nœud de la preuve et le combine avec la feuille pour reconstruire le hachage jusqu'à ce qu'il atteigne la racine de Merkle. Si la racine reconstruite correspond à la racine de Merkle attendue, cela signifie que la revendication est valide. 
![](/imag-en/regconvertor/rc3.png){.align-right .img50}
<br>
[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L719)
<br>

4. **Rejet des revendications invalides** : Si la vérification échoue, le contrat rejette la revendication en générant une erreur, garantissant ainsi que seules les revendications valides, correspondant à la structure de l'arbre de Merkle, sont acceptées.

## Mode de reclamation

Les principaux modes de reclamation du contrat sont :

### Réclamation utilisateur (lignes 176 à 200)

- Fonction : *claim*
- Description : Ce mode permet à un utilisateur de réclamer des jetons REG en fonction d'un montant en USDREG. - Processus :

1. L'utilisateur appelle la fonction de réclamation avec le montant souhaité et une preuve Merkle.
2. La fonction vérifie que le contrat n'est pas suspendu et que les horodatages sont valides.
3. Elle vérifie que la preuve Merkle est valide pour l'utilisateur et le montant spécifié.
4. Elle vérifie le montant déjà réclamé par l'utilisateur afin d'éviter les doubles réclamations.
5. Si tous les contrôles sont concluants, le contrat calcule le montant de REG à émettre et émet les jetons REG pour l'utilisateur.

![](/imag-en/regconvertor/rc4.png){.align-right .img50}

[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L176)
<br>
<br>

### Réclamation par délégué (lignes 302 à 336)

- Fonction : *claimByDelegate*
- Description : Ce mode permet à un utilisateur de désigner un délégué pour réclamer des jetons en son nom.
- Processus :

1. L’utilisateur fournit les informations de réclamation (adresse du compte, montant, destinataire) et une signature pour prouver son autorisation.
2. La fonction vérifie que le contrat n’est pas suspendu et que les horodatages sont valides. 3. Elle valide la signature pour garantir que le délégué est autorisé à agir au nom de l’utilisateur.
4. Comme pour la méthode de réclamation initiée par l’utilisateur, la preuve Merkle est validée et le montant déjà réclamé est vérifié.
5. Si tout est valide, le contrat génère les jetons REG pour le destinataire.

> L’interface de cette fonction envoie le REG à l’adresse déléguée. Dans une prochaine mise à jour de l'interface, il sera possible de déléguer la réclamation sans envoyer les jetons au délégué, mais à une adresse déterminée lors de la signature. {.is-warning}

![](/imag-en/regconvertor/rc5.png){.align-right .img50}
<br>
[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L302)
<br>
<br>

### Réclamation automatique (lignes 552 à 640)

- Fonction : claimByAutoClaim
- Description : Ce mode permet aux utilisateurs de réclamer automatiquement des jetons REG sans avoir à initier manuellement la réclamation.
- Processus :

1. L'utilisateur appelle la fonction avec un tableau de structures de réclamation. 2. La fonction vérifie que le contrat n'est pas suspendu et que les horodatages sont valides. 3. Pour chaque réclamation, elle vérifie si l'auto-réclamation est activée pour le destinataire.
4. Elle valide la preuve Merkle et vérifie le montant déjà réclamé.
5. Des jetons REG sont émis pour le destinataire, et des frais peuvent s'appliquer si configurés.

![](/imag-en/regconvertor/rc6.png){.align-right .img50}

[Lien vers le code correspondant](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L552)
<br>
<br>

Les réclamations automatiques sont executées : 
- par un automate réalisé par RealT 
Le script va chercher sur Thegraph les autoclaim, les balance déjà claim, récupère le merkel, fait le tris pour ne garder que ceux qui ont des USDREG à claimer et effectue des transactions en batch pour claim. [Première execution](https://gnosisscan.io/tx/0xc839581772372c88436049828ae8cf01740c4f51c29a5d8c47c22254134ab82e)
 - ou par tout le monde, manuellement ou automatiquement. Ci-après un exemple : [autoclaim.pdf](/assets/document/autoclaim.pdf)
<br>

## Oracle de prix REG

Le contrat REG Price Oracle est basé sur le code Chainlink et comprend :

- le contrat intelligent [https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code)
- Une infrastructure Chainlink permettant d'automatiser les relevés de prix on-chain et off-chain sur plusieurs sources et d'effectuer des calculs visant à prévenir les manipulations de prix. Le calcule prend en compte les volumes d’échange, le temps passé à un prix, les différences entre les source de prix, il crée une pondération afin d'éviter les manipulations, le taux d'actualisation avec la valeur calculé est mise à jours onchain tout les 24h par un automate Chainlink.

Sur le graphique ci-après vous pouvez voir le prix moyen pondéré (Time-Weighted Average Price, TWAP) calculé par l'Oracle comparé au prix spot du REG

![oracle_vs_spot.png](/imag-en/regconvertor/oracle_vs_spot.png){.align-center .img75}

### Pourquoi utiliser ce mode de calcul pour la conversion des REG

- **Atténuer la manipulation du marché** : TWAP calcule le prix sur 24 heures, réduisant ainsi le risque de manipulation sur des marchés peu liquides comme celui de REG, où les prix spot peuvent être facilement faussés par de petites transactions.

- **Previenir les spirales descendantes** : en ne réagissant pas immédiatement aux baisses de prix, TWAP stabilise le marché et empêche les ventes paniques, qui peuvent entraîner une spirale descendante pour des tokens peu liquides.

- **Assurer la stabilité des prix** : TWAP offre un taux de conversion constant basé sur des moyennes quotidiennes, protégeant REG de la volatilité des prix spot, surtout dans des environnements à faible liquidité. Il garantit l’équité en veillant à ce que tous les utilisateurs puissent, sur une fenêtre temporelle, obtenir la même quantité de REG pour un crédit donné.

- **Supporter l’auto-équilibrage pour protéger REG** : TWAP ajuste l’émission de REG en fonction des mouvements de prix ; les utilisateurs reçoivent moins de REG lorsque les prix baissent et plus de REG lorsque les prix augmentent.

- **Conçu pour la conversion** : le mécanisme est pensé pour permettre aux utilisateurs de choisir le moment de leur conversion, plutôt que de servir d’outil de trading en temps réel.

 