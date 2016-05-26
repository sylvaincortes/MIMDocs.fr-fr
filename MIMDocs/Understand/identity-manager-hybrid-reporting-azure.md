---
# required metadata

title: Rapports de gestion des identités hybrides | Microsoft Identity Manager
description: La création de rapports hybrides d’Azure Active Directory vous permet de créer des rapports personnalisés qui incluent les événements cloud et les événements sur site.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
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

# Rapports de gestion des identités hybrides dans Azure
Avec Azure Active Directory (AD), vous pouvez créer un rapport pour surveiller l’activité de gestion des identités locale ou dans le cloud. Cette fonctionnalité vous permet de gérer toutes vos données d’identité et d’accès dans un emplacement unique, ce qui fait gagner du temps et réduit les coûts généraux.

## Qu’est-ce que la fonctionnalité de création de rapports hybrides Azure AD ?
La création de rapports hybrides permet aux professionnels de l’informatique de faire face aux besoins courants en matière de gestion des identités.

1. **Recueillez les activités de gestion des identités sur différents systèmes.** Les rapports hybrides montrent l’activité de gestion des identités d’Azure AD et Identity Manager.

2. **Exportez les données de création de rapports et créez des rapports personnalisés.** Outre l’affichage des rapports dans le portail Azure, vous pouvez également exporter les données pour générer vos propres affichages personnalisés.

3. **Réduisez les coûts d’infrastructure du système de création de rapports.** Grâce à la création de rapports hybrides dans le cloud, vous pouvez éliminer l’infrastructure locale d’entrepôt de données de création de rapports.

## Fonctionnement

Pour recueillir les données locales, vous installez d’abord un agent de création de rapports sur votre serveur Identity Manager. Vous téléchargez l’agent de création de rapports à partir de la page de configuration de votre annuaire dans le [portail Azure Classic](https://manage.windowsazure.com/).

Le processus de création de rapports hybrides se déroule comme suit :
1. Une fois l’agent de création de rapports installé, les données d’activité Identity Manager sont envoyées au journal des événements Windows.
2. L’agent de création de rapports traite les événements du journal des événements Windows et les charge vers Azure.
3. Les données d’activité sont stockées dans Azure pendant un mois.
4. Quand vous demandez un rapport, les événements d’activité sont analysés et filtrés en fonction des rapports demandés.
5. Le portail Azure Classic récupère les données de création de rapports et les affiche dans le cadre du rapport d’activité.

## Voir aussi
- Obtenir plus de détails sur l’[Utilisation des rapports hybrides Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)


<!--HONumber=May16_HO3-->


