---
title: "Étape 7 du déploiement PAM : Accès utilisateur | Microsoft Identity Manager"
description: "La dernière étape consiste à accorder un accès temporaire à un utilisateur disposant de privilèges pour illustrer la réussite de votre déploiement Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: b4b3f4c0605fabc7166e8ff8309078f80461301e


---

# Étape 7 – Élever l'accès d'un utilisateur

>[!div class="step-by-step"]
[« Étape 6 ](step-6-transition-group-to-pam.md)


Cette étape permet de montrer qu’un utilisateur peut demander l’accès à un rôle par le biais de MIM.

## Vérifier que Jen ne peut pas accéder à la ressource privilégiée
Sans privilèges élevés, Jen ne peut pas accéder à la ressource privilégiée dans la forêt CORP.

1. Déconnectez-vous de CORPWKSTN pour supprimer toute connexion ouverte mise en cache.
2. Connectez-vous à CORPWKSTN en tant que *CONTOSO\Jen*, puis basculez vers la vue **Bureau**.
3. Ouvrez une invite de commandes DOS.
4. Tapez la commande `dir \\corpwkstn\corpfs`. Le message d’erreur **Accès refusé** doit s’afficher.
5. Laissez la fenêtre d'invite de commandes ouverte.

## Demandez un accès privilégié à partir de MIM.
1. Sur CORPWKSTN, toujours en tant que CONTOSO\Jen, tapez la commande suivante.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. À l’invite, tapez le mot de passe du compte PRIV.Jen. Une nouvelle fenêtre d'invite de commandes s'affiche.
3. Quand la fenêtre PowerShell apparaît, tapez les commandes suivantes.

    > [!NOTE]
    > Une fois ces commandes exécutées, toutes les étapes suivantes ont une durée limitée.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. À la fin de l’opération, fermez la fenêtre PowerShell.
5. Dans la fenêtre de commandes DOS, tapez la commande suivante

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Tapez le mot de passe du compte PRIV.Jen. Une nouvelle fenêtre d'invite de commandes s'affiche.

## Validez l'accès avec élévation de privilèges.
Dans la fenêtre récemment ouverte, tapez les commandes suivantes :

```
whoami /groups
dir \\corpwkstn\corpfs
```

Si la commande dir échoue avec le message d’erreur **Accès refusé**, revérifiez la relation d’approbation.

## Activer le rôle privilégié
Procédez à l’activation en demandant un accès privilégié via l’exemple de portail PAM.

1. Sur CORPWKSTN, vérifiez que vous êtes connecté en tant que CORP\Jen.
2. Tapez la commande suivante dans une fenêtre de commandes DOS.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. À l’invite, tapez le mot de passe du compte PRIV.Jen. Une nouvelle fenêtre du navigateur web s'affiche.
4. Accédez à http://pamsrv.priv.contoso.local:8090 et vérifiez qu’une page web de l’exemple de portail est visible.
5. Dans Internet Explorer, sélectionnez **Outils** > **Options Internet**, puis cliquez sur l’onglet **Sécurité**.
6. Cliquez sur **Zone Intranet local** > **Sites** > **Avancé**, puis ajoutez le site web à la zone.
7. Fermez les boîtes de dialogue **Options Internet** .
8. Sous l’onglet gauche, cliquez sur **Activer**. Sélectionnez le **rôle PAM**, puis cliquez sur **Activer**.

> [!Note]
> Dans cet environnement, vous pouvez aussi apprendre à développer des applications qui utilisent l’API REST PAM, décrite dans [Référence de l’API REST PAM](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference.md).

## Résumé
Une fois que vous aurez effectué les étapes décrites dans cette procédure pas à pas, vous aurez expérimenté un scénario Privileged Access Management, où les privilèges de l’utilisateur sont élevés pour une durée limitée, ce qui lui permet d’accéder à des ressources protégées à l’aide d’un compte privilégié distinct. Dès l'expiration de la session d'élévation, le compte privilégié ne peut plus accéder à la ressource protégée. L’administrateur PAM décide des groupes de sécurité qui représentent les rôles privilégiés. Une fois que les droits d’accès ont été migrés vers le système Privileged Access Management, l’accès qui était jusqu’à maintenant possible grâce au compte d’utilisateur d’origine est désormais possible uniquement en se connectant par le biais d’un compte privilégié spécial, mis à disposition à la demande. Ainsi, les appartenances aux groupes dotés de privilèges élevés sont effectives pour une durée limitée.

>[!div class="step-by-step"]
[« Étape 6 ](step-6-transition-group-to-pam.md)



<!--HONumber=Jul16_HO3-->


