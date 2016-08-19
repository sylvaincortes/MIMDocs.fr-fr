---
title: "Étape 1 du déploiement PAM : Domaine CORP | Microsoft Identity Manager"
description: "Préparez le domaine CORP avec des identités nouvelles ou existantes à gérer avec Privileged Identity Manager."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 9a2fafa86c5c928339ff8d7ad1593472046ccb98


---

# Étape 1 : Préparer l’ordinateur hôte et le domaine CORP

>[!div class="step-by-step"]
[Étape 2 »](step-2-prepare-priv-domain-controller.md)


Lors de cette étape, vous allez vous préparer à héberger l’environnement bastion. Si nécessaire, vous allez aussi créer un contrôleur de domaine et une station de travail membre dans un nouveau domaine et une nouvelle forêt (la forêt *CORP*) avec des identités à gérer par l’environnement bastion. Cette forêt CORP simule une forêt existante qui contient des ressources à gérer. Ce document comprend un exemple de ressource à protéger, un partage de fichiers.

Si vous avez déjà un domaine Active Directory (AD) existant avec un contrôleur de domaine exécutant Windows Server 2012 R2 ou version ultérieure, pour lequel vous êtes administrateur de domaine, vous pouvez utiliser ce domaine à la place.  

## Préparer le contrôleur de domaine CORP

Cette section décrit comment configurer un contrôleur de domaine pour un domaine CORP. Dans le domaine CORP, les utilisateurs administratifs sont gérés par l’environnement bastion. Le nom du système DNS (Domain Name System) du domaine CORP utilisé dans cet exemple est *contoso.local*.

### Installer Windows Server

Installez Windows Server 2012 R2 ou Windows Server 2016 Technical Preview 4 ou ultérieur sur une machine virtuelle pour créer un ordinateur nommé *CORPDC*.

1. Choisissez **Windows Server 2012 R2 Standard (serveur avec une interface graphique utilisateur) x64** ou **Windows Server 2016 Technical Preview (serveur avec expérience utilisateur)**.

2. Lisez et acceptez les termes du contrat de licence.

3. Comme le disque est vide, sélectionnez **Personnalisé : installer uniquement Windows** et utilisez l’espace disque non initialisé.

4. Connectez-vous à ce nouvel ordinateur comme administrateur. Accédez au Panneau de configuration. Nommez l’ordinateur *CORPDC* et attribuez-lui une adresse IP statique sur le réseau virtuel. Redémarrez le serveur.

5. Une fois que le serveur a redémarré, connectez-vous comme administrateur. Accédez au Panneau de configuration. Configurez l’ordinateur pour rechercher les mises à jour, puis installez les mises à jour nécessaires. Redémarrez le serveur.

### Ajouter des rôles pour établir un contrôleur de domaine

Dans cette section, vous allez ajouter les rôles Services de domaine Active Directory (AD DS), Serveur DNS et Serveur de fichiers (partie de la section Services de fichiers et de stockage), puis promouvoir ce serveur en contrôleur de domaine d’une nouvelle forêt nommée contoso.local.

> [!NOTE]  
> Si vous avez déjà un domaine à utiliser comme domaine CORP et que ce domaine utilise Windows Server 2012 R2 ou ultérieur comme contrôleur de domaine, vous pouvez passer directement à [Créer des utilisateurs et des groupes supplémentaires à des fins de démonstration](#create-additional-users-and-groups-for-demonstration-purposes).

1. Toujours en étant connecté comme administrateur, lancez PowerShell.

2. Tapez les commandes suivantes.

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  Le système vous invitera à fournir un mot de passe d'administrateur en mode sans échec. Notez que des messages d'avertissement relatifs aux paramètres de chiffrement et de délégation DNS s'afficheront. C'est normal.

3. Une fois la forêt créée, déconnectez-vous. Le serveur redémarre automatiquement.

4. Une fois le serveur redémarré, connectez-vous à CORPDC comme administrateur du domaine. Il s’agit généralement de l’utilisateur CONTOSO\\Administrateur, dont le mot de passe est celui créé quand vous avez installé Windows sur CORPDC.

### Créer un groupe

Créez un groupe à des fins d’audit par Active Directory, si le groupe n’existe pas. Le nom du groupe doit être identique au nom de domaine NetBIOS, suivi de trois signes dollar, par exemple *CONTOSO$$$*.

Pour chaque domaine, connectez-vous à un contrôleur de domaine comme administrateur de domaine et procédez comme suit :

1. Lancez PowerShell.

2. Tapez les commandes suivantes, mais remplacez « CONTOSO » par le nom NetBIOS de votre domaine.

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

Dans certains cas, le groupe existe déjà. C’est normal si le domaine était également utilisé dans des scénarios de migration Active Directory.

### Créer des utilisateurs et des groupes supplémentaires à des fins de démonstration

Si vous avez créé un domaine CORP, vous devez créer des utilisateurs et groupes supplémentaires pour illustrer le scénario PAM. Les utilisateurs et les groupes de démonstration ne doivent ni être administrateurs de domaine, ni être contrôlés par les paramètres adminSDHolder dans Active Directory.

> [!NOTE]
> Si vous avez déjà un domaine que vous allez utiliser comme domaine CORP et que ce domaine a un utilisateur et un groupe que vous pouvez utiliser à des fins de démonstration, passez à la section [Configurer l’audit](#configure-auditing).

Nous allons créer un groupe de sécurité nommé *CorpAdmins* et un utilisateur nommé *Jen*. Vous pouvez utiliser des noms différents si vous le souhaitez.

1. Lancez PowerShell.

2. Tapez les commandes suivantes. Remplacez le mot de passe « Pass@word1 » par une chaîne de mot de passe différente.

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### Configurer l’audit

Vous devez activer l’audit dans les forêts existantes pour établir la configuration PAM sur ces forêts.  

Pour chaque domaine, connectez-vous à un contrôleur de domaine comme administrateur de domaine et procédez comme suit :

1. Accédez à **Démarrer** > **Outils d’administration** (ou, sur Windows Server 2016, **Outils d’administration Windows**), puis lancez **Gestion des stratégies de groupe**.

2. Accédez à la stratégie des contrôleurs de domaine pour ce domaine.  Si vous avez créé un domaine pour contoso.local, accédez à **Forêt : contoso.local** > **Domaines** > **contoso.local** > **Contrôleurs de domaine** > **Stratégie des contrôleurs de domaine par défaut**. Un message d’information s’affiche.

3. Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut** et sélectionnez **Modifier**. Une nouvelle fenêtre s’affiche.

4. Dans la fenêtre Éditeur de gestion des stratégies de groupe, sous l’arborescence Stratégie des contrôleurs de domaine par défaut, accédez à **Configuration de l’ordinateur** > **Stratégies** > **Paramètres Windows** > **Paramètres de sécurité** > **Stratégies locales** > **Stratégie d’audit**.

5. Dans le volet d’informations, cliquez avec le bouton droit sur **Auditer la gestion des comptes**, puis sélectionnez **Propriétés**. Sélectionnez **Définir ces paramètres de stratégie**, cochez la case **Réussite**, cochez la case **Échec**, puis cliquez sur **Appliquer** et sur **OK**.

6. Dans le volet d’informations, cliquez avec le bouton droit sur **Auditer l’accès au service d’annuaire**, puis sélectionnez **Propriétés**. Sélectionnez **Définir ces paramètres de stratégie**, cochez la case **Réussite**, cochez la case **Échec**, puis cliquez sur **Appliquer** et sur **OK**.

7. Fermez les fenêtres Éditeur de gestion de stratégie de groupe et Gestion des stratégies de groupe.

8. Appliquez les paramètres d’audit en lançant une fenêtre PowerShell et en tapant ce qui suit :

  ```
  gpupdate /force /target:computer
  ```

Le message **La mise à jour de la stratégie d’ordinateur s’est terminée sans erreur** doit s’afficher après quelques minutes.

### Configurer les paramètres du Registre

Dans cette section, vous allez configurer les paramètres de Registre nécessaires pour la migration de l’historique des SID, qui seront utilisés pour la création des groupes de gestion Privileged Access Management.

1. Lancez PowerShell.

2. Tapez les commandes suivantes pour configurer le domaine source de sorte à autoriser l’accès par appel de procédure distante (RPC) à la base de données du Gestionnaire de comptes de sécurité (SAM).

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

Le contrôleur de domaine CORPDC redémarre. Pour plus d’informations sur ce paramètre de Registre, consultez [Comment faire pour résoudre les problèmes de migration de sIDHistory inter-forêts avec ADMTv2](http://support.microsoft.com/kb/322970).

## Préparer une station de travail CORP et une ressource

Si vous n’avez encore aucune station de travail jointe au domaine, suivez ces instructions pour en préparer une.  

> [!NOTE]
> Si vous avez déjà une station de travail jointe au domaine, passez à [Créer une ressource à des fins de démonstration](#create-a-resource-for-demonstration-purposes).

### Installer Windows 8.1 ou Windows 10 Entreprise comme machine virtuelle

Sur une autre nouvelle machine virtuelle sans logiciel, installez Windows 8.1 Entreprise ou Windows 10 Entreprise pour créer un ordinateur nommé *CORPWKSTN*.

1. Utilisez les paramètres Express pendant l'installation.

2. Notez que l'installation risque de ne pas pouvoir se connecter à Internet. Sélectionnez **Créer un compte local**. Spécifiez un nom d'utilisateur différent. N'utilisez pas « Administrateur » ou « Jen ».

3. À l’aide du Panneau de configuration, attribuez à cet ordinateur une adresse IP statique sur le réseau virtuel et définissez le serveur CORPDC comme serveur DNS préféré de l’interface.

4. À l’aide du Panneau de configuration, joignez l’ordinateur CORPWKSTN au domaine contoso.local. Vous devrez fournir les informations d’identification d’administrateur du domaine Contoso. Une fois cette opération terminée, redémarrez l’ordinateur CORPWKSTN.

### Créer une ressource à des fins de démonstration

Vous aurez besoin d’une ressource pour illustrer le contrôle d’accès en fonction du groupe de sécurité avec PAM.  Si vous n’avez pas encore de ressource, vous pouvez utiliser un dossier de fichiers à des fins de démonstration.  Cette ressource utilisera les objets AD « Jen » et « CorpAdmins » que vous avez créés dans le domaine contoso.local.

1. Connectez-vous à la station de travail CORPWKSTN. Cliquez sur l’icône **Changer d’utilisateur**, puis sur **Autre utilisateur**. Vérifiez que l’utilisateur CONTOSO\\Jen peut se connecter à CORPWKSTN.

2. Créez un dossier nommé *CorpFS* et partagez-le avec le groupe *CorpAdmins*.

3. Ouvrez PowerShell comme administrateur.

4. Tapez les commandes suivantes.

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

Lors de l’étape suivante, vous allez préparer le contrôleur de domaine PRIV.

>[!div class="step-by-step"]
[Étape 2 »](step-2-prepare-priv-domain-controller.md)



<!--HONumber=Jul16_HO3-->


