---
title: Addenda
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
ms.openlocfilehash: 482cfbbac3ea668ca4bf9d8a4a45469e61634f98


---
# Addenda :

## Addenda 1 : Configurer le domaine PRIV

Après avoir décompressé le fichier compressé dans le dossier $env:SYSTEMDRIVE\PAM, modifiez le fichier PAMDeploymentConfig.xml pour fournir les détails de la forêt PRIV. Mettez à jour le nom DNS, le nom NetBIOS, le nom du contrôleur de domaine, le chemin d’accès de la base de données/des journaux et le chemin d’accès Sysvol. Mettez également à jour le mode du domaine et de la forêt. Si vous testez Windows Server Technical Preview 5, définissez DomainMode et ForestMode sur WinThreshold.

1. Connectez-vous au contrôleur de domaine PRIV en tant qu’administrateur
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Sélectionnez l’option de menu 9 (Configurer la forêt PRIV)


Le contrôleur de domaine redémarre automatiquement une fois cette opération terminée. Le mot de passe administrateur du Mode restauration des services d’annuaire (DSRM) doit correspondre aux critères suivants :

  * La longueur minimale du mot de passe est de 15 caractères
  * Le mot de passe contient au moins un caractère en minuscule
  * Le mot de passe contient au moins un caractère en MAJUSCULE
  * Le mot de passe contient au moins un chiffre ou caractère spécial

## Addenda 2 : Configurer le domaine CORP

Si vous débutez avec PAM et souhaitez configurer un environnement de test, le script permet également la configuration d’un domaine CORP. Après avoir décompressé le fichier compressé dans le dossier $env:SYSTEMDRIVE\PAM, modifiez le fichier PAMDeploymentConfig.xml pour ajouter les détails de la forêt CORP. Mettez à jour le nom DNS, le nom NetBIOS, le nom du contrôleur de domaine, le chemin d’accès de la base de données/des journaux et le chemin d’accès Sysvol. Le niveau fonctionnel doit être au moins Windows Server 2012 R2.

1. Connectez-vous au contrôleur de domaine CORP en tant qu’administrateur
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Sélectionnez l’option de menu 10 (Configurer la forêt CORP)

Le contrôleur de domaine redémarre automatiquement une fois cette opération terminée

## Addenda 3 : Configurer un client CORP pour effectuer la validation

ClientBinaryLocation dans le fichier de configuration doit pointer vers l’emplacement où se trouve setup.exe.
Connectez-vous au client en tant qu’administrateur local et exécutez les commandes suivantes dans une fenêtre PowerShell avec élévation de privilèges :

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Sélectionnez l’option de menu 7 (Configurer le client MIM PAM)


Si l’ordinateur n’est pas joint à un domaine, vous êtes invité à fournir les informations d’identification de l’administrateur CORP pour effectuer la jonction de domaine. L’ordinateur doit être redémarré après la jonction de domaine. Reconnectez-vous au client en tant qu’administrateur local et exécutez les commandes suivantes dans une fenêtre PowerShell avec élévation de privilèges :

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Sélectionnez l’option de menu 7 (Configurer le client MIM PAM)

Passez à l’étape 8 indiquée ci-dessus.

## Addenda 4 : En cas de problème

Tous les journaux de script sont enregistrés sous %AppData%\MIMPAMInstall. Veuillez compresser le dossier dans un fichier Zip et l’envoyer, ainsi que les détails de l’opération et de l’erreur, par courrier électronique à [mim2016@microsoft.com](mim2016@microsoft.com).



<!--HONumber=Sep16_HO4-->


