---
title: "Modèle de niveaux pour le partitionnement des privilèges d’administration | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 509c05bbda5f0a0b936518fb023000771c45d4f7


---

# Modèle de niveaux pour le partitionnement des privilèges d’administration

Dans l’environnement de menace d’aujourd’hui, la question n’est pas de savoir si un agresseur parviendra à accéder à vos systèmes, mais quand. Cela signifie que la sécurité interne est tout aussi importante que la robustesse de la défense de périmètre. Cet article décrit un modèle de sécurité destiné à protéger contre l’élévation de privilèges en isolant les activités à privilèges élevés des zones à haut risque. Ce modèle fournit une expérience utilisateur plaisante tout en respectant les bonnes pratiques et les principes de sécurité.

## Élévation de privilèges dans des forêts Active Directory

Les comptes d’utilisateurs, de services ou d’applications qui ont des privilèges d’administration permanents pour les forêts Windows Server Active Directory (AD) représentent des risques importants pour la mission et l’activité de l’organisation. Ces comptes sont souvent ciblés par les attaquants car, s’ils peuvent être compromis, l’attaquant a le privilège de se connecter à d’autres serveurs ou applications du domaine.

Le modèle de niveaux crée des divisions entre les administrateurs en fonction des ressources qu’ils gèrent. Les administrateurs qui contrôlent les stations de travail des utilisateurs sont séparés de ceux qui contrôlent les applications ou qui gèrent les identités d’entreprise. Pour en savoir plus sur ce modèle, consultez la [documentation de référence sur la sécurisation de l’accès privilégié](http://aka.ms/tiermodel).

## Limitation de l’exposition des informations d’identification avec des restrictions d’ouverture de session

La réduction des risques de vol d’informations d’identification pour les comptes d’administration nécessite généralement de repenser les pratiques d’administration pour limiter l’exposition aux attaquants. Dans un premier temps, il est conseillé aux organisations de :

- Limiter le nombre d’hôtes sur lesquels les informations d’identification d’administration sont exposées.
- Limiter les privilèges des rôles au minimum nécessaire.
- Assurez-vous que les tâches d’administration ne sont pas effectuées sur des hôtes utilisés pour les activités des utilisateurs standard (par exemple l’e-mail et la navigation sur le web).

L’étape suivante consiste à implémenter des restrictions d’ouverture de session, et à mettre en œuvre des processus et des pratiques pour respecter les spécifications requises du modèle des niveaux. Dans l’idéal, l’exposition des informations d’identification doit également être réduite au privilège minimal requis pour le rôle dans chaque niveau.

Des restrictions d’ouverture de session doivent être appliquées pour garantir que les comptes à privilèges élevés n’ont pas accès aux ressources moins sécurisées. Exemple :

- les administrateurs de domaines (niveau 0) ne peuvent pas ouvrir des sessions sur des serveurs d’entreprise (niveau 1) et sur des stations de travail d’utilisateurs standard (niveau 2) ;
- les administrateurs de serveurs (niveau 1) ne peuvent pas ouvrir des sessions sur des stations de travail d’utilisateurs standard (niveau 2).

>[!NOTE] 
> Les administrateurs de serveurs ne doivent pas être dans le groupe d’administrateurs du domaine. Le personnel ayant des responsabilités à la fois pour la gestion des contrôleurs de domaine et pour les serveurs d’entreprise doivent disposer de comptes distincts.

Les restrictions d’ouverture de session peuvent être appliquées avec :

- Des restrictions des droits d’ouverture de session de stratégie de groupe, notamment :  
    - Refuser l'accès à un ordinateur à partir du réseau  
    - Interdire l’ouverture de session en tant que tâche  
    - Interdire l’ouverture de session en tant que service  
    - Interdire l’ouverture d’une session locale  
    - Interdire l’ouverture de session par les services Bureau à distance  
- Stratégies et silos d’authentification, si vous utilisez Windows Server 2012 ou ultérieur
- Authentification sélective, si le compte est dans une forêt d’administration dédiée

L’article suivant, [Planification d’un environnement bastion](planning-bastion-environment.md), explique comment ajouter une forêt d’administration dédiée pour Microsoft Identity Manager pour établir les comptes d’administration.



<!--HONumber=Jun16_HO5-->


