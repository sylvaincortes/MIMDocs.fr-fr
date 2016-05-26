---
# required metadata

title: Installer MIM 2016&#58; le service de synchronisation MIM | Microsoft Identity Manager
description: Prise en main des composants MIM 2016 en installant et en configurant le service de synchronisation.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installer le service de synchronisation MIM 2016

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Service et portail MIM »](install-mim-service-portal.md)

> [!NOTE]
> Cette procédure pas à pas utilise des exemples de noms et de valeurs tirés d’une société appelée Contoso. Remplacez-les par les vôtres. Exemple :
> - Nom du contrôleur de domaine : **mimservername**
> - Nom de domaine : **contoso**
> - Mot de passe : **Pass@word1**

Pour installer les composants de Microsoft Identity Manager 2016, configurez d’abord le package d’installation.

1. Connectez-vous en tant que *contoso\Administrateur* sur le serveur que vous utilisez pour la gestion des identités.

2. Décompressez le package d'installation MIM, ou montez DVD de l'image MIM.

## Installer le service de synchronisation MIM 2016

1. Dans le dossier d'installation MIM décompressé, accédez au dossier **Service de synchronisation** .

2. Exécutez le **programme d'installation du service de synchronisation MIM**. Suivez les instructions du programme d'installation, puis terminez l'installation.

3. Dans l'écran d'accueil, cliquez sur **Suivant**.

    ![Image de Bienvenue dans l’Assistant du programme d’installation de MIM](media/MIM-Install1.png)

4. Prenez connaissance des termes du contrat de licence, et cliquez sur **Suivant** pour les accepter.

5. Dans l’écran **Installation personnalisée**, cliquez sur **Suivant**.

    ![Page Installation personnalisée](media/MIM-Install2.png)

6.  Dans l'écran de configuration de la base de données de synchronisation, sélectionnez :

    1.  Le serveur SQL Server se trouve sur : **Cet ordinateur**.

    2.  L'instance SQL Server est : **L'instance par défaut**.

    ![Image de la connexion de base de données](media/MIM-Install3.png)

7.  Configurez le compte de service de synchronisation en fonction du compte créé précédemment :

    1.  Compte de service : *MIMSync*

    2.  Mot de passe : *Pass@word1*

    3.  Domaine du compte de service ou nom de l'ordinateur local : *contoso*

    ![Image du compte de service](media/MIM-Install4.png)

8.  Indiquez au programme d'installation de la synchronisation MIM les groupes de sécurité appropriés :

    1. Administrateur = *contoso\MIMSyncAdmins*

    2. Opérateur = *contoso\MIMSyncOperators*

    3. Jointure = *contoso\MIMSyncJoiners*

    4. Connecteurs = *contoso\MIMSyncBrowse*

    5. Gestion des mots de passe WMI = *contoso\MIMSyncPasswordReset*

    ![Image des groupes de sécurité](media/MIM-Install5.png)

9. Dans l’écran des paramètres de sécurité, cochez **Activer les règles de pare-feu pour les communications RPC entrantes**, puis cliquez sur **Suivant**.

10. Cliquez sur **Installer** pour commencer l’installation de la synchronisation MIM.

    1. Un avertissement concernant le compte de service de synchronisation MIM s'affiche. Cliquez sur **OK**.

    2. La synchronisation MIM va être installée.

    3. Une notification relative à la création d’une sauvegarde pour la clé de chiffrement s’affiche : cliquez sur **OK**, puis sélectionnez un dossier où stocker la sauvegarde de la clé de chiffrement.

        ![Image de la notification de la clé de chiffrement de la sauvegarde de synchronisation MIM](media/MIM-Install7.png)

    4. Une fois l'exécution du programme d'installation achevée, cliquez sur **Terminer**.

    5. Vous devez vous déconnecter et vous reconnecter pour que les modifications de l’appartenance au groupe prennent effet. Cliquez sur **Oui** pour vous déconnecter.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Service et portail MIM »](install-mim-service-portal.md)


<!--HONumber=Apr16_HO3-->


