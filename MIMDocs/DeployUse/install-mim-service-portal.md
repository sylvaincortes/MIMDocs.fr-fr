---
title: Installer le portail et le service MIM | Microsoft Identity Manager
description: "Obtenir les étapes pour configurer et installer le service et le portail MIM pour Microsoft Identity Manager 2016"
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: c18ea7b0390ca11c213ed66bfd1476454cf86951


---

# Installer MIM 2016 : service et portail MIM

>[!div class="step-by-step"]
[« Service de synchronisation MIM](install-mim-sync.md)
[Synchroniser les bases de données »](install-mim-sync-ad-service.md)

> [!NOTE]
> Cette procédure pas à pas utilise des exemples de noms et de valeurs tirés d’une société appelée Contoso. Remplacez-les par les vôtres. Exemple :
> - Nom du contrôleur de domaine : **mimservername**
> - Nom de domaine : **contoso**
> - Mot de passe : **Pass@word1**
> - Nom du compte de service : **MIMService**

Si vous n’avez pas configuré le package d’installation MIM lors de la dernière étape, revenez en arrière et installez les composants de Microsoft Identity Manager 2016 avant de continuer.


## Configurer le service MIM et le portail pour l’installation

1. Exécutez le programme d’installation du **service MIM et du portail** à partir du sous-dossier décompressé **Service et portail**.

2. Dans l'écran d'accueil, cliquez sur **Suivant**.

3. Lisez le Contrat de Licence Utilisateur Final et cliquez sur **Suivant** si vous en acceptez les termes.

4. Dans l’écran **Programme d’amélioration du produit MIM**, cliquez sur **Suivant**.

5. Lorsque vous sélectionnez les fonctionnalités de ce déploiement, veillez à inclure les fonctionnalités de service MIM (à l’exception de MIM Reporting) et de portail MIM. Vous pouvez également sélectionner le portail d’inscription de mot de passe MIM et le service de notification de modification de mot de passe MIM.

6. Dans la page **Configurer la connexion de base de données MIM**, choisissez **Créer une nouvelle base de données**.

    ![Image de la configuration de la connexion de base de données MIM](media/MIM-Install10.png)

7. Dans **Configurer la connexion au serveur de messagerie**, entrez le nom de votre serveur Exchange en tant que **serveur de messagerie**. Si vous n’avez aucun serveur de messagerie configuré, utilisez **localhost** comme nom de serveur de messagerie, puis décochez les deux premières cases. Cliquez sur **Suivant**.

    ![Image de la configuration de la connexion au serveur de messagerie](media/MIM-Install11.png)

8. Spécifiez que vous souhaitez générer un nouveau certificat auto-signé ou sélectionnez le certificat approprié.

9. Spécifiez le nom du compte de service à utiliser, par exemple *MIMService*, le mot de passe correspondant, par exemple *Pass@word1*, votre domaine de compte de service, par exemple *contoso*, ainsi que le compte de messagerie du service, par exemple *contoso*.

    ![Image de la configuration du compte de service](media/MIM-Install12.png)

10. Notez qu'un avertissement peut s'afficher en indiquant que le compte de service n'est pas sécurisé dans sa configuration actuelle.

11. Acceptez les valeurs par défaut pour l’emplacement du serveur de synchronisation et spécifiez *contoso\MIMsync* comme compte d’agent de gestion MIM.

    ![Image de la configuration du service et du portail MIM](media/MIM-Install13.png)

12. Spécifiez *CORPIDM (nom de cet ordinateur)* en tant qu’adresse du serveur du service MIM pour le portail MIM.

13. Spécifiez *http://CorpIDM.contoso.local:82* comme URL de la collection de sites SharePoint.

14. Spécifiez *http://CorpIDM.contoso.local:8080* comme URL d’inscription du mot de passe.

15. Spécifiez *http://CorpIDM.contoso.local:8088* comme URL de réinitialisation du mot de passe.

16. Cochez la case permettant d'ouvrir les ports 5725 et 5726 dans le pare-feu, puis la case permettant d'accorder à tous les utilisateurs authentifiés l'accès au portail MIM.

## Configurer le portail d’enregistrement du mot de passe MIM

1.  Spécifiez *contoso\MIMSSPR* comme nom du compte pour l’inscription SSPR avec le mot de passe *Pass@word1*.

2.  Spécifiez *CORPIDM* en tant que nom d’hôte pour l’enregistrement de mot de passe MIM, puis définissez le port **8080**. Activez l’option **Ouvrir le port dans le pare-feu**.

    ![Entrez les informations de configuration utilisées par l’image IIS](media/MIM-Install14.png)

3.  Un avertissement s'affiche. Lisez-le, puis cliquez sur **Suivant**.

4. Dans l’écran de configuration Portail d’enregistrement du mot de passe MIM, spécifiez *http://CorpIDM.contoso.local* en tant qu’adresse du serveur de Service MIM pour le portail de l’enregistrement du mot de passe.

## Configurer le portail de réinitialisation du mot de passe MIM

1.  Spécifiez *Contoso\MIMSSPRService* comme nom du compte pour l’inscription SSPR avec le mot de passe *Pass@word1*.

2.  Spécifiez *CORPIDM* en tant que nom d’hôte pour l’enregistrement de mot de passe MIM, puis définissez le port **8080**. Activez l’option **Ouvrir le port dans le pare-feu**.

    ![Entrez les informations de configuration utilisées par l’image IIS](media/MIM-Install15.png)

3.  Un avertissement s'affiche. Lisez-le, puis cliquez sur **Suivant**.

4. Dans l’écran de configuration Portail d’enregistrement du mot de passe MIM, spécifiez *http://CorpIDname.domain.local* en tant qu’adresse du serveur de Service MIM pour le portail de réinitialisation du mot de passe.

## Installer le service et le portail MIM

Lorsque toutes les définitions de préinstallation sont prêtes, cliquez sur **Installer** pour commencer l’installation des composants du **service et du portail** sélectionnés.

Une fois l'installation achevée, vérifiez que le portail MIM est actif.

1. Lancez Internet Explorer et connectez-vous au portail MIM à l’adresse *http://corpidm.contoso.local:82/identitymanagement*. Notez qu'un bref délai peut avoir lieu la première fois que vous visitez cette page.

    - Si nécessaire, authentifiez-vous en tant que *contoso\Administrator* auprès d’Internet Explorer.

2. Dans Internet Explorer, ouvrez **Options Internet**, accédez à l'onglet **Sécurité** , puis ajoutez le site à la zone **Intranet local** , s'il ne s'y trouve pas déjà.  Fermez la boîte de dialogue **Options Internet** .

3. Permettez aux utilisateurs d'afficher leur propre entrée dans MIM.

    1.  À l'aide d'Internet Explorer, dans **Portal MIM**, cliquez sur **Règles de stratégie de gestion**.

    2.  Recherchez la règle de stratégie de gestion **Gestion des utilisateurs : les utilisateurs peuvent lire leurs propres attributs**.

    3.  Sélectionnez cette règle de stratégie de gestion, décochez **La stratégie est désactivée**.

    4.  Cliquez sur **OK** , puis sur **Envoyer**.

4.  Vérifiez que le pare-feu autorise les connexions entrantes vers les ports TCP 5725 et 5726.

    1.  Lancez **Outils d’administration » Pare-feu Windows** avec **fonctions avancées de sécurité**.

    2.  Cliquez sur **Règles de trafic entrant**.

    3.  Vérifiez que les deux règles suivantes sont listées :

        -   Forefront Identity Manager Service (STS).

        -   Forefront Identity Manager Service (Webservice).

    4.  Effectuez les étapes de l'Assistant, puis fermez l'application **Pare-feu Windows** .

    5.  Lancez **Panneau de configuration » Réseau et Internet » Afficher l’état et la gestion du réseau**.

    6.  Vérifiez qu'il existe qu'un réseau actif listé en tant que réseau de domaine contoso.local.

    7.  Fermez le **Panneau de configuration**.

> [!NOTE]
> Facultatif : à ce stade, vous pouvez installer les compléments et extensions MIM.

>[!div class="step-by-step"]  
[« Service de synchronisation MIM](install-mim-sync.md)
[Synchroniser les bases de données »](install-mim-sync-ad-service.md)



<!--HONumber=Jul16_HO3-->


