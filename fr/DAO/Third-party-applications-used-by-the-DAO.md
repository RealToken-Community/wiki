---
title: Application tiers utilisée par la DAO
description: 
published: true
date: 2025-01-03T12:06:31.840Z
tags: 
editor: markdown
dateCreated: 2025-01-02T17:32:35.677Z
---

## Introduction à Sablier.com

#### **⭐ Pour les novices**

Sablier.com est une plateforme de finance décentralisée (DeFi) qui permet de gérer des flux de paiements continus sur la blockchain. Imaginez un robinet qui verse de l'argent goutte à goutte, en temps réel, dans une réserve dans laquelle vous pouvez puiser à tout moment.
Sablier c'est exactement ce qu'il fait, distribuer un token en temps réel d'une adresse à une autre adresse.
Cette solution facilite les paiements récurrents, comme des salaires, des subventions ou des récompenses.

#### **⭐⭐ Pour les initiés**

Sablier utilise des smart contracts pour créer des flux de paiements continus, où les tokens sont débloqués progressivement au fil du temps, à la seconde près.
Sablier comprend deux protocoles principaux :
- Lockup : pour des flux, dont le montant total est bloqué dès l'origine pour une certaine durée. La fréquence des paiements peut etre très variée. 
Cas d'usage : le vesting et les airdrops
- Flow : pour des flux, sans dépôt initiale ni de durée à l'origine.
Cas d'usage : salaires, subventions, etc.

Le flux de paiement est alloué continuement à un NFT, dont le bénéficiaire des paiements est propriétaire. Le montant accumulé dans le NFT, peut être retiré à tout moment.
Ce NFT peut être transférable, voire servir de garantie.


#### **⭐⭐⭐ Pour les experts**

Sablier repose sur des contrats intelligents déployés sur plusieurs blockchains, utilisant des protocoles de distribution de jetons pour gérer les flux de paiements/distribution de tokens. Les flux sont créés en verrouillant des tokens dans un contrat intelligent, qui libère ensuite les fonds de manière continue selon un taux prédéfini. 
Fonctionnalités clés sont : Représentation des flux par des NFT ERC-721; Création de flux en masse; Compatibilité avec les multisig comme Safe; Système sans permission et décentralisé.
Sablier propose une [interface web](https://app.sablier.com/) pour interagir facilement avec les protocoles.
Les développeurs peuvent intégrer Sablier dans leurs applications via des API et des modèles d'intégration disponibles dans la [documentation officielle](https://docs.sablier.com/). Les flux de paiements peuvent être surveillés et analysés à l'aide de sous-graphes et d'indexeurs off-chain, offrant une visibilité complète sur les transactions.
Exemple de flux de paiements : ![flux_sablier.png](/imag-en/flux_sablier.png)

## Pourquoi la DAO pourrait utiliser Sablier.com

#### **⭐ Pour les novices**

La DAO pourrait utiliser Sablier pour gérer les paiements de manière plus efficace et transparente. Par exemple, les partenaires de la DAO pourraient recevoir leurs récompenses ou salaires en temps réel, ce qui simplifie la gestion des finances et assure une transparence totale.
La DAO pourrait aussi distribuer des récompenses, dont la disponibilitée serait progressive.

#### **⭐⭐ Pour les initiés**

L'utilisation de Sablier par la DAO permettrait de :

1. **Optimiser la gestion des trésoreries** : En utilisant des flux de paiements continus, la DAO peut mieux gérer ses liquidités et éviter les paiements en gros montants qui peuvent perturber la trésorerie.
2. **Améliorer la transparence** : Tous les paiements sont enregistrés sur la blockchain, ce qui permet à tous les membres de la DAO de voir comment les fonds sont distribués.
3. **Faciliter les paiements récurrents** : Les paiements de salaires, de subventions ou de récompenses peuvent être automatisés, réduisant ainsi la charge administrative.

Le premier cas d'usage de la DAO est la distribution des REG du budget dédié à l'équipe RealT. Un vote sur Tally déclenchera le transfert du budget de la Trésorerie vers le sablier,qui sera initialisé au profit de RealT avec la fréquence de paiement qui aura été votée.

#### **⭐⭐⭐ Pour les experts**

Pour la DAO, l'intégration de Sablier permettrait une gestion granulaire des flux de trésorerie grâce à des contrats intelligents personnalisables. Les développeurs peuvent tirer parti des modèles d'intégration et des contrats de verrouillage pour créer des solutions sur mesure adaptées aux besoins spécifiques de la DAO. De plus, l'utilisation de sous-graphes permet une analyse approfondie des flux de paiements, facilitant la prise de décision basée sur des données en temps réel. Les risques associés à l'utilisation de Sablier sont minimisés grâce à des audits de sécurité réguliers et à une communauté active de développeurs et d'utilisateurs.


## Conclusion

L'intégration de Sablier.com dans l'écosystème de la DAO RealToken pourrait apporter des avantages significatifs en termes de gestion financière et de transparence. En adoptant cette technologie, la DAO pourrait non seulement améliorer son efficacité opérationnelle, mais aussi renforcer la confiance de ses membres grâce à une gestion transparente et automatisée des paiements.
L'utilisation de Sablier est entièrement gratuite : outre les coûts de gaz de la blockchain, il n'y a aucun frais.

## Ressources

- [Site officiel](https://sablier.com/)
- [Documentation officielle de Sablier](https://docs.sablier.com/)
- [API de Sablier](https://docs.sablier.com/api)
- [Documentation des smart contracts](https://docs.sablier.com/guides/lockup/deployments)
- [Blog de Sablier pour les dernières mises à jour](https://blog.sablier.com/)
- [Github de Sablier](https://github.com/sablier-labs)
---