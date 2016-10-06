---
title: "Étape 6 : Configurer l’approbation PAM"
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
ms.openlocfilehash: 46afda513e849e457f5f3644a46f244161467e50


---

# Configurer l’approbation PAM

**Ceci n’est pas requis pour un environnement PRIV uniquement** Connectez-vous à PAMServer avec le compte MIMAdmin.

1. Connectez-vous à PAMServer avec le compte MIMAdmin
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Sélectionnez l’option de Menu 6 (Configurer l’approbation PAM)

  Lorsque vous y êtes invité, entrez les informations d’identification du compte d’administrateur CORP. Après avoir entré les informations d’identification, l’approbation est établie et la configuration est terminée.



<!--HONumber=Sep16_HO4-->


