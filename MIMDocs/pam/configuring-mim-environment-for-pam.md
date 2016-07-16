---
title: "Configuration de l’environnement MIM pour Privileged Access Management | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# Configuration de l'environnement MIM pour Privileged Access Management
Vous devez effectuer sept étapes lors de la configuration de l'environnement pour l'accès entre forêts, de l'installation et de la configuration d'Active Directory et Microsoft Identity Manager et de la démonstration d'une demande d'accès juste-à-temps.

Ces étapes sont présentées pour que vous puissiez partir de zéro et créer un environnement de test. Si vous appliquez PAM dans un environnement existant, vous pouvez utiliser vos propres comptes d’utilisateur ou contrôleurs de domaine plutôt qu’en créer de nouveaux pour correspondre aux exemples.

1.  Préparez le serveur *CORPDC* comme contrôleur de domaine et *CORPWKSTN* comme station de travail membre.

2.  Préparez le serveur *PRIVDC* comme contrôleur de domaine.

3.  Préparez le serveur *PAMSRV* dans la forêt *PRIV* .

4.  Installez les composants MIM sur *PAMSRV* et les applets de commande sur une station de travail membre de la forêt *CONTOSO* et préparez-les pour la gestion de l'accès privilégié.

5.  Établissez l'approbation entre les forêts *PRIV* et *CONTOSO* .

6.  Préparation des groupes de sécurité privilégiés avec l'accès aux ressources protégées et aux comptes membres pour la gestion de l'accès privilégié juste-à-temps.

7.  Démontrer la demande, la réception et l'utilisation de l'accès élevé privilégié à une ressource protégée.

>[!div class="step-by-step"]
[[!div class="step-by-step"] [Étape »](step-1-prepare-corp-domain.md)](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO2-->


