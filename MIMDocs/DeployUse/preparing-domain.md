---
# required metadata

title: Création d’un domaine | Microsoft Identity Manager
description: Créer un contrôleur de domaine Active Directory avant d’installer MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Création d’un domaine

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

## Créer les comptes et les groupes d’utilisateurs

MIM nécessite qu'Active Directory soit déjà installé. Vérifiez que vous disposez d'un contrôleur de domaine dans votre environnement pour un domaine que vous êtes en mesure d'administrer.

> [!NOTE]
> Dans tous les exemples ci-dessous, **mimservername** représente le nom de votre contrôleur de domaine, **contoso** représente votre nom de domaine, et **Pass@word1** représente un exemple de mot de passe.

1. Connectez-vous au contrôleur de domaine en tant qu’administrateur du domaine, par exemple *Contoso\Administrateur*.

2. Créez les comptes d’utilisateur suivants pour les services MIM. Démarrez PowerShell et tapez le script PowerShell suivant pour mettre à jour le domaine.

        ```
        import-module activedirectory
        $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
        New-ADUser –SamAccountName MIMMA –name MIMMA
        Set-ADAccountPassword –identity MIMMA –NewPassword $sp
        Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMSync –name MIMSync
        Set-ADAccountPassword –identity MIMSync –NewPassword $sp
        Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMService –name MIMService
        Set-ADAccountPassword –identity MIMService –NewPassword $sp
        Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
        Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
        Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName SharePoint –name SharePoint
        Set-ADAccountPassword –identity SharePoint –NewPassword $sp
        Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName SqlServer –name SqlServer
        Set-ADAccountPassword –identity SqlServer –NewPassword $sp
        Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
        Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
        Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
        ```

2.  Créez des groupes de sécurité pour tous les groupes.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global       –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global         –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

3.  Ajoutez les noms de principal du service pour activer l’authentification Kerberos des comptes de service

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S MIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S MIMSync/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)


<!--HONumber=Apr16_HO2-->


