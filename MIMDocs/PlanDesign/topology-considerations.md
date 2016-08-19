---
title: "Guide de topologie du déploiement | Microsoft Identity Manager"
description: "Comprendre les composants MIM 2016 et obtenir des suggestions de déploiement dans votre environnement."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 8efb513bfe7dfb67d240a17b39535270f0c7fab6


---


# Éléments de topologie à prendre en compte :
Vous pouvez déployer les composants Microsoft Identity Manager (MIM) sur le même serveur ou sur plusieurs serveurs dans plusieurs configurations. La topologie de déploiement que vous choisissez affecte les performances que vous pouvez obtenir de MIM. Cet article présente plusieurs topologies de déploiement qu’il est possible d’implémenter.

## Composants MIM
Lorsque vous concevez votre topologie de déploiement, il est important de savoir ce que fait chaque composant et comment ces composants interagissent.

- **Portail MIM** : interface pour la réinitialisation des mots de passe, la gestion des groupes et les opérations administratives.
    -
- **Service MIM** : service web qui met en œuvre les fonctionnalités de gestion d’identité de MIM 2016.
- **Service de synchronisation MIM** : synchronise les données avec les autres systèmes d’identité.
- **Microsoft SQL Server** : le service MIM et le service de synchronisation MIM stockent leurs données dans des bases de données SQL.

Le tableau suivant présente les options d’hébergement de chacun des composants MIM. Ils peuvent être hébergés sur le même ordinateur ou bien être distribués sur plusieurs serveurs et clusters.

| | Portail MIM | Service MIM | Service de synchronisation MIM | SQL Server |
| --- | --- | --- | --- | --- |
| Même ordinateur | Oui | Oui | Oui | Oui |
| Serveur distinct | Oui | Oui | Oui | Oui |
| Cluster d’équilibrage de la charge réseau | Oui | Oui | | |
| Cluster serveur | | | | Oui |


## Topologie à plusieurs niveaux
La topologie à plusieurs niveaux est la topologie la plus couramment utilisée. Elle offre la plus grande flexibilité. Le portail MIM, le service MIM et les bases de données sont séparés sur plusieurs niveaux et déployés sur plusieurs ordinateurs. Cette topologie offre davantage de souplesse dans la mise à l’échelle des différents composants MIM. Par exemple, vous pouvez faire évoluer le portail MIM horizontalement en ajoutant des serveurs au cluster d’équilibrage de charge réseau. De même, vous pouvez faire évoluer le service MIM à l’aide d’un cluster d’équilibrage de charge réseau et en augmentant le nombre d’ordinateurs (nœuds) dans le cluster en fonction des besoins.

Dans la topologie à plusieurs niveaux, un ordinateur est dédié à l’hébergement de chaque base de données SQL (une pour le service MIM et une autre pour le service de synchronisation MIM). L’évolutivité des performances des ordinateurs qui hébergent les bases de données SQL peut être améliorée avec l’ajout ou la mise à niveau de matériel. Par exemple, il peut s’agir de remplacer les processeurs, d’en ajouter de nouveaux, d’augmenter la mémoire vive ou de la mettre à niveau ou bien encore de mettre à niveau la configuration des disques durs pour optimiser l’accès en lecture et en écriture et réduire la latence.

![Diagramme de topologie MIM à plusieurs niveaux](media/MIM-topo-multitier.png)

Dans cette configuration, le service de synchronisation MIM et sa base de données sont hébergés sur le même ordinateur. Toutefois, il se peut que vous soyez être en mesure d’atteindre des performances similaires s’il existe une connexion réseau à haut débit dédiée entre le service de synchronisation MIM et sa base de données s’ils sont hébergés sur des ordinateurs distincts.


## Topologie à plusieurs niveaux avec plusieurs services MIM
La synchronisation des données avec des systèmes externes peut prendre beaucoup de temps et ajouter une charge de travail considérable au système pendant cette période. Si les résultats de la configuration de la synchronisation provoquent le déclenchement de stratégies avec flux de travail, ces stratégies sont en concurrence pour les ressources des flux de travail de l’utilisateur final. Ces problèmes peuvent être résolus par avec des flux de travail d’authentification, comme la réinitialisation de mot de passe, qui sont effectués en temps réel avec un utilisateur final attendant le résultat du processus. En fournissant une seule instance du service MIM pour les opérations des utilisateurs finaux et un portail distinct pour la synchronisation des données d’administration, vous pouvez assurer une meilleure réactivité des opérations pour les utilisateurs finaux.

![Diagramme de topologie MIM multiple à plusieurs niveaux](media/MIM-topo-multitier-multiservice.png)

Comme avec la topologie standard à plusieurs niveaux, vous pouvez améliorer les performances du portail MIM à l’aide d’un cluster d’équilibrage de charge réseau ou en augmentant le nombre de nœuds du cluster, en fonction des besoins.

Les ordinateurs SQL Server qui hébergent le service de synchronisation MIM et la base de données du service MIM influencent considérablement les performances globales de votre déploiement MIM. Par conséquent, suivez les recommandations mentionnées dans la documentation de SQL Server afin d’optimiser les performances de vos bases de données. Pour plus d’informations, consultez les documents suivants :

## Voir aussi
- La version téléchargeable du [Guide de planification des capacités de Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) explique plus en détail les tests et les résultats des tests de performances.



<!--HONumber=Jul16_HO3-->


