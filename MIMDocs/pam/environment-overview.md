---
title: "Vue d’ensemble de l’environnement | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: a01cb2e1df52f3157b3d84a4eab837cececfbe1b


---

# Vue d'ensemble de l'environnement

PAM fonctionne avec des machines virtuelles avec des lecteurs distincts connectés les uns aux autres sur un réseau partagé. Ces machines virtuelles peuvent être hébergées par Windows 8.1, Windows Server 2012 R2 ou d’autres plateformes de système d’exploitation.

![Serveurs PAM : relations et plateformes prises en charge - diagramme](media/pam-test-lab-architecture.png)

Vous devez avoir au minimum trois machines virtuelles.  Si vous n’avez pas encore de domaine Active Directory pour PAM à gérer, il vous faudra une machine virtuelle supplémentaire comme contrôleur de domaine CORP.  Si vous souhaitez configurer le logiciel PRIV pour la haute disponibilité, deux machines virtuelles supplémentaires sont nécessaires.

Les lecteurs où seront stockées les images de disques de machines virtuelles doivent avoir un minimum de 120 Go d’espace disque libre pour contenir toutes les machines virtuelles.  Si vous prévoyez d’effectuer un déploiement pour la haute disponibilité, vérifiez que le sous-système de disque répond aux exigences du stockage partagé SQL.  Le stockage partagé peut être constitué de disques de cluster Clustering de basculement Windows Server, de disques sur un réseau de zone de stockage (SAN) ou de partages de fichiers sur un serveur SMB. Notez qu’il doivent être dédiés à l’environnement bastion. Le partage du stockage avec d’autres charges de travail en dehors de l’environnement bastion n’est pas recommandé, car cela peut mettre en danger l’intégrité de l’environnement bastion.

> [!NOTE]
> La version CTP (Customer Technical Preview) actuelle de MIM n’est pas compatible avec le contenu de base de données ou d’annuaire de la version CTP précédente. Si vous avez précédemment évalué des scénarios MIM pour PAM ou d'autres scénarios, sauvegardez et archivez les ordinateurs virtuels utilisés pour ce test, puis commencez le déploiement avec de nouvelles images d'ordinateur virtuel qui n'ont pas déjà été utilisées pour des scénarios MIM.



<!--HONumber=Jul16_HO2-->


