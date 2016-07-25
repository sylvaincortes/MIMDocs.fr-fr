---
title: "Renouvellement de carte à puce en libre-service | Microsoft Identity Manager"
description: "Découvrez comment les utilisateurs sans accès administrateur à leur ordinateur peuvent inscrire des cartes à puce afin de pouvoir utiliser le Gestionnaire de certificats."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 2fddede481b5ba677d0d463be4b14cda4b463865


---

# Inscrire des cartes à puce pour les non-administrateurs
Si un utilisateur n’est pas un administrateur local sur son ordinateur, par défaut il ne pourra pas inscrire de carte à puce sur son propre ordinateur. La procédure suivante vous permet de contourner cette limitation.

## Activation du renouvellement de carte à puce pour les non-administrateurs dans le Gestionnaire de certificats MIM 2016

1.  **Décompresser le fichier appx**

    Obtenez un certificat de signature. Suivez les étapes pour [signer des applications Windows 8 à l’aide d’une infrastructure PKI interne](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Arrêtez-vous quand vous parvenez à « Signer l'application ». Nommez le fichier pfx exporté. Exportez également vers un fichier .cer et importez-le sur le client à l'aide du fichier cer du nouveau certificat de signature.

    Exécutez la commande suivante pour décompresser le fichier appx :

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Modifier le fichier de configuration**

    Renommez le fichier nommé `CustomDataExample.xml custom.data`. L'application CM recherchera ce nom de fichier.

    Modifiez le fichier custom.data et modifiez ce qui suit :

    1.  Dans l’élément &lt;NonAdmin&gt;, affectez à l’attribut Value la valeur « True ».

    2.  Enregistrez le fichier et quittez l'éditeur.

    3.  Supprimez le fichier nommé AppxSignature.p7x.

    4.  Modifiez le fichier nommé AppxManifest.xml.

    5.  Dans l’élément &lt;Identity&gt;, affectez comme valeur de l’attribut Publisher le sujet de votre certificat de signature, par exemple : "CN=ABCD"

        Le sujet ici doit être identique à celui du certificat de signature que vous utilisez pour signer l'application.

    6.  Enregistrez le fichier et quittez l'éditeur.

3.  **Réempaqueter et signer le package d'application (fichier appx)**

    Exécutez la commande suivante pour empaqueter et signer le fichier appx :

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Dupliquez le modèle de profil et ajoutez la clé d'administration initiale pour configurer le serveur MIM :

    1.  Connectez-vous au portail CM en tant qu'utilisateur disposant de privilèges d'administrateur.

    2.  Accédez à **Administration** &gt; **Gérer les modèles de profils** et assurez-vous que la case est cochée en regard du modèle de profil que vous avez créé, puis cliquez sur Copier un modèle de profil sélectionné.

    3.  Tapez le nom du modèle de profil, ajoutez « nonAdmin » et cliquez sur **OK**.

    4.  Quand les paramètres généraux du modèle de profil s’affichent, faites défiler vers le bas jusqu’au bout et, sous **Configuration de la carte à puce**, cliquez sur **Modifier les paramètres**.

    5.  Sous **Valeur initiale de la clé Admin (hexadécimal)** , entrez la clé d'administration par défaut : "010203040506070801020304050607080102030405060708"

    6.  Faites défiler et cliquez sur **OK**.

5.  **Créer un compte non-administrateur sur l'ordinateur client**

    Les utilisateurs non-administrateurs ne peuvent pas créer la carte à puce virtuelle sur le module de plateforme sécurisée. Vous devez donc la créer pour eux.

6.  **Créer une carte à puce virtuelle à l’aide de TpmVscMgr**

    Procédez comme suit (toujours en tant qu’administrateur) pour créer une carte à puce virtuelle vide sur un ordinateur. Vous pouvez effectuer cette procédure via Intune, SCCM ou la stratégie de groupe.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Installer l'application CM dans le compte non-administrateur**

8.  **Lancer l’application CM et s’inscrire pour une carte à puce virtuelle**



<!--HONumber=Jul16_HO3-->


