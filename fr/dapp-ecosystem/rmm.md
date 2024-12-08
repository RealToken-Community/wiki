---
title: RMM
description: 
published: true
date: 2024-12-08T22:18:27.660Z
tags: rmm
editor: markdown
dateCreated: 2024-12-08T21:03:58.118Z
---

# **RMM v3**

## **1. Introduction au Wrapper RMM**

#### **⭐ Pour les novices**

Le Wrapper RMM est un contrat intelligent qui sert d'interface entre les RealTokens et le protocole RMM (RealToken Market Maker). Il permet aux utilisateurs de déposer leurs RealTokens et de recevoir en échange des RTW (RealToken Wrapped) qui sont utilisés dans le RMM comme preuve de dépôt pour emprunter.

#### **⭐⭐ Pour les initiés**

Le Wrapper joue un rôle crucial dans l'écosystème RMM en :

- Standardisant l'interface des RealTokens pour le protocole RMM
- Permettant d'étendre la capacité en nombre de token du RMM limité de base a 128 tokens, il permet de déposer 2000 tokens par Wrapper, soit environ 100x2000 tokens
- Pour chaque RealToken déposé, le Wrapper crée des RTW (RealToken Wrapped) d'une valeur de 1$ par RTW, est utilisé dans le RMM comme dépôt pour emprunter.
- Le wrapper crée et dépose autant de RTW que la valeur (en $) des RealTokens déposés.
  Le déposant reçoit autant de armmRTW (preuve de dépôt) que la valeur de ses RealTokens déposés..
- Le wrapper gère les balances de RTW en fonction de la valeur de chaque RealToken en créant ou brulant des RTW pour garder la valeur déposée (en RTW) toujours égale à la valeur déposée en RealToken.

![rmm_update.drawio.svg](/assets/img/rmm_update.drawio.svg)

## **2. Modifications majeures apportées aux wrapper par la v3**

adresse du wrapper : 0x10497611Ee6524D75FC45E3739F472F83e282AD5

Ces modifications font suite à la proposition de gouvernance RIP00008 qui a été acceptée par la DAO en date du 2024-12-07 21h19 UTC.

[RIP00008 - Transfer RealToken Wrapper RMMv3 execution functions to the DAO](https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposal/4412019256781079844728885554420538992900805587725535743224739658055634526928)

#### **⭐ Pour les novices**

Les principales modifications apportées au Wrapper ont été effectuées pour garantir la conformité légale et les opérations courantes liées à la vie d'un RealToken.

Dans certains cas, l'émetteur peux être amené à devoir détruire des Realtokens :

- Piratage ou perte d'accès à d'addresse du propriétaire,
- Décès du propriétaire,
- Vente de la maison liée au Realtoken,
- Remboursement d'un partie ou l'intégralité de la dette liée au Realtoken,

ce qui pourrait rendre le RMM dysfonctionnel, pour les Realtoken correspondants, car il n'y aurait plus de collatéral pour couvrir les dettes contractés.

Dans l'objectif de pouvoir répondre à ces obligations et assurer la continuité et le bon fonctionnement du RMM, de nouvelles fonctionnalités ont été ajoutées au Wrapper pour rembourser partiellement ou intégralement une dette liée à un Realtoken sur un compte et d'extraire de force le Realtoken concerné des comptes concernés.

Ce processus est supervisé par le vote de la DAO pour s'assurer qu'il n'y a pas d'abus.
Il garantie que le montant du par la détokenisation soit correctement perçu par le propriétaire, que le RMM pourra continuer de fonctionner et de respecter les obligations légales.

#### **⭐⭐⭐ Pour les experts**

Dans l'objectif que conserver un bon fonctionnement du RMM, les modifications apportées au Wrapper ont été effectuées pour permettre de récupérer les RealTokens liés à une offre en cas de décès du propriétaire, de vente de la maison liée au Realtoken, du remboursement d'un partie ou l'intégralité de la dette liée au Realtoken, de la détokenisation ou d'autres événements rendant l'actif sous-jacent inéligible au dépôt dans le RMM.

Pour récupérer ou forcer le retrait des RealTokens du protocole RMMv3, deux fonctions sont nécessaires :

- `repayForRecover` : Cette fonction gère le remboursement partiel ou intégral de la dette qui peut être active sur le compte d'un utilisateur du RMM (dans les cas comme le décès, détokenisation, autres...). Une fois exécutée, la seconde fonction peut être appelée en toute sécurité pour extraire les RealTokens du RMM.
- `recoverByGovernance` : Cette fonction récupère les RealTokens, soit en les retirant vers une adresse spécifiée (adresse de service pour burn), soit en déplaçant les aTokens/Realtokens vers l'adresse spécifiée (nouvelle adresse de l'utilisateur).

Ces deux fonctions ne peuvent être appelées que par la gouvernance de l'écosystème (REG DAO). Une proposition doit être soumise dans le processus actif de la DAO et inclure l'appel à ces deux fonctions ainsi que les approuves utiles à son exécution, correctement configurées pour chaque adresse nécessitant une récupération (batch d'adresse et de tokens).

Ces fonctions sont nécessaires dans les cas suivants :

- Piratage de l'adresse du déposant (voir note spécial sur le sujet des adresses piratées).
- Perte d'accès à l'adresse du déposant.
- Décès du déposant.
- Détokenisation ou autres événements rendant l'actif sous-jacent inéligible au dépôt dans le RMM.

Contrôle d'accès : Ces deux fonctions sont limitées aux contrats de gouvernance, seule une proposition approuvée par la communauté de gouvernance de l'écosystème RealToken (REG) peut les activer, cette mesure permet d'avoir une surveillance de l'usage d'une fonctionnalité qui peut être dangereuse, et de donner une transparence totale.

## **3. Enjeux et considérations**

#### **⭐ Pour les novices**

Ces modifications donnent un droit total sur les tokens déposés dans le RMM, il est donc vital que toute soumission d'activation de ces fonctionnalités soit minutieusement analysée par la DAO pour s'assurer qu'il n'y a pas d'abus.  
En principe, seul RealT ou un partenaire de la DAO approuvé qui emmétrait des tokens devrait initialiser de telle proposition.  
La DAO doit donc veiller à ce que ces fonctionnalités soient activées uniquement dans des cas le demandant expressément pour respecter une réglementation ou et garantir le bon fonctionnement du RMM.

#### **⭐⭐ Pour les initiés**

L'usage abusif de ces fonctionnalités pourrait conduire à des voles de tokens, des remboursements de dettes abusifs, des emprunts non légitimes, portant atteinte à la sécurité du RMM et à ses utilisateurs.

Les principaux enjeux liés au Wrapper sont :

- Protection des RealTokens déposés.
- Prévention des attaques potentielles.
- Préserver le bon fonctionnement du RMM.
- Garantir le respect des obligations légales.
- Pouvoir redonner la propriété des RealTokens déposés à l'utilisateur qui aurait perdu accès à son compte.

> Il est crucial de comprendre que le Wrapper est un composant central du RMM et que toute modification, usage de ses fonctionnalités doit être soigneusement évaluée.
> {.is-warning}

#### **⭐⭐⭐ Pour les experts**

Le Wrapper RMM v3 est un contrat central dans l'extension des capacités du RMM et il conserve la majeur partie de la valeur mis en garantie des dettes du RMM.  
Chaque modification ou usage de ces fonctionnalités doit être analysé minutieusement pour s'assurer qu'il n'y a pas d'abus, ou d'erreur qui pourrait endommager le RMM.

Les principales considérations sont :

1. La sécurité des fonds déposés dans le RMM

- Une erreur dans le code ou un mauvais usage de la fonctionnalité pourrait endommager le RMM ou créer des pertes pour les utilisateurs.

2. La protection des utilisateurs et des investisseurs

- La DAO a un rôle majeur à jouer dans la protection des utilisateurs et des investisseurs, elle doit veiller à ce que les fonctionnalités soient utilisées de façon légitime et conforme à la réglementation.

3. La préservation de la valeur des RealTokens déposés

- Le Wrapper RMM v3 est le pont entre les RealToken et le RMM, c'est lui qui gère la valeur des RealTokens déposés dans le RMM et donc la valeur mis en collatéral des dettes du RMM.

4. La gestion des risques liés aux dettes et aux emprunts

- Une erreur de code ou un mauvais usage de la fonctionnalité pourrait entrainer une mauvaise évaluation de la valeurs déposer dans le RMM en collatéral et donc permettre d'emprunter plus que la valeur déposée en RealToken, créant une sous collatéralisation des dettes du RMM. (bad debt)

5. Tentative de fraude

- Un utilisateur mal intentionné pourrait tenter de frauder le système en effectuant une proposition qui d'apparence ne pose pas de problème ou serait même une excellente idée, mais qui en réalité tenterait : de payer ses dettes avec la trésorerie de la DAO, de voler des RealTokens à des utilisateurs, ou de mettre en position de liquidation des utilisateurs.

## **4. ⭐⭐⭐ informations Techniques**

### **4.1. repayForRecover**

#### **Paramètres :**

- `repayWallet` (address) : L'adresse détenant les RealTokens dans le RMMv3 et dont les tokens seront extrait du RMM sur lesquels une dette est active.
- `refundWallet` (address) : L'adresse qui recevra tout excédent de paiement après le remboursement de la dette.
- `percent` (uint256) : Le pourcentage (échelle 100% = 10000) de chaque token déposé qui sera extrait du RMM.
- `recoverAssets` (address[]) : Un tableau d'adresses de tokens représentant les RealTokens qui seront extrait du RMMv3.
- `debtAssets` (address[]) : Un tableau d'adresses de tokens représentant les actifs de dette à rembourser.
- `payer` (address) : L'adresse responsable du paiement de la dette,  
  ! important : cette adresse doit avoir une balance des tokens debtAssets supérieur ou égale à la dette à rembourser et avoir une approval de la fonction `transferFrom` de chaque token debtAssets au bénéfice du Wrapper.

Note : La valeur des actifs quels qu'ils soient dans le RMM est calculé en $USD, ainsi il est possible qu'un USDC/XDAI ou autre ne soit pas exactement a 1$USD, il est donc important de bien calculer la balance du `payer` avec une marge de sécurité qui peux être raisonnablement de 5/100k.

#### **Comportement de l'exécution de la fonction :**

- Vérifier la dette sur repayWallet.
- Calculer le nombre de tokens par rapport au solde et au pourcentage sur lequel l'action sera exécutée.
- Calculer le montant du collatéral fourni correspondant aux adresses dans [tokens address].
- Utiliser transferFrom pour transférer le montant du/des token(s) à rembourser du payeur vers le Wrapper (l'approbation doit être faite au préalable).
- Remboursement :
  - S'il n'y a pas de dette, transférer le montant total calculé du collatéral vers le refundWallet.
  - S'il existe une dette, utiliser la fonction repay du RMMv3. Deux scénarios sont possibles :
    - Dette ≥ Collatéral : La dette est remboursée à hauteur du montant disponible lié à la valeur des tokens qui seront extraits du RMM.
    - Dette < Collatéral : La dette est entièrement remboursée, et le surplus est envoyé au refundWallet.

Note : si la fonciton repay est utilisée pour un seul utilisateur avec une ou plusieurs adresse, le `refundWallet` peux etre directement une adresse de l'utilisateur.
Dans le cas d'usage en batch pour plusieurs utilisateur, le `refundWallet` doit etre une adresse de service qui permettera de distribuer les montants correctes à chaque utilisateur pour qui le montant de remboursement n'a pas été totalement consommé par le remboursement de la dette.

#### **Validation des Entrées :**

- S'assure que les tableaux recoverAssets et debtAssets ne sont pas vides.
- Valide que le paramètre percent est dans la plage valide (supérieur à 0 et inférieur ou égal à 10000).
- Valide que repayWallet, refundWallet, et payer ne sont pas des adresses nulles.

#### **Calcul de la Dette :**

- Récupère la dette totale du repayWallet en utilisant POOL.getUserAccountData.
- Calcule le montant total du collatéral à récupérer basé sur les recoverAssets spécifiés et le pourcentage.

#### **Remboursement de la Dette :**

- Si aucune dette n'est trouvée, le collatéral est transféré au refundWallet en portions égales à travers les debtAssets spécifiés.
- Si une dette est présente, elle est remboursée proportionnellement en utilisant le collatéral disponible, et tout excédent de collatéral est transféré au refundWallet.

#### **Événements :**

- Émet un événement RepayForRecover, capturant les paramètres clés et les résultats de l'exécution de la fonction.

#### **Considérations de Sécurité :**

- Contrôle d'Accès : La fonction est restreinte pour n'être appelée que par les adresses ayant le GOVERNANCE_ROLE.
- Vérifications d'Adresse Nulle : La fonction vérifie les adresses nulles pour prévenir les transactions invalides.
- Précision Décimale : La fonction gère les montants avec une précision appropriée basée sur les décimales des actifs impliqués.

#### **Risques Potentiels :**

- Une valeur de pourcentage incorrecte pourrait conduire à des transferts non intentionnels.
- Le payeur doit avoir un solde approuvé suffisant pour que les transferts réussissent.
- La fonction ne gère pas le Facteur de Santé (HF), qui doit être géré en externe.

#### **Validation des données par la gouvernance :**

- S'assurer que les tableaux recoverAssets et debtAssets ne sont pas vides et sont bien les adresses des tokens et non des aTokens.
- Valider que le paramètre percent est dans la plage valide (supérieur à 0 et inférieur ou égal à 10000).
- Valider que repayWallet, refundWallet et payer sont des adresses non nulles.
- Valider que le `payer` a une balance des tokens debtAssets supérieur ou égale à la dette à rembourser et avoir une approval de la fonction `transferFrom` de chaque token debtAssets au bénéfice du Wrapper.

### **4.2. recoverByGovernance**

L'activation de cette fonction permet l'extraction des RealTokens d'un compte utilisateur, toutefois pour que la transaction n'échoue pas, il est nécessaire que le nombre de realtoken à extraire soit permis par le niveau de collatéral du compte.
Il est donc important de bien calculer la valeur des tokens à extraire et son impact sur le niveau de collatéralisation, si il est insufisant pour executer avec succès la fonction, il est impératif d'executer la fonction `repayForRecover` avant de pouvoir executer `recoverByGovernance`.

Note : Il est impétatif d'executer la fonction `repayForRecover` et `recoverByGovernance` dans la meme transaction, afin de prévenir une attaque de double dépenses dans la quelle l'utilisateur réussirait à emprunter après l'execution de la fonction `repayForRecover` (qui rembouree la dette) et l'execution de la fonction `recoverByGovernance` (qui extrait les RealTokens), cette dernière échouerait.

#### **Paramètres :**

- `oldWallet` (address) : L'adresse détenant les RealTokens dans le RMMv3 et dont les tokens seront extrait du RMM
- `newWallet` (address) : L'adresse qui recevra les Realtokens ou RTW associés.
- `percent` (uint256) : Le pourcentage (échelle 100% = 10000) de chaque realtoken déposer qui sera extrait du RMM.
- `recoverAssets` (address[]) : Un tableau d'adresses de tokens représentant les RealTokens qui seront extraits du RMMv3.
- `withdraw` (bool) : valeur booléen qui détermine si les tokens seront retirés du RMM ou déplacés vers la nouvelle adresse. True : les Realtokens seront retirés du RMM; False : les RTW seront déplacés vers la nouvelle adresse.

Note : Dans une proposal qui est executée pour plusieurs utilisateurs, il faudra autant d'appel à la fonction qu'il y a d'utilisateurs pour lesquels il faut extraire les RealTokens.

#### **Comportement Attendu :**

- Calculer le nombre de tokens relatif au solde et au pourcentage sur lequel l'action sera exécutée.
- Transférer tous les RealTokens ou aTokens de oldWallet vers newWallet, basé sur [tokens address].
- Si withdraw = true, effectuer un retrait du RMM ; si false, déplacer les aTokens.

#### **Validation des Entrées :**

- S'assure que le tableau recoverAssets n'est pas vide.
- Valide que le paramètre percent est dans la plage valide (supérieur à 0 et inférieur ou égal à 10000).
- Valide que oldWallet et newWallet ne sont pas des adresses nulles.

#### **Récupération des Tokens :**

- Calcule le montant des RealTokens à récupérer basé sur les recoverAssets spécifiés et le pourcentage.
- Si withdraw est vrai, les RealTokens sont retirés du RMMv3 et transférés vers le newWallet.
- Si withdraw est faux, les aTokens sont transférés de oldWallet vers newWallet.

#### **Événements :**

- Émet un événement RecoverByGovernance, capturant les paramètres clés et les résultats de l'exécution de la fonction.

#### **Considérations de Sécurité :**

- Contrôle d'Accès : La fonction est restreinte pour n'être appelée que par les adresses ayant le GOVERNANCE_ROLE.
- Vérifications d'Adresse Nulle : La fonction vérifie les adresses nulles pour prévenir les transactions invalides.
- Gestion des Tokens Excédentaires : Si plus de tokens sont transférés que nécessaire, l'excédent est retransféré vers oldWallet.

#### **Risques Potentiels :**

- Une valeur de pourcentage incorrecte pourrait résulter en un montant incorrect de tokens transférés.
- S'assurer d'une liquidité suffisante dans le Pool pour les retraits est critique pour éviter les échecs de transaction.
- Toute différence dans les soldes de tokens doit être gérée en dehors de la fonction, car elle suppose des préconditions correctes.

#### **Validation des données par la gouvernance :**

- S'assurer que les tableaux recoverAssets et debtAssets ne sont pas vides et sont bien les adresses des tokens et non des aTokens.
- Valider que le paramètre percent est dans la plage valide (supérieur à 0 et inférieur ou égal à 10000).
- Valider que les tokens sortant de oldWallet vont bien à la destination prévue qui est dans la proposition (vérifier la valeur de newWallet).

## **5. Note Importante :**

Les fonctions `repayForRecover` et `recoverByGovernance` ne gèrent pas le Facteur de Santé (HF). Par conséquent, il faut s'assurer au préalable que le HF permet le retrait, sans quoi la proposal dans son intégralité échouera.

La fonction `recoverByGovernance` ne vérifie pas les dettes existantes. Cela doit être anticipé dans la proposition. La fonction `repayForRecover` doit être exécutée au préalable si nécessaire pour éviter un échec.

Les adresses qui à un moment récéptionnent des RealTokens, doivent impérativement respecter les règles de la compliance registry de ces tokens, sans quoi la proposal dans son intégralité échouera.

Les appouvals, et solde de balance pour le paiement des dettes doivent impérativement inclure un marge minimum de 5/100k, sans quoi la proposal dans son intégralité échouera si la valeur du token de dette à rembourser à une valeur inférieur a 1$USD.

## **6. Modifications**

Liste des modifications dans la mise à jour de RealTokenWrapper :

- Suppression de la vérification \_isRealTokenWhitelisted pour la fonction withdraw (ligne 171) : puisqu'elle vérifie déjà le solde de tokens de l'utilisateur dans RealTokenWrapper. S'il y a un solde > 0, cela signifie que le RealToken était whitelisté. Il devrait permettre le retrait et ne pas permettre l'approvisionnement si le RealToken n'est plus whitelisté maintenant.

```
if (!_isRealTokenWhitelisted[asset]) revert WrapperErrors.TokenNotWhitelisted(asset);
```

- Utilisation du tableau de tokens en cache de l'utilisateur pour l'optimisation du gas (\_tokenListOfUser au lieu de \_realTokenList)

Changement de la lecture directe depuis le mapping de stockage \_realTokenList

```
uint256 length = _realTokenList.length;
uint256 userIndex = 0;
```

vers la copie de \_tokenListOfUser[user] en mémoire (cette liste est individuelle pour chaque utilisateur) (lignes 484-485)

```
// Cache user token list for gas optimization
address[] memory userTokenList = _tokenListOfUser[user];
uint256 length = userTokenList.length;
```

- Suppression de tous les anciens recoverByGovernance et ajout des 2 nouvelles fonctions repayForRecover/recoverByGovernance (lignes 643 à 870)

> IMPORTANT : la validation d'une proposal qui executera les fonctions `repayForRecover` et `recoverByGovernance` doit être faite avec une grande attention, car elle peux etre très dangereuse si elle n'est pas correctement configurée ou utilisée à des fins malveillantes.
> {.is-warning}

## **7. Audit**

Réalisé par la société ADBK : https://github.com/abdk-consulting/audits/tree/main/realt
