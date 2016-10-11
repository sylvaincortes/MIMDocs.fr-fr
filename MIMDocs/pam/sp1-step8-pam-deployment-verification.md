---
title: "Étape 8 : Vérifier le déploiement PAM"
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
ms.openlocfilehash: 743ba586374ccc04e9ddafff759a00574e13f6ac


---

# Étape 8 : Vérifier le déploiement PAM

Le package de déploiement est accompagné de scripts de vérification qui peuvent exécuter un scénario PAM pour valider que le déploiement PAM fonctionne comme prévu.
Pour utiliser la vérification du déploiement, modifiez la section PAMDeploymentConfig.xml appelée <PamValidation/>.

>[!Note] La validation nécessite qu’un domaine d’ordinateur client soit joint au domaine CORP avec les composants côté client PAM installés. Consultez l’Addenda pour obtenir des scripts d’installation d’un client.

Le nom de l’ordinateur client doit être mis à jour dans la balise <PAMValidationClient/> du fichier PAMDeploymentConfig.xml. Le reste des données dans le nœud <PAMValidation/> doit être modifié uniquement en cas de conflit avec les utilisateurs/groupes existants, car cette validation va tenter de les créer.
Utilisez les étapes suivantes pour effectuer la validation :

Étape 1 :

1. Connectez-vous à CORPDC en tant qu’un administrateur de domaine CORP
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Ceci permet de créer les groupes et les utilisateurs nécessaires à la validation

Étape 2 :

1. Connectez-vous au serveur PAM en tant que MIMAdmin
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Cette étape migre les utilisateurs et les groupes vers l’environnement PAM

Étape 3 :

1. Connectez-vous au client CORP en tant qu'administrateur local
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Cette étape vous demande de fournir les informations d’identification CORPAdmin. Une fois ces informations fournies, les utilisateurs requis sont ajoutés aux groupes « Utilisateurs du Bureau à distance » et « Gestion à distance des utilisateurs ».
Sur le client CORP, utilisez la commande suivante pour ouvrir PowerShell en tant que l’utilisateur PRIV que vous validez. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Dans la fenêtre PowerShell, tapez :

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Ceci affiche l’état de la demande.
  Initialement, l’utilisateur n’aura pas accès à la ressource. Une fois que l’utilisateur est ajouté de manière Juste-à-temps au rôle, l’accès lui est autorisé. Une fois que la durée de la demande expire, l’utilisateur n’aura de nouveau plus accès.
  Le script utilise la valeur par défaut (11 minutes) avant l’expiration de la demande.



<!--HONumber=Sep16_HO4-->


