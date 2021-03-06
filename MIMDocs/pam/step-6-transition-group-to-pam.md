---
title: "Étape 6 du déploiement PAM : Déplacer un groupe | Microsoft Identity Manager"
description: "Effectuez la migration d’un groupe vers la forêt PRIV afin qu’il puisse être géré avec Privilege Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 603e5e28f0eee0f648ef7e00ef137f5a08b2ba34


---

# Étape 6 – Effectuer la transition d'un groupe vers Privileged Access Management

>[!div class="step-by-step"]
[« Étape 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Étape 7 »](step-7-elevate-user-access.md)

La création d’un compte privilégié dans la forêt PRIV s’effectue à l’aide d’applets de commande PowerShell. Ces applets de commande remplissent les fonctions suivantes :

- Création d’un groupe dans la forêt PRIV avec le même identificateur de sécurité (SID) qu’un groupe dans la forêt CORP.  
- Création d’un objet dans la base de données du Service MIM correspondant au groupe dans la forêt PRIV.  
- Pour chaque compte d’utilisateur, création de deux objets dans la base de données du Service MIM. Ils correspondent à l’utilisateur situé dans la forêt CORP et au nouveau compte d’utilisateur situé dans la forêt PRIV.  
- Création d’un objet de rôle PAM dans la base de données du service MIM.  

Les applets de commande doivent être exécutées une fois pour chaque groupe et une fois pour chaque membre d’un groupe. Les applets de commande de migration ne modifient ni ne changent aucun utilisateur ou groupe de la forêt CORP : cela sera fait manuellement par l’administrateur PAM à un moment ultérieur.

1. Connectez-vous à PAMSRV, directement ou à partir d’une station de travail PRIV, en tant que *PRIV\MIMAdmin*.

2.  Lancez PowerShell et tapez les commandes suivantes.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Créez un compte d’utilisateur correspondant dans PRIV pour un compte d’utilisateur dans une forêt existante, à des fins de démonstration.

    Tapez les commandes suivantes dans PowerShell.  Si vous n’avez pas utilisé le nom *Jen* pour créer l’utilisateur dans contoso.local, modifiez les paramètres de la commande comme il convient. Le mot de passe « Pass@word1 » est juste un exemple. Vous devez le remplacer par une valeur de mot de passe unique.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Copiez un groupe et son membre, Jen, de CONTOSO vers le domaine PRIV, à des fins de démonstration.

    Exécutez les commandes suivantes, en spécifiant le mot de passe de l’administrateur de domaine CORP (CONTOSO\Administrateur) quand vous y êtes invité :

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Pour référence, la commande **New-PAMGroup** accepte les paramètres suivants :

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Facultatif) Sur CORPDC, supprimez le compte de Jen du groupe **CONTOSO CorpAdmins**, s’il est encore présent.  Cette opération est nécessaire uniquement pour illustrer comment les autorisations peuvent être associées aux comptes créés dans la forêt PRIV.

    1.  Connectez-vous à CORPDC en tant que *CONTOSO\Administrateur*.

    2.  Lancez PowerShell, exécutez la commande suivante, puis confirmez le changement.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Si vous souhaitez démontrer que les droits d’accès entre forêts sont efficaces pour le compte d’administrateur de l’utilisateur, passez à l’étape suivante.

>[!div class="step-by-step"]
[« Étape 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Étape 7 »](step-7-elevate-user-access.md)



<!--HONumber=Jul16_HO3-->


