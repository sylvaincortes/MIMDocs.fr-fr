---
title: "Étape 2 du déploiement PAM : Contrôleur de domaine PRIV | Microsoft Identity Manager"
description: "Préparez le contrôleur de domaine PRIV, qui fournit l’environnement bastion où Privileged Access Management est isolé."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 048a17c6b8150501185b7a13c3d2cb292791c9e8


---

# Étape 2 : Préparer le contrôleur de domaine PRIV

>[!div class="step-by-step"]
[« Étape 1](step-1-prepare-corp-domain.md)
[Étape 3 »](step-3-prepare-pam-server.md)

Lors de cette étape, vous allez créer un domaine qui fournit l’environnement bastion pour l’authentification d’administrateur.  Cette forêt a besoin d’au moins un contrôleur de domaine et au moins un serveur membre. Le serveur membre sera configuré à l’étape suivante.

## Créer un contrôleur de domaine Privileged Access Management

Dans cette section, vous allez configurer une machine virtuelle comme contrôleur de domaine d’une nouvelle forêt.

### Installer Windows Server 2012 R2
Sur une autre nouvelle machine virtuelle sans logiciel, installez Windows Server 2012 R2 pour créer un ordinateur « PRIVDC ».

1. Sélectionnez cette option pour effectuer une installation personnalisée (et non une mise à niveau) de Windows Server. Durant l’installation, spécifiez **Windows Server 2012 R2 Standard (serveur avec une interface graphique utilisateur) x64**. _Ne sélectionnez pas_ **Data Center ou Server Core**.

2. Lisez et acceptez les termes du contrat de licence.

3. Comme le disque est vide, sélectionnez **Personnalisé : installer uniquement Windows** et utilisez l’espace disque non initialisé.

4. Après avoir installé la version du système d’exploitation, connectez-vous à ce nouvel ordinateur comme nouvel administrateur. Utilisez le Panneau de configuration pour nommer l’ordinateur *PRIVDC*, attribuez-lui une adresse IP statique sur le réseau virtuel, puis configurez le serveur DNS pour l’affecter au contrôleur de domaine installé à l’étape précédente. Cette opération nécessite un redémarrage du serveur.

5. Une fois que le serveur a redémarré, connectez-vous en tant qu’administrateur. À l’aide du Panneau de configuration, configurez l’ordinateur pour vérifier les mises à jour, puis installez les mises à jour nécessaires. Cette opération peut nécessiter un redémarrage du serveur.

### Ajouter des rôles
Ajoutez les rôles Services de domaine Active Directory (AD DS) et Serveur DNS.

1. Lancez PowerShell en tant qu’administrateur.

2. Tapez les commandes suivantes pour préparer une installation de Windows Server Active Directory.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### Configurer les paramètres du Registre pour la migration de l’historique des SID

Lancez PowerShell, puis tapez la commande suivante pour configurer le domaine source afin d’autoriser l’accès par appel de procédure distante (RPC) à la base de données du Gestionnaire de comptes de sécurité (SAM).

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## Créer une forêt Privileged Access Management

Ensuite, promouvez le serveur en tant que contrôleur de domaine dans une nouvelle forêt.

Dans ce document, le nom priv.contoso.local est utilisé comme nom de domaine de la nouvelle forêt.  Le nom de la forêt n’est pas un élément crucial et ne doit pas être nécessairement subordonné à un nom de forêt existant de l’organisation. Toutefois, le nom de domaine et le nom NetBIOS de la nouvelle forêt doivent être uniques et distincts de ceux de n’importe quel autre domaine de l’organisation.  

### Créer un domaine et une forêt

1. Dans une fenêtre PowerShell, tapez les commandes suivantes pour créer le domaine.  Cette opération crée également une délégation DNS dans un domaine supérieur (contoso.local) qui a été créé lors d’une étape précédente.  Si vous envisagez de configurer le système DNS plus tard, omettez les paramètres `CreateDNSDelegation -DNSDelegationCredential $ca`.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Quand la fenêtre contextuelle s’affiche, fournissez les informations d’identification de l’administrateur de la forêt CORP (par exemple, le nom d’utilisateur CONTOSO\\Administrateur et le mot de passe correspondant de l’étape 1).

3. La fenêtre PowerShell vous invite à fournir un mot de passe d’administrateur en mode sans échec. Entrez un nouveau mot de passe deux fois. Des messages d’avertissement relatifs aux paramètres de chiffrement et de délégation DNS vont s’afficher. C’est normal.

Une fois la forêt créée, le serveur redémarrera automatiquement.

### Créer des comptes d’utilisateur et de service
Créez les comptes d’utilisateur et de service pour la configuration du Service et portail MIM. Ces comptes iront dans le conteneur Utilisateurs du domaine priv.contoso.local.

1. Une fois le serveur redémarré, connectez-vous à PRIVDC comme administrateur de domaine (PRIV\\Administrateur).

2. Lancez PowerShell et tapez les commandes suivantes. Le mot de passe « Pass@word1 » est juste un exemple. Vous devez utiliser un autre mot de passe pour les comptes.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### Configurez les droits d’audit et d’ouverture de session.

Vous devez configurer l’audit pour établir la configuration PAM entre les forêts.  

1. Vérifiez que vous êtes connecté comme administrateur de domaine (PRIV\\Administrateur).

2. Accédez à **Démarrer** > **Outils d’administration** > **Gestion des stratégies de groupe**.

3. Accédez à : **Forêt : priv.contoso.local** > **Domaines** > **priv.contoso.local** > **Contrôleurs de domaine** > **Stratégie des contrôleurs de domaine par défaut**. Un message d’avertissement apparaît.

4. Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut** et sélectionnez **Modifier**.

5. Dans l’arborescence de la console de l’Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur** > **Stratégies** > **Paramètres Windows** > **Paramètres de sécurité** > **Stratégies locales** > **Stratégie d’audit**.

6. Dans le volet d’informations, cliquez avec le bouton droit sur **Auditer la gestion des comptes**, puis sélectionnez **Propriétés**. Cliquez sur **Définir ces paramètres de stratégie**, cochez la case **Réussite** , la case **Échec** , cliquez sur **Appliquer** puis sur **OK**.

7. Dans le volet d’informations, cliquez avec le bouton droit sur **Auditer l’accès au service d’annuaire**, puis sélectionnez **Propriétés**. Cliquez sur **Définir ces paramètres de stratégie**, cochez la case **Réussite** , la case **Échec** , cliquez sur **Appliquer** puis sur **OK**.

8. Accédez à **Configuration ordinateur** > **Stratégies** > **Paramètres Windows** > **Paramètres de sécurité** > **Stratégies de comptes** > **Stratégie Kerberos**.

9. Dans le volet d’informations, cliquez avec le bouton droit sur **Durée de vie maximale du ticket d’utilisateur**, puis sélectionnez **Propriétés**. Cliquez sur **Définir ces paramètres de stratégie**, définissez le nombre d’heures à *1*, cliquez sur **Appliquer** puis sur **OK**. Notez que d’autres paramètres de la fenêtre changent également.

10. Dans la fenêtre Gestion des stratégies de groupe, sélectionnez **Stratégie de domaine par défaut**, cliquez avec le bouton droit, puis sélectionnez **Modifier**.

11. Développez **Configuration ordinateur** > **Stratégies** > **Paramètres Windows** > **Paramètres de sécurité** > **Stratégies locales**, puis sélectionnez **Attribution des droits utilisateur**.

12. Dans le volet d’informations, cliquez avec le bouton droit sur **Interdire l’ouverture de session en tant que tâche**, puis sélectionnez **Propriétés**.

13. Cochez la case **Définir ces paramètres de stratégie**, cliquez sur **Ajouter un utilisateur ou un groupe** puis, dans le champ Noms d’utilisateurs et de groupes, tapez *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* et cliquez sur **OK**.

14. Cliquez sur **OK** pour fermer la fenêtre.

15. Dans le volet d’informations, cliquez avec le bouton droit sur **Interdire l’ouverture de session par les services Bureau à distance**, puis sélectionnez **Propriétés**.

16. Cochez la case **Définir ces paramètres de stratégie**, cliquez sur **Ajouter un utilisateur ou un groupe** puis, dans le champ Noms d’utilisateurs et de groupes, tapez *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* et cliquez sur **OK**.

17. Cliquez sur **OK** pour fermer la fenêtre.

18. Fermez les fenêtres Éditeur de gestion de stratégie de groupe et Gestion des stratégies de groupe.

19. Lancez une fenêtre PowerShell en tant qu'administrateur, puis tapez la commande suivante pour mettre à jour le contrôleur de domaine à partir des paramètres de stratégie de groupe.

  ```
  gpupdate /force /target:computer
  ```

  Au bout d'une minute, la commande se termine et affiche le message « La mise à jour de la stratégie d'ordinateur s'est terminée sans erreur ».


### Configurer la redirection de noms DNS sur PRIVDC

À l’aide de PowerShell sur PRIVDC, configurez le transfert de noms DNS pour que le domaine PRIV reconnaisse les autres forêts existantes.

1. Lancez PowerShell.

2. Pour chaque domaine en haut de chaque forêt existante, tapez la commande suivante en spécifiant le domaine DNS existant (par exemple, contoso.local) et l’adresse IP du serveur maître de ce domaine.  

  Si vous avez créé un domaine contoso.local à l’étape précédente, spécifiez *10.1.1.31* comme adresse IP de réseau virtuel de l’ordinateur CORPDC.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> Les autres forêts doivent aussi pouvoir router les requêtes DNS pour la forêt PRIV vers ce contrôleur de domaine.  Si vous avez plusieurs forêts Active Directory, vous devez également ajouter un redirecteur DNS conditionnel à chacune de ces forêts.

### Configurer Kerberos

1. À l’aide de PowerShell, ajoutez des noms de principaux du service pour que SharePoint, l’API REST PAM et le Service MIM puissent utiliser l’authentification Kerberos.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> Les étapes suivantes de ce document décrivent comment installer les composants de serveur MIM 2016 sur un seul ordinateur. Si vous envisagez d’ajouter un autre serveur pour la haute disponibilité, une configuration Kerberos supplémentaire est nécessaire, comme décrit dans [FIM 2010 : Kerberos Authentication Setup (Configuration de l’authentification Kerberos)](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### Configurer la délégation pour donner accès aux comptes de service MIM

Procédez comme suit sur PRIVDC comme administrateur de domaine.

1. Lancez **Utilisateurs et ordinateurs Active Directory**.  
2. Cliquez avec le bouton droit sur le domaine **priv.contoso.local**, puis sélectionnez **Déléguer le contrôle**.  
3. Sous l’onglet Utilisateurs et groupes sélectionnés, cliquez sur **Ajouter**.  
4. Dans la fenêtre Sélectionner les utilisateurs, les ordinateurs ou les groupes, tapez *mimcomponent; mimmonitor; mimservice*, puis cliquez sur **Vérifier les noms**. Une fois les noms soulignés, cliquez sur **OK**, puis sur **Suivant**.  
5. Dans la liste des tâches courantes, sélectionnez **Créer, supprimer et gérer les comptes d’utilisateurs** et **Modifier l’appartenance à un groupe**, puis cliquez sur **Suivant** et sur **Terminer**.

6. Recliquez avec le bouton droit sur le domaine **priv.contoso.local**, puis sélectionnez **Déléguer le contrôle**.  
7. Sous l’onglet Utilisateurs et groupes sélectionnés, cliquez sur **Ajouter**.  
8. Dans la fenêtre Sélectionner les utilisateurs, les ordinateurs ou les groupes, entrez *MIMAdmin*, puis cliquez sur **Vérifier les noms**. Une fois les noms soulignés, cliquez sur **OK**, puis sur **Suivant**.  
9. Sélectionnez **tâche personnalisée**, appliquez à **Ce dossier**avec des **Autorisations générales**.    
10. Dans la liste des autorisations, sélectionnez ce qui suit :  
  - **Lire**  
  - **Écrire**  
  - **Créer tous les objets enfants**  
  - **Supprimer tous les objets enfants**  
  - **Lire toutes les propriétés**  
  - **Écrire toutes les propriétés**  
  - **Migrer l’historique SID**  
  Cliquez sur **Suivant** , puis sur **Terminer**.

11. Recliquez avec le bouton droit sur le domaine **priv.contoso.local**, puis sélectionnez **Déléguer le contrôle**.  
12. Sous l’onglet Utilisateurs et groupes sélectionnés, cliquez sur **Ajouter**.  
13. Dans la fenêtre Sélectionner les utilisateurs, les ordinateurs ou les groupes, entrez *MIMAdmin*, puis cliquez sur **Vérifier les noms**. Une fois les noms soulignés, cliquez sur **OK**, puis sur **Suivant**.  
14. Sélectionnez **tâche personnalisée**, appliquez à **Ce dossier**, puis cliquez sur **uniquement les objets utilisateur**.    
15. Dans la liste des autorisations, sélectionnez **Modifier le mot de passe** et **Réinitialiser le mot de passe**. Cliquez ensuite sur **Suivant** , puis sur **Terminer**.  
16. Fermez Utilisateurs et ordinateurs Active Directory.

17. Ouvrez une invite de commandes.  
18. Passez en revue la liste de contrôle d’accès sur l’objet Admin SD Holder dans les domaines PRIV. Par exemple, si votre domaine était « priv.contoso.local », tapez la commande  
  ```  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"  
  ```  
19. Mettez à jour la liste de contrôle d’accès pour vous assurer que le service MIM et le service de composant MIM peuvent mettre à jour les appartenances des groupes protégés par cette liste ACL.  Entrez la commande suivante :  
  ```  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"  
  ```  
20. Redémarrez le serveur PRIVDC pour que ces changements soient pris en compte.

## Préparer une station de travail PRIV

Si vous n’avez pas encore de station de travail jointe au domaine PRIV pour effectuer la maintenance des ressources PRIV (par exemple, MIM), suivez ces instructions pour préparer une station de travail.  

### Installer Windows 8.1 ou Windows 10 Entreprise

Sur une autre nouvelle machine virtuelle sans logiciel, installez Windows 8.1 Entreprise ou Windows 10 Entreprise pour créer un ordinateur nommé *« PRIVWKSTN »*.

1. Utilisez les paramètres Express pendant l'installation.

2. Notez que l'installation risque de ne pas pouvoir se connecter à Internet. Cliquez sur **Créer un compte local**. Spécifiez un nom d'utilisateur différent. N'utilisez pas « Administrateur » ou « Jen ».

3. À l’aide du Panneau de configuration, attribuez à cet ordinateur une adresse IP statique sur le réseau virtuel et définissez le serveur PRIVDC comme serveur DNS préféré de l’interface.

4. À l’aide du Panneau de configuration, joignez l’ordinateur PRIVWKSTN au domaine priv.contoso.local. Vous devrez fournir les informations d’identification de l’administrateur du domaine PRIV. Une fois cette opération terminée, redémarrez l’ordinateur PRIVWKSTN.

Si vous souhaitez plus d’informations, consultez [Securing privileged access workstations](https://technet.microsoft.com/en-us/library/mt634654.aspx) (Sécurisation des stations de travail à accès privilégié).

Lors de l’étape suivante, vous allez préparer un serveur PAM.

>[!div class="step-by-step"]
[« Étape 1](step-1-prepare-corp-domain.md)
[Étape 3 »](step-3-prepare-pam-server.md)



<!--HONumber=Jul16_HO3-->


