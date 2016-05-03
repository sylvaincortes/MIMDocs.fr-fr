---
# required metadata

title: Création de rapports hybrides Identity Manager dans Azure | Microsoft Identity Manager
description: La création de rapports hybrides d’Azure Active Directory vous permet de créer des rapports personnalisés qui incluent les événements cloud et les événements sur site.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Création de rapports hybrides Identity Manager dans Azure
Si vous disposez d'un abonnement Azure, vous pouvez désormais facilement créer un rapport des événements locaux et dans le cloud. Les rapports peuvent ensuite être consultés sur le portail Azure. Mieux encore, les rapports sont combinés aux activités relatives à Azure Active Directory. Grâce à la création de rapports hybrides Identity Manager, le Portail de gestion Azure AD peut afficher des rapports d'activité sur la gestion des identités pour les activités locales et dans le cloud. Cette fonctionnalité de création de rapports offre les avantages suivants :

-   Unification de l'expérience : rapports unifiés pour les activités IAM, aussi bien localement que dans le cloud.

-   Réduction des coûts : disparition de la nécessité de disposer d’une infrastructure d’entrepôt de données locale pour la création de rapports

-   Propriété de vos données : les données de création de rapports peuvent être facilement exportées à partir de l’instance locale d’Identity Manager ou d’Azure AD, et peuvent servir à générer des rapports basés sur un affichage personnalisé

## Qu'est-ce que la fonctionnalité de création de rapports hybrides Azure AD ?
Grâce à la création de rapports hybrides, le Portail de gestion Azure AD peut afficher des rapports d'activité unifiés en matière de gestion des identités. Peu importe l'emplacement d'origine de l'activité, Identity Manager ou Azure AD. Par exemple, si vous souhaitez savoir qui s’est inscrit pour la réinitialisation du mot de passe libre-service au cours du mois dernier, vous pouvez tout voir sur le Portail de gestion Azure AD. Dans ce rapport, vous verrez quels sont les utilisateurs qui se sont inscrits pour la réinitialisation du mot de passe libre-service dans le [volet d’accès aux applications](https://myapps.microsoft.com) et dans Identity Manager.

![Image des activités de réinitialisation du mot de passe](media/MIM-Hybrid-passwordreset.jpg)

## Mode d’utilisation
La création de rapports hybrides permet aux professionnels de l'informatique de faire face à certains besoins courants en matière de gestion des identités.

1.  Créer des rapports sur les activités de gestion des identités ayant eu lieu sur différents systèmes : vous pouvez désormais consulter les rapports de gestion des identités relatifs aux activités sur Azure AD et Identity Manager, à partir du Portail de gestion Azure AD.

2.  Exporter les données de création de rapports, et créer des rapports personnalisés : Outre les rapports Azure AD, à travers cette nouvelle fonctionnalité, nous avons ajouté des événements Windows qui reflètent l'activité relative à Identity Manager. Cela facilite énormément l'intégration aux systèmes SIEM, l'affichage de l'activité relative à Identity Manager et la création de rapports personnalisés.

3.  Réduire les coûts d'infrastructure du système de création de rapports : le déploiement de cette nouvelle fonctionnalité ne vous prendra que quelques minutes. Il vous suffit d'installer un agent de création de rapports sur le serveur Identity Manager.

Vous pouvez télécharger l'agent de création de rapports à partir du Portail de gestion Azure AD, dans l'écran de configuration d'annuaire :

![Image du téléchargement de l’agent de création de rapports MIM](media/MIM-Hybrid-downloadReportAgent.jpg)

## Comment fonctionne-t-il ?
Après l'installation de l'agent de création de rapports, les données d'activité Identity Manager sont envoyées au journal des événements Windows. L'agent de création de rapports traite les événements et les charge vers Azure. Dans Azure, les données d'activité sont stockées pendant un mois. Durant la récupération du rapport, les événements d'activité sont analysés et filtrés pour les rapports demandés. Enfin, le Portail de gestion Azure récupère les données de création de rapports, et les affiche dans le cadre du rapport d'activité.

![Diagramme de la création de rapports hybrides](media/MIM-Hybrid-howitworks.png)


<!--HONumber=Apr16_HO2-->


