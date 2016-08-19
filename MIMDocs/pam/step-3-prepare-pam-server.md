---
title: "Étape 3 du déploiement PAM : Serveur PAM | Microsoft Identity Manager"
description: "Préparez un serveur PAM qui hébergera SQL et SharePoint pour le déploiement Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 1a21399df9528f689b811400a660543853d88472


---

# Étape 3 - Préparer un serveur PAM

>[!div class="step-by-step"]
[« Étape 2](step-2-prepare-priv-domain-controller.md)
[Étape 4 »](step-4-install-mim-components-on-pam-server.md)

## Installer Windows Server 2012 R2
Sur une troisième machine virtuelle, installez Windows Server 2012 R2, plus précisément Windows Server 2012 R2 Standard (serveur avec interface utilisateur graphique) x64, pour créer *PAMSRV*. Dans la mesure où SQL Server et SharePoint 2013 seront installés sur cet ordinateur, 8 Go de RAM sont nécessaires au minimum.

1. Sélectionnez **Windows Server 2012 R2 Standard (serveur avec interface utilisateur graphique) x64**.

    ![Choisir Windows Server standard avec interface utilisateur graphique - capture d’écran](media/PAM_GS_Select_WS2012.png)

2. Lisez et acceptez les termes du contrat de licence.

3.  Comme le disque sera vide, sélectionnez **Personnalisé : installer uniquement Windows** et utilisez l’ **espace disque non initialisé**.

4.  Connectez-vous à ce nouvel ordinateur comme administrateur.  À l’aide du Panneau de configuration, attribuez-lui une adresse IP statique sur le réseau virtuel, configurez cette interface réseau pour envoyer les requêtes DNS à l’adresse IP de PRIVDC, puis nommez l’ordinateur *PAMSRV*.  Cette opération nécessite un redémarrage du serveur.

5.  Si le réseau virtuel ne fournit pas de connectivité Internet, ajoutez une interface réseau supplémentaire à l’ordinateur qui fournit une connexion à Internet.  Cette connexion sera nécessaire pour l’installation de SharePoint et vous pourrez la désactiver une fois cette étape terminée.

6.  Une fois que le serveur a redémarré, connectez-vous en tant qu’administrateur. À l’aide du Panneau de configuration, configurez l’ordinateur pour vérifier les mises à jour, puis installez les mises à jour nécessaires.  Cette opération peut nécessiter un redémarrage du serveur.

7.  Une fois le serveur redémarré, connectez-vous en tant qu’administrateur, ouvrez le Panneau de configuration et joignez PAMSRV au domaine PRIV (priv.contoso.local).  Vous devrez fournir le nom d’utilisateur et les informations d’identification d’un administrateur de domaine PRIV (PRIV\Administrateur). Après l'affichage du message de bienvenue, fermez la boîte de dialogue et redémarrez ce serveur.


### Ajouter les rôles de serveur web (IIS) et de serveur d’applications
Ajoutez les rôles Serveur web (IIS) et Serveur d'applications, les fonctionnalités .NET Framework 3.5, le module Active Directory pour Windows PowerShell, ainsi que les autres fonctionnalités nécessaires à SharePoint.

1.  Connectez-vous en tant qu’administrateur de domaine PRIV (PRIV\Administrateur) et lancez PowerShell.

2.  Tapez les commandes suivantes. Notez que vous devrez peut-être spécifier un autre emplacement pour les fichiers sources pour les fonctionnalités .NET Framework 3.5. Ces fonctionnalités ne sont généralement pas présentes durant l’installation de Windows Server, mais elles sont disponibles dans le dossier côte à côte (SxS), dans le dossier de sources du disque d’installation du système d’exploitation, par exemple d:\Sources\SxS.\.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Configurer la stratégie de sécurité de serveur
Configurez la stratégie de sécurité de serveur pour autoriser les comptes récemment créés à s'exécuter en tant que services.

1.  Lancez le programme **Stratégie de sécurité locale** .   
2.  Accédez à **Stratégies locales** > **Attribution des droits utilisateur**.  
3.  Dans le volet de détails, cliquez avec le bouton droit sur **Ouvrir une session en tant que service**, puis sélectionnez **Propriétés**.  
4.  Cliquez sur **Ajouter un utilisateur ou un groupe** et, dans Noms d’utilisateurs et de groupes, tapez *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Cliquez sur **Vérifier les noms**, puis sur **OK**.  

5.  Cliquez sur **OK** pour fermer la fenêtre Propriétés.  
6.  Dans le volet d’informations, cliquez avec le bouton droit sur **Interdire l’accès à cet ordinateur à partir du réseau**et sélectionnez **Propriétés**.  
7.  Cliquez sur **Ajouter un utilisateur ou un groupe** puis, dans Noms d’utilisateurs et de groupes, tapez *priv\mimmonitor; priv\MIMService; priv\mimcomponent* et cliquez sur **OK**.  
8.  Cliquez sur **OK** pour fermer la fenêtre Propriétés.  

9. Dans le volet d’informations, cliquez avec le bouton droit sur **Interdire l’ouverture d’une session locale**, puis sélectionnez **Propriétés**.  
10. Cliquez sur **Ajouter un utilisateur ou un groupe** puis, dans Noms d’utilisateurs et de groupes, tapez *priv\mimmonitor; priv\MIMService; priv\mimcomponent* et cliquez sur **OK**.  
11. Cliquez sur **OK** pour fermer la fenêtre Propriétés.  
12. Fermez la fenêtre Stratégie de sécurité locale.  

13. Ouvrez le Panneau de configuration, puis basculez vers **Comptes d’utilisateurs**.  
14. Cliquez sur **Accorder l’accès à cet ordinateur à des tiers**.  
15. Cliquez sur **Ajouter**, entrez le nom d’utilisateur *MIMADMIN* dans le domaine *PRIV* puis, dans l’écran suivant de l’Assistant, cliquez sur **Ajouter cet utilisateur en tant qu’administrateur**.  
16. Cliquez sur **Ajouter**, entrez le nom d’utilisateur *SharePoint* dans le domaine *PRIV* puis, dans l’écran suivant de l’Assistant, cliquez sur **Ajouter cet utilisateur en tant qu’administrateur**.  
17. Fermez le Panneau de configuration.  

### Changer la configuration d’IIS
Il existe deux façons de changer la configuration d’IIS pour permettre aux applications d’utiliser le mode d’authentification Windows. Vérifiez que vous êtes connecté en tant que MIMAdmin, puis suivez l’une de ces options.

Si vous souhaitez utiliser PowerShell :
1.  Cliquez avec le bouton droit sur PowerShell, puis sélectionnez **Exécuter en tant qu’administrateur**.  
2.  Arrêtez IIS, puis déverrouillez les paramètres d'hôte de l'application à l'aide des commandes suivantes.  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```  

Si vous souhaitez utiliser un éditeur de texte tel que le Bloc-notes :   
1. Ouvrez le fichier **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. Faites défiler jusqu’à la ligne 82 du fichier. La valeur de balise de **overrideModeDefault** doit être **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Remplacez la valeur de **overrideModeDefault** par *Autoriser*  
4. Enregistrez le fichier, puis redémarrez IIS à l’aide de la commande PowerShell `iisreset /START`

## Installer SQL Server
Si SQL Server n’est pas encore dans l’environnement bastion, installez SQL Server 2012 (Service Pack 1 ou version ultérieure) ou SQL Server 2014. Les étapes suivantes sont basées sur l'utilisation de SQL Server 2014.

1. Vérifiez que vous êtes connecté en tant que MIMAdmin.
2. Cliquez avec le bouton droit sur PowerShell, puis sélectionnez **Exécuter en tant qu’administrateur**.   
3. Accédez au répertoire où se trouve le programme d’installation de SQL Server.  
4. Tapez la commande suivante.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Installer SharePoint Foundation 2013

À l’aide du programme d’installation de SharePoint Foundation 2013 avec SP1, installez les composants logiciels nécessaires de SharePoint sur PAMSRV.

> [!NOTE]
> Ce programme d’installation nécessite une connexion Internet pour télécharger les composants requis. Une fois ces composants installés, le serveur redémarre.

1. Cliquez avec le bouton droit sur PowerShell, puis sélectionnez **Exécuter en tant qu’administrateur**.  
2. Accédez au répertoire où SharePoint a été décompressé.  
3. Tapez la commande `.\prerequisiteinstaller.exe`.

Une fois que les composants nécessaires de SharePoint sont installés, installez SharePoint Foundation 2013 avec SP1.

1.  Cliquez avec le bouton droit sur PowerShell, puis sélectionnez **Exécuter en tant qu’administrateur**.  
2.  Accédez au répertoire où SharePoint a été décompressé.  
3.  Tapez la commande `.\setup.exe`.  
4.  Sélectionnez le type de **serveur complet** .  
5.  Une fois l'installation terminée, exécutez l'Assistant.  

### Configurer Sharepoint
Exécuter l’Assistant Configuration des produits SharePoint pour configurer SharePoint.

1.  Sous l’onglet Se connecter à une batterie de serveurs, choisissez de **Créer une batterie de serveurs**.  
2.  Spécifiez **PAMSRV** comme serveur de bases de données pour la base de données de configuration et **PRIV\SharePoint** comme compte d’accès à la base de données à utiliser par SharePoint.  
3.  Spécifiez un mot de passe de sécurité de batterie de serveurs (il ne sera pas utilisé plus loin dans cette procédure pas à pas).  
4.  Pour le moment, acceptez le reste des paramètres par défaut de l’Assistant Configuration de SharePoint pour créer une batterie à un seul serveur.    
5.  Une fois que l’Assistant Configuration a terminé la tâche de configuration 10 sur 10, cliquez sur **Terminer**. Un navigateur web s’ouvre.  
6.  Dans la fenêtre contextuelle Internet Explorer, authentifiez-vous en tant qu’administrateur de domaine (PRIV\MIMAdmin) pour continuer.  
7.  Démarrez l’Assistant dans l’application web pour configurer la batterie de serveurs SharePoint.  
8.  Choisissez d’utiliser le compte géré existant (PRIV\SharePoint), décochez les services facultatifs, puis cliquez sur **Suivant**.  
9. Une fois que la fenêtre de création de collection de sites s’affiche, cliquez sur **Ignorer**, puis sur **Terminer**.  

## Créer une application web SharePoint Foundation 2013
Une fois l’Assistant terminé, utilisez PowerShell pour créer une Application web SharePoint Foundation 2013 pour héberger le portail MIM. Cette procédure pas à pas étant à des fins de démonstration, le protocole SSL n’est pas activé.

1.  Cliquez avec le bouton droit sur SharePoint 2013 Management Shell, sélectionnez **Exécuter en tant qu’administrateur**, puis exécutez le script PowerShell suivant :

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Un message d’avertissement signale que la méthode d’authentification Windows classique est utilisée et que l’exécution de la commande finale peut prendre plusieurs minutes.  Une fois terminé, la sortie indique l’URL du nouveau portail.

> [!NOTE]
> Laissez la fenêtre SharePoint 2013 Management Shell ouverte pour l’utiliser à l’étape suivante.

## Créer une collection de sites SharePoint
Ensuite, créez une collection de sites SharePoint associée à cette application web pour héberger le portail MIM.

1.  Lancez **SharePoint 2013 Management Shell**, s’il n’est pas encore ouvert, puis exécutez le script PowerShell suivant :

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Vérifiez que la variable **CompatibilityLevel** a la valeur *14*. Si le script retourne la valeur *15*, la collection de sites n’a pas été créée pour la version d’expérience 2010. Supprimez la collection de sites et recréez-la.

2.  Exécutez les commandes PowerShell suivantes dans **SharePoint 2013 Management Shell**. Elles désactivent l’état d’affichage côté serveur SharePoint et la tâche SharePoint **Tâche d’analyse de l’intégrité (Toutes les heures, Microsoft SharePoint Foundation Timer, Tous les serveurs)**.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## Modifier les paramètres de mise à jour

1. Ouvrez le Panneau de configuration, accédez à **Windows Update**, puis cliquez sur **Modifier les paramètres**.  
2. Modifiez les paramètres pour recevoir les mises à jour de Windows Update et des autres produits de Microsoft Updates.  
3. Vérifiez les mises à jour et assurez-vous que les mises à jour importantes en attente sont installées avant de continuer.

## Définir le site web en tant qu’intranet local

1. Lancer Internet Explorer et ouvrir un onglet dans le navigateur web
2. Accédez à http://pamsrv.priv.contoso.local:82/, puis connectez-vous en tant que PRIV\MIMAdmin.  Un site SharePoint vide nommé « Portail MIM » s'affiche.  
3. Dans Internet Explorer, ouvrez **Options Internet**, accédez à l’onglet **Sécurité**, sélectionnez **Intranet local**, puis ajoutez l’URL `http://pamsrv.priv.contoso.local:82/`.

En cas d’échec de la connexion, les noms de principaux du service (SPN) Kerberos créés à l’[Étape 2](step-2-prepare-priv-domain-controller.md) devront éventuellement être mis à jour.

## Démarrer le service d’administration SharePoint

À l’aide de **Services** (dans Outils d’administration), démarrez le service **Administration SharePoint**, s’il n’est pas en cours d’exécution.

À l’Étape 4, vous commencerez l’installation des composants MIM sur le serveur PAM.

>[!div class="step-by-step"]
[« Étape 2](step-2-prepare-priv-domain-controller.md)
[Étape 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Jul16_HO3-->


