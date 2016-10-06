---
title: "Étape 2 : Configurer le domaine CORP"
description: "Préparez le domaine CORP avec des identités nouvelles ou existantes à gérer avec Privileged Identity Manager à l’aide de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 7919fa3fdbb6613fbfa636ddb34762faa85925b9


---

# Étape 2 : Configurer le domaine CORP

Une fois que le fichier SIDs.txt est copié vers CORPDC **non requis pour les déploiements PRIVOnly**

1. Connectez-vous à CORPDC en tant qu’administrateur
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Sélectionnez l’option de menu 2 (Configurer la forêt CORP)



<!--HONumber=Sep16_HO4-->


