---
# required metadata

title: Guide de planification des capacités | Microsoft Identity Manager
description: Utilisez ce guide pour comprendre les variables à prendre en compte avant de déployer MIM 2016, y compris les niveaux de charge et les décisions de stratégie.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Guide de planification des capacités

Microsoft Identity Manager (MIM) vous permet de créer, de mettre à jour et de supprimer des comptes d’utilisateur dans votre organisation. Il permet également aux utilisateurs finaux de gérer les fonctionnalités libre-service de leurs propres comptes. Même dans un environnement de petite taille, toutes ces actions peuvent s’ajouter rapidement.

Avant de commencer à utiliser MIM, lisez ce guide et tirez parti des environnements de test pour identifier la portée appropriée pour votre déploiement. Cet article explique les nombreux facteurs courants que vous devez prendre en considération. Toutefois, chaque déploiement étant unique, tester vos scénarios dans un laboratoire constitue toujours le meilleur moyen de déterminer les serveurs, le matériel ou les topologies les plus adaptés à vos besoins.

Si vous ne connaissez pas encore MIM 2016 et ses composants, nous vous conseillons d’obtenir plus de détails sur [Microsoft Identity Manager 2016](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016) avant de poursuivre.

## Vue d'ensemble
Un certain nombre de variables peuvent affecter la capacité globale et les performances générales de votre déploiement Microsoft Identity Manager. La façon dont vous déployez physiquement les composants MIM (la topologie) et le matériel sur lequel ces composants sont exécutés sont des facteurs importants qui déterminent les performances et la capacité que vous pouvez attendre de votre déploiement MIM. Le nombre et la complexité des objets de configuration de la stratégie MIM peuvent être moins évidents, mais ils sont tout autant des facteurs significatifs à prendre en compte lors de la planification des capacités. Enfin, l’échelle attendue du déploiement, ainsi que la charge attendue, sont généralement des facteurs plus évidents qui affectent les performances et la capacité.

Les principaux facteurs qui affectent la capacité et les performances d’un déploiement de MIM 2016 sont décrits dans le tableau qui suit.

| Facteur de conception | Considérations |
| ------------- | -------------- |
| Topologie | La distribution des services MIM sur les ordinateurs du réseau. |
| Matériel | Le matériel physique et les spécifications matérielles virtualisées pour chaque composant MIM. Ceci comprend le processeur, la mémoire, la carte réseau et la configuration du disque dur. |
| Objets de configuration de la stratégie MIM | Le nombre et le type d’objets de configuration de la stratégie MIM, dont les jeux, les règles de stratégie de gestion (MPR) et les flux de travail. |
| Mettre à l'échelle | Le nombre d’utilisateurs, de groupes, de groupes calculés et de types d’objets personnalisés, comme les ordinateurs qui seront gérés par MIM 2016. Prenez également en compte la complexité des groupes dynamiques et veillez à tenir compte de l’imbrication des groupes. |
| Chargement | Fréquence d’utilisation. Par exemple, la fréquence prévue d’opérations telles que la création de groupes ou d’utilisateurs, la réinitialisation de mots de passe ou la consultation du portail dans un laps de temps donné. Notez que cette charge peut varier au cours sur une heure, un jour, une semaine ou une année. En fonction du composant, vous pouvez prévoir des pics de charge ou une charge moyenne. |


## Hébergement des composants de Microsoft Identity Manager

Il n’est pas obligatoire que les composants de Microsoft Identity Manager se trouvent sur le même ordinateur. Réfléchir à ces composants et aux ordinateurs physiques ou machines virtuelles qui les hébergent constitue une partie importante de la planification de capacité.

Des facteurs matériels peuvent affecter les performances de l’environnement MIM. Exemple :
- Quelle est la configuration de disque physique pour l’ordinateur qui exécute la base de données SQL de service MIM 2016 ? Le nombre de plateaux qui composent le disque ou la distribution des fichiers journaux et des fichiers de données risque d’affecter considérablement les performances du système.

Vous devez aussi tenir compte des facteurs externes dans votre configuration. Exemple :
- Si vous utilisez un stockage réseau SAN en tant que configuration de base de données du service MIM 2016, quelles sont les autres applications qui partagent ce SAN ? Ces applications peuvent affecter les performances de la base de données si elles sont en concurrence pour les ressources disque partagées sur le SAN.


## Utilisateurs et groupes
Le nombre d’utilisateurs et de groupes dans votre environnement est élément à prendre en compte lorsque vous réfléchissez à la taille du déploiement. Toutefois, il existe plusieurs autres considérations liées dont vous devez également tenir compte dans la planification.

- Les utilisateurs peuvent-ils créer des groupes ? Dans ce cas, vous devez envisager d’évaluer si la manière dont les utilisateurs créent des groupes affecte la croissance des groupes dans votre environnement.

- Des groupes dynamiques seront-ils déployés ? Déterminez le nombre et les types de groupes dynamiques prévus dans votre environnement.


## Niveaux de charge attendus
Vous devez également envisager le type de charge qui repose sur les composants MIM. Pour obtenir une estimation, examinez les applications actuelles dans votre environnement. Certains points importants à prendre en considération sont les suivants :

- À quelle fréquence vous attendez-vous à recevoir les demandes pour rejoindre ou quitter un groupe ?

- À quelle fréquence envisagez-vous que les utilisateurs créent des groupes statiques ou dynamiques ?

- Combien d’opérations non pilotées par les utilisateurs attendez-vous, par exemple la synchronisation des modifications à partir de systèmes externes ? Assurez-vous de tenir compte de la charge générée par la synchronisation des informations d’identité avec les systèmes externes.

- Quels sont les scénarios que vous envisagez de déployer ? Pour différents scénarios, vous obtiendrez différents modèles de charge. Par exemple, les ordinateurs clients dotés du client MIM 2016 valident régulièrement si l’inscription est nécessaire à l’ouverture de la session, ce qui augmente la charge de votre système.

- Prévoyez-vous de grandes variations dans les niveaux de charge, entre le niveau normal et les pics de charge ? Par exemple, il y a généralement de nombreuses réinitialisations de mot de passe après une période de fêtes. Assurez-vous que vous prévoyez vos activités de synchronisation et de maintenance du système en dehors des pics d’utilisation attendus. Lorsque vous envisagez la planification des capacités, vérifiez que vous tenez bien compte des périodes de pointe.


## Objets de configuration de la stratégie

Parmi les objets de stratégie de configuration Microsoft Identity Manager figurent les MPR, les jeux, les flux de travail et les règles de synchronisation d’un déploiement. Les déploiements MIM sont uniques à chaque client, car la configuration de stratégie change en fonction des besoins de chaque déploiement. Les considérations clés des performances liées aux objets de configuration de la stratégie MIM sont les suivantes :

- **Jeux** Chaque opération dans le système doit être évaluée par rapport aux appartenances et aux mises à jour qui entraînent des modifications dans l’appartenance au jeu. Par exemple, une modification simple comme le remplacement du numéro du bâtiment par le bureau de la personne peut avoir un impact énorme. Toutefois, d’autres modifications peuvent avoir un impact en cascade, par exemple le changement de gestionnaire, qui peut affecter plusieurs objets à plusieurs niveaux.

- Les **règles de stratégie de gestion** (MPR) gèrent les règles de contrôle d’accès et déclenchent des flux de travail. Lors de la création des MPR, vous devrez peut-être augmenter le nombre de jeux pour pouvoir capturer les différents états de transition des objets. Ces jeux supplémentaires peuvent déclencher des flux de travail supplémentaires, chaque mappage de flux de travail correspondant à des requêtes uniques dans le système. C’est alors un autre élément à prendre en compte dans la planification de la capacité.

La configuration de stratégie MIM comprend également des décisions sur l’approvisionnement dans votre environnement. N’oubliez pas de réfléchir aux points suivants :

- Allez-vous approvisionner les principes de sécurité externes sur plusieurs forêts de services de domaine Active Directory ? Ceci génère encore plus de flux de travail et de demandes, ce qui entraîne une charge supplémentaire pour le système.

- Utiliserez-vous l’approvisionnement ? Si c’est le cas, cela affecte le nombre d’entrées de règles attendues, ainsi que le nombre de demandes et de flux de travail associés dans le système.


## Voir aussi
- [Éléments de topologie à prendre en compte pour le déploiement MIM](topology-considerations.md)
- La version téléchargeable du [Guide de planification des capacités de Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) explique plus en détail les tests et les résultats des tests de performances.


<!--HONumber=May16_HO3-->


