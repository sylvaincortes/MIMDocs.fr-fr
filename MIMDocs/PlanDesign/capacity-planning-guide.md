---
# required metadata

title: Guide de planification des capacités | Microsoft Identity Manager
description: Utilisez ce guide pour comprendre les variables à prendre en compte avant de déployer MIM 2016, y compris les niveaux de charge et les décisions de stratégie.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
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

Ce guide a été conçu pour faciliter la planification côté client, mais il ne doit pas être utilisé seul pour déterminer les serveurs, le matériel ou les topologies appropriés qui sont nécessaires à un déploiement de Microsoft Identity Manager (MIM). Les organisations doivent configurer leurs propres environnements de test et sont encouragées à le faire pour estimer de façon plus précise les capacités et les performances. Microsoft ne garantit pas que toutes les organisations connaissent le même niveau de capacité ou les mêmes caractéristiques de performances, même si les composants MIM 2016 sont déployés et configurés de manière identique à ce qui est décrit dans ce guide.

Pour préparer correctement votre déploiement de MIM, simulez votre environnement de production prévu en laboratoire et testez-le. Vous pouvez décider de tester différentes topologies sur différents types de matériel, puis effectuer des tests de mise à l’échelle et de charge, ce qui peut vous aider à mieux évaluer ce qui peut se produire lorsque vous déployez MIM 2016 dans votre environnement.


## Vue d'ensemble
Un certain nombre de variables peuvent affecter la capacité globale et les performances générales de votre déploiement Microsoft Identity Manager. La façon dont vous déployez physiquement les composants MIM (la topologie) et le matériel sur lequel ces composants sont exécutés sont des facteurs importants qui déterminent les performances et la capacité que vous pouvez attendre de votre déploiement MIM. Le nombre et la complexité des objets de configuration de la stratégie MIM peuvent être moins évidents, mais ils sont tout autant des facteurs significatifs à prendre en compte lors de la planification des capacités. Enfin, l’échelle attendue du déploiement, ainsi que la charge attendue, sont généralement des facteurs plus évidents qui affectent les performances et la capacité.

Les principaux facteurs qui affectent la capacité et les performances d’un déploiement de MIM 2016 sont décrits dans le tableau qui suit.

| Facteur de conception | Considérations |
| ------------- | -------------- |
| Topologie | La distribution des services MIM sur les ordinateurs du réseau. Par exemple, le service de synchronisation MIM 2016 sera-t-il hébergé sur le même ordinateur que celui qui héberge sa base de données ? |
| Matériel | Le matériel physique et les spécifications matérielles virtualisées pour chaque composant MIM. Ceci comprend le processeur, la mémoire, la carte réseau et la configuration du disque dur. |
| Objets de configuration de la stratégie MIM | Le nombre et le type d’objets de configuration de la stratégie MIM, dont les jeux, les règles de stratégie de gestion (MPR) et les flux de travail. Par exemple, le nombre de flux de travail déclenchés. Combien y a-t-il de définitions de jeux et quelle est leur complexité relative ? |
| Mettre à l'échelle | Le nombre d’utilisateurs, de groupes, de groupes calculés et de types d’objets personnalisés, comme les ordinateurs qui seront gérés par MIM 2016. Prenez également en compte la complexité des groupes dynamiques et veillez à tenir compte de l’imbrication des groupes. |
| Chargement | Fréquence d’utilisation prévue. Par exemple, le nombre prévu de créations de nouveaux groupes ou de nouveaux utilisateurs, de réinitialisations de mots de passe ou de consultations simultanées du portail. Notez que cette charge peut varier au cours sur une heure, un jour, une semaine ou une année. En fonction du composant, vous devrez prévoir des pics de charge ou une charge moyenne.


## Hébergement des composants de Microsoft Identity Manager
Microsoft Identity Manager possède de nombreux composants. Dans de nombreux cas, ces composants ne se trouvent pas sur le même ordinateur. Lorsque vous envisagez MIM du point de vue de la planification des capacités, les composants et les ordinateurs physiques (et éventuellement les ordinateurs virtuels) sur lequel ils sont hébergés sont primordiaux. De nombreux facteurs potentiels peuvent affecter les performances de l’environnement MIM, par exemple la configuration du disque physique de l’ordinateur qui exécute la base de données SQL du service MIM 2016. Le nombre de plateaux qui composent le disque ou la distribution des fichiers journaux et des fichiers de données risque d’affecter considérablement les performances du système. En outre, tenez compte des facteurs externes dans votre configuration. Si vous utilisez un stockage réseau SAN en tant que configuration de base de données du service MIM 2016, quelles sont les autres applications qui partagent ce SAN ? Comment ces applications affectent-elles les performances de la base de données alors qu’elles sont en concurrence pour les ressources disque partagées sur le SAN ? Lorsque plusieurs applications utilisent les mêmes ressources disque, il est possible de constater des goulots d’étranglement et des irrégularités dans les performances disque.


## Utilisateurs et groupes
Le nombre d’utilisateurs et de groupes dans votre environnement est élément à prendre en compte lorsque vous réfléchissez à la taille du déploiement. Toutefois, il existe plusieurs autres considérations liées dont vous devez également tenir compte dans la planification. Les autres considérations sont les suivantes :

- Les utilisateurs peuvent-ils créer des groupes ? Dans ce cas, vous devez envisager d’évaluer si la manière dont les utilisateurs créent des groupes affecte la croissance des groupes dans votre environnement.

- Des groupes dynamiques seront-ils déployés ?
  - Quels types de groupes dynamiques seront déployés ?
  - Combien de groupes dynamiques sont susceptibles d’être déployés ?


## Niveaux de charge attendus
Vous devez également envisager le type de charge qui repose sur les composants MIM. Certains points importants à prendre en considération sont les suivants :

- À quelle fréquence vous attendez-vous à recevoir les demandes pour rejoindre ou quitter un groupe ?

- À quelle fréquence envisagez-vous que les utilisateurs créent des groupes statiques ou dynamiques ?

- Pouvez-vous obtenir ces informations à partir des applications existantes dans votre environnement ?

- Quelle charge attendez-vous des opérations non pilotées par les utilisateurs, par exemple la synchronisation des modifications à partir de systèmes externes ? Assurez-vous de tenir compte de la charge générée par les opérations de synchronisation des informations d’identité avec les systèmes externes.

- Quels sont les scénarios que vous envisagez de déployer ? Pour différents scénarios, vous obtiendrez différents modèles de charge. Par exemple, les ordinateurs clients dotés du client MIM 2016 valident régulièrement si l’inscription est nécessaire à l’ouverture de la session, ce qui augmente la charge de votre système.

- Prévoyez-vous de grandes variations dans les niveaux de charge, entre le niveau normal et les pics de charge ? Par exemple, attendez-vous un grand nombre de réinitialisations de mot de passe après les périodes de vacances ? Assurez-vous que vous prévoyez vos activités de synchronisation et de maintenance du système en dehors des pics d’utilisation attendus. Lorsque vous envisagez la planification des capacités, vérifiez que vous tenez bien compte des périodes de pointe.


## Objets de configuration de la stratégie

Les objets de configuration de stratégie Microsoft Identity Manager représentent la logique métier du déploiement de MIM. Il s’agit là d’un domaine dans lequel chaque implémentation MIM reste probablement unique, car la configuration de la stratégie est spécifique aux besoins de chaque déploiement. Parmi les objets de stratégie de configuration MIM, on compte les MPR, les jeux, les flux de travail et les règles de synchronisation d’un déploiement. Les considérations clés des performances liées aux objets de configuration de la stratégie MIM sont les suivantes :

- **Jeux** Chaque opération dans le système doit être évaluée par rapport aux appartenances et aux mises à jour qui entraînent des modifications dans l’appartenance au jeu. Par exemple, une modification simple comme le remplacement du numéro du bâtiment par le bureau de la personne peut avoir un impact énorme. Toutefois, d’autres modifications peuvent avoir un impact en cascade, par exemple le changement de gestionnaire, qui peut affecter plusieurs objets à plusieurs niveaux.

- Les **règles de stratégie de gestion** (MPR) sont utilisées pour contrôler les règles du contrôle d’accès et le déclenchement des flux de travail. Lorsque vous créez des MPR, il est possible qu’il soit nécessaire d’augmenter le nombre de jeux afin de pouvoir capturer les différents états de transition des objets. Ces jeux supplémentaires peuvent déclencher des flux de travail supplémentaires, chaque mappage de flux de travail correspondant à des requêtes uniques dans le système. C’est alors un autre élément à prendre en compte dans la planification de la capacité.

Lorsque vous travaillez sur les objets de stratégie de configuration MIM, vous devez également envisager les points suivants :

- Allez-vous approvisionner les principes de sécurité externes sur plusieurs forêts de services de domaine Active Directory ? Ceci génère encore plus de flux de travail et de demandes, ce qui entraîne une charge supplémentaire pour le système.

- Utiliserez-vous l’approvisionnement ? Si c’est le cas, cela affecte le nombre d’entrées de règles attendues, ainsi que le nombre de demandes et de flux de travail associés dans le système.


## Voir aussi
- La version téléchargeable du [Guide de planification des capacités de Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) explique plus en détail les tests et les résultats des tests de performances.


<!--HONumber=Apr16_HO2-->


