---
title: "Étape 4 : Configurer SharePoint"
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
ms.openlocfilehash: aed3370e8dbc458c97c60686957f337cfd451d33


---

# Étape 4 : Configurer SharePoint

SharePoint doit être SharePoint Foundation 2013 avec SP1.

Pour les serveurs joints au domaine, connectez-vous en tant que MIMAdmin

1. Exécutez PowerShell en tant qu’administrateur
2.  .\PAMDeployment.ps1
3.  Sélectionnez l’option de menu 4 (Configurer SharePoint)


Pour les serveurs de groupe de travail

1. Exécutez PowerShell en tant qu’administrateur
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Sélectionnez l’option de menu 4 (Configurer SharePoint)

L’ordinateur redémarre plusieurs fois durant l’installation de SharePoint. À chaque fois, réexécutez la configuration de SharePoint, en veillant à vous connecter avec le compte MIMAdmin.
Si l’ordinateur installant SharePoint n’a pas de connectivité à Internet pour télécharger les composants requis, ceux-ci peuvent être téléchargés indépendamment et placés dans un dossier local. **Le chemin d’accès à ce dossier local doit être mis à jour dans le fichier PAMConfiguration.xml dans <PrerequisitesBinaryLocation/>.** Consultez l’Addenda 5 pour obtenir les liens de téléchargement des fichiers.
Après l’installation, l’interface utilisateur graphique de configuration de SharePoint s’ouvre. Suivez les étapes pour terminer l’installation de SharePoint. Sélectionnez Serveur complet et passez en revue le reste de l’interface utilisateur. Après l’installation, vous êtes invité à exécuter l’Assistant Configuration. Exécutez les étapes ci-dessous.

1. Sous l’onglet **Se connecter à une batterie de serveurs**, choisissez de **créer une batterie de serveurs.**
2. Spécifiez **SQLServer** comme serveur de base de données pour la base de données de configuration et **SharePoint ServiceAccount** comme compte d’accès à la base de données à utiliser par SharePoint.
3. Spécifiez un mot de passe de sécurité pour la batterie de serveurs **(il ne sera pas utilisé ultérieurement)**
4. Acceptez le reste des paramètres par défaut de l’Assistant Configuration de SharePoint pour créer une batterie à un seul serveur

Vous trouverez plus d’informations dans la section **Configurer SharePoint** à [l’étape 3 : préparer un serveur PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server) Une fois l’opération terminée, exécutez de nouveau le script « .\PAMDeployment.ps1 », en sélectionnant l’option 4 (Configurer SharePoint) pour effectuer cette étape.



<!--HONumber=Sep16_HO4-->


