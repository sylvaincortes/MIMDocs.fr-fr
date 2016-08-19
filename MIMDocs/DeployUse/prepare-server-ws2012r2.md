---
title: Configurer Windows Server | Microsoft Identity Manager
description: "Découvrez les étapes et la configuration minimale requise pour préparer Windows Server 2012 RS à travailler avec MIM 2016."
keywords: 
author: kgremban
manager: femila
ms.date: 08/11/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 739797502e09c2b92e35767e2c943308cd1de5c9
ms.openlocfilehash: bdf1f4b7c5762095f4986506f9e17369dadea011


---

# Configurer un serveur de gestion d’identité : Windows Server 2012 R2

>[!div class="step-by-step"]
[« Préparation d’un domaine](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> Cette procédure pas à pas utilise des exemples de noms et de valeurs tirés d’une société appelée Contoso. Remplacez-les par les vôtres. Exemple :
> - Nom du contrôleur de domaine : **mimservername**
> - Nom de domaine : **contoso**
> - Mot de passe : **Pass@word1**

## Joindre votre domaine Windows Server 2012 R2

Commencez avec un ordinateur Windows Server 2012 R2, avec un minimum de 8 Go de RAM. Lors de l'installation, spécifiez l'édition « Windows Server 2012 R2 (serveur avec une interface graphique utilisateur) x64 ».

1. Ouvrez une session sur le nouvel ordinateur en tant qu'administrateur.

2. Utilisez le panneau de configuration, donnez une adresse IP statique à l’ordinateur sur le réseau. Configurez cette interface réseau pour envoyer des requêtes DNS à l’adresse IP du contrôleur de domaine à l’étape précédente et nommez l’ordinateur **CORPIDM**.  Cette opération nécessite un redémarrage du serveur.

3. Ouvrez le Panneau de configuration et joignez l’ordinateur au domaine que vous avez configuré dans la dernière étape, *contoso.local*.  Vous devrez fournir le nom d’utilisateur et les informations d’identification d’un administrateur de domaine tel que *Contoso\Administrateur*.  Après l'affichage du message de bienvenue, fermez la boîte de dialogue et redémarrez ce serveur.

4. Connectez-vous à l’ordinateur *CorpIDM* en tant qu’administrateur de domaine, comme *Contoso\Administrateur*.

5. Lancez une fenêtre PowerShell en tant qu’administrateur et tapez la commande suivante pour mettre à jour l’ordinateur avec les paramètres de stratégie de groupe.

    ```
    gpupdate /force /target:computer
    ```

    Au bout d’une minute maximum, la commande se termine et affiche le message « La mise à jour de la stratégie d’ordinateur s’est terminée sans erreur ».

6. Ajoutez les rôles **Serveur Web (IIS)** et **Serveur d’applications**, les fonctionnalités **.NET Framework** 3.5, 4.0 et 4.5 et le module **Active Directory** pour Windows PowerShell.

    ![Image des fonctionnalités de PowerShell](media/MIM-DeployWS2.png)

7. Dans PowerShell, tapez les commandes suivantes. Notez que vous devrez peut-être spécifier un autre emplacement pour les fichiers sources pour les fonctionnalités **.NET Framework** 3.5. Ces fonctionnalités ne sont généralement pas présentes durant l’installation de Windows Server, mais elles sont disponibles dans le dossier côte à côte (SxS) dans le dossier de sources du disque d’installation du système d’exploitation, par exemple « *d:\Sources\SxS\* ».

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## Configurer la stratégie de sécurité de serveur

Configurez la stratégie de sécurité de serveur pour autoriser les comptes récemment créés à s’exécuter en tant que services.

1. Lancez le programme Stratégie de sécurité locale.

2. Accédez à **Stratégies locales > Attribution des droits utilisateur**.

3. Dans le volet de détails, cliquez avec le bouton droit sur **Ouvrir une session en tant que service**, puis sélectionnez **Propriétés**.

    ![Image de la stratégie de sécurité locale](media/MIM-DeployWS3.png)

4. Cliquez sur **Ajouter un utilisateur ou un groupe** et, dans la zone de texte, tapez `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, cliquez sur **Vérifier les noms**, puis cliquez sur **OK**.

5. Cliquez sur **OK** pour fermer la fenêtre **Propriétés de Ouvrir une session en tant que service**.

6.  Dans le volet d’informations, cliquez avec le bouton droit sur **Interdire l’accès à cet ordinateur à partir du réseau**et sélectionnez **Propriétés**.

7. Cliquez sur **Ajouter un utilisateur ou un groupe** et, dans la zone de texte, tapez `contoso\MIMSync; contoso\MIMService`, puis cliquez sur **OK**.

8. Cliquez sur **OK** pour fermer la fenêtre **Interdire l’accès à cet ordinateur à partir du réseau**.

9. Dans le volet d’informations, cliquez avec le bouton droit sur **Interdire l’ouverture d’une session locale**, puis sélectionnez **Propriétés**.

10. Cliquez sur **Ajouter un utilisateur ou un groupe** et, dans la zone de texte, tapez `contoso\MIMSync; contoso\MIMService`, puis cliquez sur **OK**.

11. Cliquez sur **OK** pour fermer la fenêtre **Propriétés de Interdire l’ouverture d’une session locale**.

12. Fermez la fenêtre Stratégie de sécurité locale.


## Modifier le Mode d’authentification Windows IIS

1.  Ouvrez une fenêtre PowerShell.

2.  Arrêtez les services IIS à l’aide de la commande *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« Préparation d’un domaine](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)



<!--HONumber=Aug16_HO2-->


