---
title: "Étape 5 : Installer et configurer PAM"
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
ms.openlocfilehash: a5d86991f1579f292d7d303148422cef746d008a


---
# Étape 5 : Installer et configurer PAM

Pour un serveur PAMServer joint à un domaine, connectez-vous en tant que MIMAdmin ou en tant qu’administrateur local.
1. Exécutez PowerShell en tant qu’administrateur
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Sélectionnez l’option de menu 5 (Configurer MIM PAM)

>[!NOTE] Si l’ordinateur n’est pas déjà joint au domaine PRIV, vous serez invité à saisir des informations d’identification. Après la jonction de domaine, l’ordinateur redémarre.

Une fois que le serveur PAMServer a redémarré, reconnectez-vous à l’ordinateur avec le compte MIMAdmin.

1. Exécutez PowerShell en tant qu’administrateur
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Sélectionnez l’option de menu 5 (Configurer MIM PAM)

  Lorsque vous y êtes invité, entrez le mot de passe, le compte d’analyse MIM, le compte de composant MIM, le compte d’agent de gestion MIM, le compte de service MIM, le compte d’administrateur MIM et le compte SharePoint.
  Une fois l’installation terminée, l’ordinateur redémarre.



<!--HONumber=Sep16_HO4-->


