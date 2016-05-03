---
# required metadata

title: Configurer un serveur de gestion des identités&#58; SharePoint | Microsoft Identity Manager
description: Installer et configurer SharePoint Foundation pour héberger la page du portail MIM. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurer un serveur de gestion d’identité : SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Dans tous les exemples ci-dessous, **mimservername** représente le nom de votre contrôleur de domaine, **contoso** représente votre nom de domaine, et **Pass@word1** représente un exemple de mot de passe.


## Installer **SharePoint Foundation 2013 avec SP1**.

> [!NOTE]
> Pour télécharger les composants requis, le programme d'installation aura besoin d'une connectivité Internet.

Le serveur redémarrera à la fin de l'installation.

1.  Lancez **PowerShell** en tant qu'administrateur de domaine.

    -   Accédez au répertoire où SharePoint a été décompressé.

    -   Tapez la commande suivante.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Une fois les composants requis de **SharePoint** installés, installez **SharePoint Foundation 2013 avec SP1** en tapant la commande suivante :

    ```
    .\setup.exe
    ```

3.  Sélectionnez le type de serveur complet.

4.  Une fois l'installation terminée, exécutez l'Assistant.

## Exécuter l’Assistant pour configurer SharePoint

Suivez les étapes de l’**Assistant Configuration des produits SharePoint** pour configurer SharePoint afin qu’il fonctionne avec MIM.

1. Sous l'onglet **Se connecter à une batterie de serveurs** , choisissez de créer une batterie de serveurs.

2. Spécifiez ce serveur comme serveur de bases de données pour la base de données de configuration et *Contoso\SharePoint* comme compte d’accès à la base de données à utiliser par SharePoint.

3. Spécifiez un mot de passe de sécurité de batterie de serveurs (il ne sera pas utilisé plus loin dans cet environnement de laboratoire).

4. Une fois que l'Assistant Configuration a terminé la tâche de configuration 10 sur 10, cliquez sur Terminer. Un navigateur web s'ouvre.

5. Dans la fenêtre Internet Explorer, authentifiez-vous en tant que *Contoso\Administrateur* (ou avec le compte d’administrateur de domaine équivalent) pour continuer.

6. Démarrez l'Assistant (dans l'application web) pour configurer la batterie de serveurs SharePoint.

7. Sélectionnez l’option d’utilisation du compte géré existant (*Contoso\SharePoint*), puis cliquez sur **Suivant**.

8. Dans la fenêtre **Création d'une collection de sites** , cliquez sur **Ignorer**.  Ensuite, cliquez sur **Terminer**.

## Préparer SharePoint pour héberger le portail MIM

1. Créez une **application web SharePoint Foundation 2013**.

    > [!NOTE]
    > Initialement, le protocole SSL ne sera pas configuré. Veillez à configurer le protocole SSL ou équivalent avant d'activer l'accès à ce portail.

    1. Lancez  **SharePoint 2013 Management Shell** et exécutez le script PowerShell suivant :

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
        -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
        ```

        2. Notez qu'un message d'avertissement signale que la méthode d'authentification Windows classique est utilisée et que l'exécution de la commande finale peut prendre plusieurs minutes.  Une fois terminé, la sortie indique l'URL du nouveau portail.  Laissez la fenêtre **SharePoint 2013 Management Shell** ouverte. Vous y reviendrez lors d'une tâche ultérieure.

2. Créez une **collection de sites SharePoint** associée à cette application web.

    1. Lancez SharePoint 2013 Management Shell et exécutez le script PowerShell suivant :

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://corpidm.contoso.local:82
        New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
        -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```

        2. Vérifiez que le résultat de la variable *CompatibilityLevel* est « 14 ».  ([Consultez Installation de FIM 2010 R2 sur SharePoint Foundation 2013](http://technet.microsoft.com/library/jj863242.aspx) pour plus d’informations). Si le résultat est « 15 », la collection de sites n'a pas été créée pour la version d'expérience 2010. Supprimez la collection de sites et recréez-la.

3. Désactivez **viewstate côté serveur SharePoint** et la tâche SharePoint « Tâche d’analyse de l’intégrité (Toutes les heures, Minuteur de Microsoft SharePoint Foundation, Tous les serveurs) » en exécutant les commandes PowerShell suivantes dans **SharePoint 2013 Management Shell** :

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

4. Sur votre serveur de gestion d’identité, ouvrez un nouvel onglet dans le navigateur web, accédez à http://localhost:82/ et connectez-vous en tant que *contoso\Administrateur*.  Un site SharePoint vide nommé *Portail MIM* s'affiche.

    ![Portail MIM à l’adresse http://localhost:82/image](media/MIM-DeploySP1.png)

5. Copiez l’URL, puis dans Internet Explorer, ouvrez **Options Internet**, accédez à l’onglet **Sécurité**, sélectionnez **Intranet local**, puis cliquez sur **Sites**.

    ![Image des Options Internet](media/MIM-DeploySP2.png)

6. Dans la fenêtre **Intranet local**, cliquez sur **Avancé** et collez l’URL copiée dans la zone de texte **Ajouter ce site Web à la zone**. Cliquez sur **Ajouter**, puis fermez les fenêtres.

7. Ouvrez le programme **Outils d’administration**, accédez à **Services**, recherchez le service Administration SharePoint et démarrez-le si ce n’est déjà fait.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)


<!--HONumber=Apr16_HO2-->


