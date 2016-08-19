---
title: "Créer des certificats logiciels | Microsoft Identity Manager"
description: "Découvrez comment utiliser le Gestionnaire de certificats pour créer et renouveler des certificats logiciels avec les modèles de profil."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: d5c8fc4f9a3eaab95441f7a915f7e02d55042ae9


---

# Créer des certificats logiciels avec Certificate Manager
Pour inscrire et renouveler des certificats logiciels, vous n'êtes pas obligé d'être administrateur et vous n'avez pas besoin de carte à puce virtuelle. À noter qu'à un moment donné vous serez invité à autoriser une opération de certificat. Ceci est normal.

## Créer un modèle de profil de certificat logiciel dans le Gestionnaire de certificats MIM 2016

1.  Créez un modèle pour le certificat que vous demanderez pour la carte à puce virtuelle. Ouvrez la console MMC.

2.  Cliquez sur **Fichier**, puis cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.

3.  Dans la liste des composants logiciels enfichables disponibles, cliquez sur **Modèles de certificats**, puis sur **Ajouter**.

4.  **Modèles de certificats** figure maintenant sous la racine de la console dans la console MMC. Double-cliquez dessus pour afficher tous les modèles de certificats disponibles.

5.  Cliquez avec le bouton droit sur le **Modèle utilisateur**, puis cliquez sur **Dupliquer le modèle**.

6.  Sous l’onglet **Compatibilité**, sous Autorité de Certification, sélectionnez Windows Server 2008 puis, sous Destinataire du certificat, sélectionnez Windows 8.1/Windows Server 2012 R2.

    1.  Sous l'onglet **Général** , dans le champ de nom complet, tapez **Modèle de certificat archivé**.

    2.  b.  Sous l'onglet **Traitement de la demande**

        1.  Définissez Signature et chiffrement comme **Objectif** .

        2.  Cochez la case **Inclure des algorithmes symétriques autorisés par le sujet**.

        3.  Si vous souhaitez archiver la clé, cochez la case **Archiver la clé privée de chiffrement du sujet**.

        4.  Sous Exécutez les opérations suivantes : sélectionnez **Demander à l'utilisateur lors de l'inscription**.

    3.  Sous l'onglet **Chiffrement**

        1.  Sous Catégorie de fournisseur, sélectionnez **Fournisseur de stockage de clés**.

        2.  Sélectionnez **Les demandes peuvent utiliser un fournisseur disponible sur l'ordinateur du sujet**.

    4.  Sous l'onglet **Sécurité** , ajoutez le groupe de sécurité auquel vous souhaitez accorder un accès d' **Inscription** . Par exemple, si vous souhaitez fournir l'accès à tous les utilisateurs, sélectionnez le groupe **Utilisateurs authentifiés** , puis sélectionnez **Autorisations d'inscription** .

    5.  Sous l'onglet **Nom du sujet**

        1.  Décochez la case **Inclure le nom de compte de messagerie dans le nom du sujet**.

        2.  Sous **Inclure cette information dans le nom de substitution du sujet**, décochez la case **Nom de messagerie électronique**.

    6.  Cliquez sur **OK** pour finaliser vos changements et créer le modèle. Votre nouveau modèle doit maintenant apparaître dans la liste des modèles de certificats.

    7.  Sélectionnez **Fichier**, puis cliquez sur **Ajouter/Supprimer un composant logiciel enfichable** pour ajouter le composant logiciel enfichable Autorité de certification à votre console MMC. Quand vous êtes invité à indiquer l'ordinateur que vous souhaitez gérer, sélectionnez **Ordinateur local**.

    8.  Dans le volet gauche de la console MMC, développez **Autorité de certification (local)**, puis développez votre autorité de certification dans la liste des autorités de certification.

    9. Cliquez avec le bouton droit sur le conteneur **Modèles de certificats**, cliquez sur **Nouveau**, puis sur **Modèle de certificat** à délivrer.

    10. Dans la liste, sélectionnez le modèle que vous venez de créer (**Modèle de certificat archivé**), puis cliquez sur **OK**.

## Créer le modèle de profil

1.  Connectez-vous au portail CM en tant qu'utilisateur disposant de privilèges d'administrateur.

2.  Accédez à **Administration &gt; Gérer les modèles de profils**, vérifiez que la case est cochée en regard de l’**exemple de modèle de profil de connexion par carte à puce MIM CM**, puis cliquez sur **Copier un modèle de profil sélectionné**.

3.  Tapez le nom du modèle de profil, puis cliquez sur **OK**.

4.  Dans l'écran suivant, cliquez sur **Ajouter un nouveau modèle de certificat** , puis veillez à cocher la case en regard du nom de l'autorité de certification.

5.  Cochez la case en regard du nom du certificat logiciel archivé et cliquez sur **Ajouter**.

6.  Supprimez le modèle utilisateur en cochant la case en regard de celui-ci et en cliquant sur **Supprimer les modèles de certificat sélectionnés** , puis sur **OK**.

7.  Cliquez sur **Modifier les paramètres généraux**.

8.  Cochez les cases à gauche de **Générer des clés de chiffrement sur le serveur** , puis cliquez sur **OK**. Dans le volet gauche, cliquez sur **Stratégie de récupération**.

9. Cliquez sur **Modifier les paramètres généraux**.

10. Si vous souhaitez réémettre des certificats archivés, cochez les cases à gauche de **Réémettre les certificats archivés** , puis cliquez sur **OK**.

11. Si vous utilisez la Gestion des certificats de cartes à puce virtuelles, vous devez désactiver les éléments de collection de données, car elle ne fonctionne pas quand la collection de données est activée. Désactivez la collection de données pour chaque stratégie. Pour ce faire, cliquez sur la stratégie dans le volet gauche, décochez la case en regard d' **Élément de données exemple** , puis cliquez sur **Supprimer les éléments de la collection de données**. Cliquez sur **OK**.



<!--HONumber=Jul16_HO3-->


