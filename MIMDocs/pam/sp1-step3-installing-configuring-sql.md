---
title: "Étape 3 : Configurer SQL"
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
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# Étape 3 : Configurer SQL

Avant d’exécuter les étapes ci-dessous, vérifiez que vous utilisez SQL Server 2012 SP1 ou version ultérieure, ou SQL Server 2014. Pour les serveurs joints à un domaine, connectez-vous en utilisant le compte MIMAdmin, sinon connectez-vous en tant qu’administrateur local
1. Exécutez PowerShell en tant qu’administrateur
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Sélectionnez l’option de menu 3 (Configurer SQL Server)

  Si le serveur n’est pas encore joint au domaine PRIV, vous êtes invité à saisir vos informations d’identification et à joindre le serveur au domaine.
  Après la jonction de domaine, l’ordinateur redémarre. Une fois le redémarrage réussi, connectez-vous au serveur avec le compte MIMAdmin.

1. Exécutez PowerShell en tant qu’administrateur
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Sélectionnez l’option de menu 3 (Configurer SQL Server)

Lorsque vous y êtes invité, entrez le mot de passe pour le compte de service MIMAdmin et laissez l’installation se poursuivre. Une fois cette procédure terminée, passez à l’étape 4.



<!--HONumber=Sep16_HO4-->


