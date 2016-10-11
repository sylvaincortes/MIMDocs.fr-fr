---
title: "Étape 1 : Configurer le domaine Priv"
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
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 37ac600701ed9d90790e557d2f282be45eed43b4


---
# Étape 1 : Configurer le domaine Priv

1. Connectez-vous à PRIVDC en tant qu’administrateur
  * S’il s’agit d’un environnement PRIV uniquement, connectez-vous à CORPDC
2. Exécutez PowerShell en tant qu’administrateur
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Sélectionnez l’option de menu 1 (Configurer la forêt PRIV)


Les comptes de service nécessaires pour la gestion de SQL/SharePoint et MIM sont automatiquement créés s’ils n’existent pas dans le domaine. Vous serez invité à entrer les mots de passe pour la création de ces comptes de service pendant l’exécution du script.
Si le domaine PRIV est Windows Server 2016, avec le niveau fonctionnel défini sur Windows Server 2016 Technical Preview 5, le script vous invite à activer la fonctionnalité facultative de gestion des accès privilégiés d’Active Directory requise par PAM. Choisissez Oui pour continuer.
Pour les niveaux fonctionnels antérieurs à Windows Server 2016, ignorez l’avertissement indiquant qu’aucune configuration supplémentaire ne sera effectuée. Vous devrez réexécuter PAMDeployment.ps1 et la configuration de la forêt PAM si l’administrateur passe au niveau fonctionnel Windows Server 2016.

>[!Note] Les étapes suivantes ne sont PAS requises pour les configurations PRIVOnly

Copiez le fichier SIDs.txt généré dans $env:SYSTEMDRIVE\PAM vers le dossier similaire sur CORPDC. Cette opération est requise par CORPDC pour autoriser les utilisateurs PRIV à lire les propriétés des utilisateurs d’entreprise.
Une fois le script terminé, vous êtes invité à redémarrer l’ordinateur pour que les modifications prennent effet.



<!--HONumber=Sep16_HO4-->


