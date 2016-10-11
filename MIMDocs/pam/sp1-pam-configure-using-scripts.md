---
title: "Configurer PAM à l’aide de scripts"
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
ms.sourcegitcommit: 96c734ade75f5c206858387cf45106761bc0a881
ms.openlocfilehash: a1e4e5561bf8d38c56c3d27249d94f4bf7103b8c


---

# Configurer PAM à l’aide de scripts

Si vous choisissez d’installer SQL et SharePoint sur des serveurs distincts, ils doivent être configurés à l’aide des instructions ci-dessous. Si les composants SQL, SharePoint et PAM sont installés sur le même ordinateur, les étapes ci-dessous doivent être exécutées sur l’ordinateur en question.

Les étapes ci-dessous supposent qu’un domaine PRIV est déjà configuré. Pour obtenir des instructions sur la configuration d’un domaine PRIV, consultez l’addenda à la fin du document.

étapes :

1. Décompressez le fichier compressé « PAMDeploymentScripts.zip » dans le dossier %SYSTEMDRIVE%\PAM sur tous les ordinateurs.
2. Sur l’un des ordinateurs, ouvrez le fichier **PAMDeploymentConfig.xml** et mettez à jour les informations à l’aide du tableau ci-dessous ou des conseils fournis dans le fichier XML lui-même. Si les forêts CORP et PRIV sont déjà configurées, il vous suffit de mettre à jour **DNSName** et **NetbiosName.**
3. Dans la section Rôles, mettez à jour le **compte de service**, les **détails sur l’ordinateur** et **l’emplacement des fichiers binaires d’installation** pour les rôles SQL, SharePoint et MIM.
    1. L’emplacement du fichier binaire MIM doit pointer vers le répertoire contenant le dossier « Service et portail ». L’emplacement du fichier binaire client doit pointer vers le répertoire contenant « Add-ins and Extensions.msi ».

4. S’il s’agit d’un environnement PRIVOnly, la balise PRIVOnly doit être définie sur True.
    1. Pour les environnements PRIVOnly, mettez à jour **DNSName** et **NetbiosName** pour le domaine PRIV afin qu’ils correspondent au domaine CORP. Vérifiez que les suffixes sont corrects pour les ordinateurs où MIM, SharePoint et SQL seront installés, car le fichier de modèle par défaut suppose qu’il s’agit d’une configuration CORP et PRIV.
    2. Cliquez ici pour plus d’informations sur les environnements PRIVOnly.

5. Copiez le même fichier PAMDeploymentConfig.xml dans le dossier %SYSTEMDRIVE%\PAM sur tous les ordinateurs, CORPDC, PRIVDC, le serveur PAM, le serveur SQL Server et les serveurs SharePoint.


## Feuille de planification de déploiement

Avant de continuer, mettez à jour le fichier PAMDeploymentConfig.xml et placez la copie mise à jour sur tous les ordinateurs.

### Setup

|Machine   | À exécuter en tant que   |Commandes   |
|---|---|---|
|  PRIVDC |Administrateur de domaine PRIV   | .\PAMDeployment.ps1 Sélectionnez l’option de menu 1 (Configurer la forêt PRIV)   |
|   |   |  L’étape ci-dessus génère un fichier SIDs.txt. Vous devez copier ce fichier dans $envDrive:PAM de CORPDC avant de passer à l’étape suivante. |
| CORPDC  |Administrateur de domaine CORP   | .\PAMDeployment.ps1 Sélectionnez l’option de menu 2 (Configurer la forêt CORP)   |
| PAMServer (ou SQL Server)   |Administrateur de domaine CORP   |  .\PAMDeployment.ps1 Sélectionnez l’option de menu 2 (Configurer la forêt CORP)  |
|  PAMServer |  Administrateur local (administrateur MIM après la jonction de domaine) |  .\PAMDeployment.ps1 Sélectionnez l’option de menu 4 (Configurer SharePoint)  |
| PAMServer  | Administrateur local (administrateur MIM après la jonction de domaine)  | .\PAMDeployment.ps1 Sélectionnez l’option de menu 5 (Configurer MIM PAM)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Sélectionnez l’option de menu 6 (Configurer l’approbation PAM) .\PAMDeployment.ps1 Sélectionnez l’option de menu 6 (Configurer l’approbation PAM) |

### Validation

|  Machine | À exécuter en tant que   | Commandes   |
|---|---|---|
| CORPClient  | Utilisateur CORP (administrateur local)  |   .\PAMDeployment.ps1 Sélectionnez l’option de menu 7 (Configurer le client MIM PAM)  |
| CORPDC  | Administrateur de domaine CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Utilisateur CORP (administrateur local)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>Utilisateur \PRIV.pamRequestor et dans le cas de PRIVOnly : <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |



<!--HONumber=Sep16_HO4-->


