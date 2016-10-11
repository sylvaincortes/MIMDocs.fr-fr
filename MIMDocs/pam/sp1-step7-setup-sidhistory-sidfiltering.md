---
title: "Étape 7 : Configurer l’historique SID/le filtrage SID"
description: "Préparez le domaine CORP avec des identités nouvelles ou existantes à gérer avec Privileged Identity Manager à l’aide de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 4c5cfa92f3111a6d298f586ba547a1eca2502853


---

# Configurer l’historique SID/le filtrage SID

**Ceci n’est pas requis pour un environnement PRIV uniquement** Connectez-vous à PAMServer avec le compte MIMAdmin.

1. Connectez-vous à CORPDC en tant qu’administrateur
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Sélectionnez l’option de menu 8 (Configurer l’historique SID/le filtrage SID)

Après une exécution réussie, les messages suivants s’affichent :<br/></br>
Pour le filtrage SID : <br/></br>
« Configuration de l’approbation pour ne pas filtrer les SID » ou « Filtrage SID non activé pour cette approbation ». </br></br>
Pour l’historique SID : </br></br>
« Activation de l’historique des SID pour cette approbation » ou « L’historique des SID est déjà activé pour cette approbation ».



<!--HONumber=Sep16_HO4-->


