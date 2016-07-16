---
title: "Utilisation du Gestionnaire de certificats MIM | Microsoft Identity Manager"
description: "Apprenez à déployer l’application Gestionnaire de certificats pour permettre aux utilisateurs de gérer leurs propres droits d’accès."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: 3e0e6cea0b268836bb6347e81694deec93320ce3


---

# Utilisation du Gestionnaire de certificats MIM
Une fois que vous vous êtes familiarisé avec MIM 2016 et le Gestionnaire de certificats, vous pouvez déployer l'application du Windows Store Gestionnaire de certificats MIM pour permettre aux utilisateurs de gérer facilement leurs cartes à puce physiques, leurs cartes à puce virtuelles et leurs certificats logiciels. Les étapes de déploiement de l'application MIM CM sont les suivantes :

1.  Créer un modèle de certificat.

2.  Créer un modèle de profil.

3.  Préparer l'application.

4.  Déployer l'application via SCCM ou Intune.

## Créer un modèle de certificat
Vous créez un modèle de certificat pour l'application Gestionnaire de certificats (CM) de manière classique, à ceci près que vous devez vous assurer que le modèle de certificat correspond à la version 3 ou ultérieure.

1.  Connectez-vous au serveur exécutant les services AD CS (serveur de certificats).

2.  Ouvrez la console MMC.

3.  Cliquez sur **Fichier &gt; Ajouter/Supprimer un composant logiciel enfichable**.

4.  Dans la liste des composants logiciels enfichables disponibles, cliquez sur **Modèles de certificats**, puis sur **Ajouter**.

5.  Vous voyez maintenant **Modèles de certificats** sous **Racine de la console** dans la console MMC. Double-cliquez dessus pour afficher tous les modèles de certificats disponibles.

6.  Cliquez avec le bouton droit sur le modèle **Connexion par carte à puce**, puis cliquez sur **Dupliquer le modèle**.

7.  Sous l'onglet compatibilité, sous Autorité de Certification, sélectionnez Windows Server 2008, puis sous Destinataire du certificat, sélectionnez Windows 8.1/Windows Server 2012 R2.
    Cette étape est cruciale, car elle permet de vérifier que vous avez un modèle de certificat version 3 (ou ultérieure). En effet, seule la version 3 (au minimum) fonctionne avec l'application Gestionnaire de certificats. Dans la mesure où la version est définie la première fois que vous créez et enregistrez le modèle de certificat, si vous n'avez pas créé ce dernier en procédant comme indiqué, il n'existe aucun moyen de le modifier en fonction de la version appropriée. Dans ce cas, vous devez créer un autre modèle de certificat avant de continuer.

8.  Sous l'onglet **Général** , dans le champ **Nom complet** , tapez le nom que vous souhaitez voir apparaître dans l'interface utilisateur de l'application, par exemple **Connexion par carte à puce virtuelle**.

9. Sous l’onglet **Gestion de la demande**, dans **Objet**, indiquez **Signature et chiffrement**, puis sous **Procéder ainsi à réception d’une demande de certificat** sélectionnez **Demander à l'utilisateur lors de l'inscription**.

10. Sous l'onglet **Chiffrement** , sous **Catégorie de fournisseur**, sélectionnez **Fournisseur de stockage de clés et Les demandes peuvent utiliser un fournisseur disponible sur l'ordinateur du sujet**.

    > [!NOTE]
    > Vous verrez seulement l'option Fournisseur de stockage de clés, si votre modèle correspond à la version 3. Si vous ne voyez pas l'option, cela signifie probablement que vous n'avez pas créé le modèle de certificat avec la version appropriée. Recommencez à l'étape 5, ci-dessus.

11. Sous l'onglet **Sécurité** , ajoutez le groupe de sécurité pour lequel vous souhaitez autoriser l' **Inscription** . Par exemple, si vous souhaitez permettre l'accès à tous les utilisateurs, sélectionnez le groupe **Utilisateurs authentifiés** , puis sélectionnez **Autorisations d'inscription** .

12. Cliquez sur **OK** pour finaliser vos changements et créer le modèle. Normalement, vous devez voir votre nouveau modèle dans la liste des modèles de certificats.

13. Sélectionnez **Fichier**, puis cliquez sur **Ajouter/Supprimer un composant logiciel enfichable** pour ajouter le composant logiciel enfichable Autorité de certification à votre console MMC. Quand vous êtes invité à indiquer l'ordinateur que vous souhaitez gérer, sélectionnez **Ordinateur local**.

14. Dans le volet gauche de la console MMC, développez **Autorité de certification (local)** , puis développez votre autorité de certification dans la liste des autorités de certification.

15. Cliquez avec le bouton droit sur **Modèles de certificats**, cliquez sur **Nouveau &gt; Modèle de certificat à délivrer**.

16. Dans la liste, sélectionnez le modèle que vous avez créé, puis cliquez sur **OK**.

## Créer un modèle de profil
Quand vous créez un modèle de profil, veillez à le définir pour créer/détruire la carte à puce virtuelle, et supprimer la collection de données. L'application CM ne peut pas gérer les données collectées. Il est donc important de les désactiver, comme suit.

1.  Connectez-vous au portail CM en tant qu'utilisateur disposant de privilèges d'administrateur.

2.  Accédez à Administration &gt; Gérer les modèles de profils, vérifiez que la case est cochée en regard de l’exemple de modèle de profil de connexion par carte à puce MIM CM, puis cliquez sur Copier un modèle de profil sélectionné.

3.  Tapez le nom du modèle de profil, puis cliquez sur **OK**.

4.  Dans l'écran suivant, cliquez sur **Ajouter un nouveau modèle de certificat** , puis veillez à cocher la case en regard du nom de l'autorité de certification.

5.  Cochez la case en regard du nom du modèle de profil **Connexion** , puis cliquez sur **Ajouter**.

6.  Supprimez le modèle SmartCardLogon. Pour ce faire, cochez la case en regard de celui-ci, cliquez sur **Supprimer les modèles de certificat sélectionnés** , puis sur **OK**.

7.  Faites défiler vers le bas, puis cliquez sur **Modifier les paramètres**.

8.  Cochez les cases en regard de **Création/destruction d’une carte à puce virtuelle** et **Diversifier la clé Admin**.

9. Sous **Stratégie de code confidentiel utilisateur** , sélectionnez **Fourni par l'utilisateur**.

10. Dans le volet gauche, cliquez sur **Stratégie de renouvellement &gt; Modifier les paramètres généraux**. Sélectionnez **Réutiliser la carte lors du renouvellement** , puis cliquez sur **OK**.

11. Vous devez désactiver les éléments de collection de données pour chaque stratégie. Pour ce faire, cliquez sur la stratégie dans le volet gauche, cochez la case en regard d' **Élément de données exemple** , puis cliquez sur **Supprimer les éléments de la collection de données**. Cliquez sur **OK**.

## Préparer l'application CM au déploiement

1.  À l'invite de commandes, exécutez la commande ci-après. Elle vous permet de décompresser l'application et d'extraire le contenu dans un nouveau sous-dossier nommé appx, et de créer une copie pour ne pas modifier le fichier d'origine.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  Dans le dossier appx, remplacez le nom du fichier CustomDataExample.xml par Custom.data

3.  Ouvrez le fichier Custom.data, puis changez les paramètres, le cas échéant.

    |||
    |-|-|
    |MIMCM URL|Nom de domaine complet du portail que vous avez utilisé pour configurer CM. Par exemple, https://mimcmServerAddress/certificatemanagement|
    |ADFS URL|Si vous comptez utiliser AD FS, insérez votre URL AD FS. Par exemple, https://adfsServerSame/adfs|
    |PrivacyUrl|Vous pouvez inclure une URL vers une page web expliquant ce que vous faites des détails utilisateur collectés pour l'inscription de certificats.|
    |SupportMail|Vous pouvez inclure une adresse de messagerie pour les problèmes de support.|
    |LobComplianceEnable|Vous pouvez affecter la valeur true ou false à ce paramètre. Par défaut, il a la valeur true.|
    |MinimumPinLength|Par défaut, il a la valeur 6.|
    |NonAdmin|Vous pouvez affecter la valeur true ou false à ce paramètre. Par défaut, il a la valeur false. Modifiez ce paramètre uniquement si vous souhaitez que les utilisateurs qui ne sont pas des administrateurs sur leurs ordinateurs puissent inscrire et renouveler des certificats.|

4.  Enregistrez le fichier et quittez l'éditeur.

5.  La signature du package entraîne la création d'un fichier de signature. Vous devez donc supprimer le fichier de signature d'origine nommé AppxSignature.p7x.

6.  Le fichier AppxManifest.xml spécifie le nom de sujet du certificat de signature. Ouvrez ce fichier pour le modifier.

7.  Vous devez obtenir un certificat de signature avant de débuter cette section. Consultez ci-dessous, Activation du renouvellement de carte à puce pour les non-administrateurs dans le Gestionnaire de certificats MIM 2016, étape 1.

8.  Dans l’élément &lt;Identity&gt;, changez la valeur de l’attribut Publisher pour qu’elle soit identique à la valeur du sujet indiqué dans votre certificat de signature, par exemple "CN=SUBJECT".

9. Enregistrez le fichier et quittez l'éditeur.

10. À l'invite de commandes, exécutez la commande suivante pour recompresser et signer le fichier .appx.

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    où le nom du package d'application est le même que le nom utilisé quand vous avez créé la copie.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Ceci permet d'obtenir le nouveau fichier .appx. Le fichier pfx fournit un emplacement pour le certificat de signature, et un mot de passe pour le fichier .pfx.

11. Pour utiliser l'authentification AD FS :

    -   Ouvrez l'application Carte à puce virtuelle. Cela vous permet de trouver plus facilement les valeurs nécessaires pour la prochaine étape.

    -   Pour ajouter l’application en tant que client au serveur AD FS et configurer CM sur le serveur, ouvrez Windows PowerShell sur le serveur AD FS et exécutez la commande `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

        Voici le script ConfigureMimCMClientAndRelyingParty.ps1 :

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   Mettez à jour les valeurs de redirectUri et serverFQDN.

    -   Pour trouver redirectUri, dans l'application Carte à puce virtuelle, ouvrez le panneau des paramètres d'application, puis cliquez sur **Paramètres**. L'URI de redirection doit être listée sous la barre d'adresses du serveur AD FS. L'URI apparaît uniquement si une adresse de serveur ADFS est configurée.

    -   Le nom de domaine complet (FQDN) du serveur correspond uniquement au nom complet de l'ordinateur du serveur MIMCM.

    -   Pour obtenir de l’aide sur le script **ConfigureMIimCMClientAndRelyingParty.ps1**, exécutez `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`

## Déployer l'application
Quand vous configurez l’application CM, dans le Centre de téléchargement, téléchargez le fichier MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip, puis extrayez l’ensemble de son contenu. Le fichier .appx représente le programme d'installation. Vous pouvez le déployer de la même façon qu’une application du Windows Store, à l’aide de [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx)ou d’[Intune](https://technet.microsoft.com/library/dn613839.aspx) pour charger une version test de l’application, et permettre aux utilisateurs d’y accéder via le portail d’entreprise ou de l’obtenir directement par transmission de type push sur leurs ordinateurs.



<!--HONumber=Jun16_HO4-->


