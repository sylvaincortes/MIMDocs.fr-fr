---
title: "Mise à niveau depuis Forefront Identity Manager 2010 R2 | Microsoft Identity Manager"
description: "Découvrez comment mettre à niveau vos composants FIM 2010 R2 et installer les composants qui sont nouveaux dans MIM 2016."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 7e61e201b277a2e8ec9fee785e9e34fca3b1cb29
ms.openlocfilehash: 24a7bf5bfb0a7450becd08be6743ed7ab1755559


---

# Mise à niveau depuis Forefront Identity Manager 2010 R2

Si vous avez un environnement Forefront Identity Manager (FIM) 2010 R2 et que vous souhaitez essayer Microsoft Identity Manager (MIM) 2016, utilisez cet article comme guide. Cette mise à niveau comporte trois phases :

1.  Installer le service de synchronisation MIM 2016 (Sync) sur un serveur joint à votre domaine Active Directory (AD). Cela remplace l’instance FIM 2010 R2 du service de synchronisation.

2.  Installer le service et le portail MIM. À ce stade, vous pouvez aussi choisir d’installer le portail d’enregistrement Réinitialisation du mot de passe libre-service et le portail de service. L’installation sera effectuée, à l’exclusion du jeu de fonctionnalités Privileged Access Management.

3.  Déployer les compléments et les extensions MIM sur un ordinateur client distinct. Cela comprend le client intégré de connexion Windows SSPR.


Ce guide part du principe que vous avez déjà configuré ce qui suit :
- FIM 2010 R2 déployé dans un environnement de test
- Serveurs exécutant Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2008 R2
- Prérequis locaux et environnementaux (SQL Server, Exchange Server, SharePoint Services, etc.) configurés pour FIM 2010 R2


## Préparation

1.  Sauvegardez votre base de données de service FIM, votre base de données de synchronisation FIM, ainsi que votre logiciel et configuration de service et de synchronisation FIM.

2.  Sur chaque serveur où sont installés les composants FIM 2010 R2, par exemple *CORPIDM*, connectez-vous en tant que Contoso\Administrateur. Dans cet exemple de déploiement, des droits d'administration sont nécessaires pour mettre à niveau FIM 2010 R2 vers **MIM**.

3.  Téléchargez ou décompressez le logiciel MIM.

## Mise à niveau du service de synchronisation

1.  Connectez-vous en tant qu'administrateur sur un serveur où le service de synchronisation FIM 2010 R2 (« Sync ») est déployé.

2.  Veillez à sauvegarder votre base de données avant de commencer cette procédure.

3.  Ouvrez la console **Services** , recherchez **Service de synchronisation de Forefront Identity Manager**et arrêtez-le.

    ![Image de la console des services](media/MIM-UpgFIM1.PNG)

4.  Exécutez le **programme d'installation du service de synchronisation MIM**. Le programme d'installation détectera la version existante du service de synchronisation et proposera une mise à niveau. Cliquez sur le bouton **Mettre à jour** pour continuer.

5.  Si vous acceptez les termes du contrat de licence, cliquez sur **Suivant** pour continuer.

6.  Entrez le mot de passe du compte de service utilisé par le service de synchronisation, puis cliquez sur **Suivant**.

    ![Image de la configuration du compte de service de synchronisation MIM](media/MIM-UpgFIM3.png)

7.  Vérifiez que les noms des groupes de sécurité sont corrects, puis cliquez sur **Suivant**.

    ![Image de la configuration des groupes de sécurité de la synchronisation MIM](media/MIM-UpgFIM4.png)

8.  Laissez inchangée la case à cocher correspondant aux règles de pare-feu pour les communications RPC entrantes.

9. Le programme d'installation est prêt à mettre à niveau le service de synchronisation FIM 2010 R2 vers MIM. Cliquez sur **Mettre à niveau** pour démarrer le processus de mise à niveau.

10. La mise à niveau est maintenant en cours. Ne quittez pas le programme d'installation et ne redémarrez pas l'ordinateur pendant que la mise à niveau est en cours.

    ![Image d’état de l’installation de la synchronisation MIM](media/MIM-UpgFIM7.png)

11. Durant celle-ci, un avertissement concernant la mise à niveau de la base de données de synchronisation est affiché. Nous vous recommandons de sauvegarder la base de données avant de lancer la mise à niveau.

12. Une fois la mise à niveau terminée, cliquez sur **Terminer**.

    ![Image d’installation réussie de la synchronisation MIM](media/MIM-UpgSP1.png)

13. Notez que le **Service de synchronisation** a redémarré.

## Mettre à niveau le service et le portail

1.  Connectez-vous en tant qu'administrateur sur un serveur où le service et le portail FIM 2010 R2 sont déployés.

2.  Ouvrez la console **Services** , recherchez **Forefront Identity Manager Service**et arrêtez-le.

    ![Image de la console des services](media/MIM-UpgFIM9.PNG)

3.  Exécutez le programme d'installation du service et du portail MIM. Cliquez sur le bouton **Installer** pour continuer.

    ![Image de l’installation du service et du portail MIM](media/MIM-UpgSP2.png)

4.  Si vous acceptez les termes du contrat de licence, cliquez sur **Suivant** pour continuer.

5.  Dans l'écran Programme d'amélioration du produit MIM, cliquez sur **Suivant** pour continuer.

6.  Sélectionnez les fonctionnalités et les composants MIM que vous souhaitez installer, puis cliquez sur **Suivant**.

    ![Page Installation personnalisée](media/MIM-UpgSP4.png)

    1.  **Service MIM** : cette fonctionnalité est obligatoire sur au moins un serveur et nécessite un serveur de bases de données SQL Server colocalisé ou sur un autre serveur.

    2.  **Portail MIM :** cette fonctionnalité est obligatoire sur au moins un serveur et nécessite SharePoint 2013 Foundation.

    3.  **Portail d’enregistrement du mot de passe MIM** : cette fonctionnalité est nécessaire pour la réinitialisation du mot de passe en libre-service.

    4.  **Portail de réinitialisation de mot de passe MIM :** cette fonctionnalité est nécessaire pour la réinitialisation de mot de passe.

7.  Fournissez les détails de l'ordinateur SQL Server utilisé pour la base de données du service FIM. Sélectionnez l'option pour réutiliser la base de données existante et conserver les données. Cliquez sur **Suivant** pour continuer.

8. Si l'option de réutilisation de la base de données existante a été sélectionnée, un rappel vous invitant à sauvegarder la base de données s'affiche.

9. Entrez les détails du serveur de messagerie. Si le serveur de messagerie se trouve sur le serveur actif, entrez « localhost » comme emplacement de serveur de messagerie. Cliquez sur **Suivant** pour continuer.

    ![Image de la configuration de la connexion au serveur de messagerie](media/MIM-UpgSP6.png)

10. Sélectionnez le certificat que le service doit utiliser pour valider les clients. Vous devez utiliser le certificat existant à partir du magasin de certificats local qui était utilisé précédemment par le service FIM.

    ![Configurer l’image de certificat de service](media/MIM-UpgSP7.png)

    1.  Si vous sélectionnez l’option de magasin de certificats local, cliquez sur le bouton **Sélectionner le certificat** et sélectionnez un certificat dans la liste dans la fenêtre contextuelle. Cliquez sur **OK**, puis sur **Suivant**.

        ![Image de la fenêtre contextuelle de sélection du certificat](media/MIM-UpgSP8.PNG)

11. Configurez les informations d'identification du compte de service pour le service MIM. Notez que le compte de service ne peut pas être identique au compte de service utilisé par le service de synchronisation. Il doit s'agir du même compte que celui qui était utilisé par le service FIM.

    ![Image de la configuration du compte de service](media/MIM-UpgSP9.png)

12. Configurez les détails du serveur de synchronisation MIM conformément au déploiement du service MIM que vous avez configuré à l’une des étapes précédentes.

    ![Image de la configuration du service MIM et de la synchronisation du portail](media/MIM-UpgSP10.png)

13. Lors de l'installation du portail MIM, entrez l'adresse du serveur du service MIM. Cliquez sur **Suivant**.

14. Lors de l'installation du portail MIM, fournissez l'URL de la collection de sites SharePoint dans laquelle le portail FIM est actuellement hébergé. Cliquez sur **Suivant**.

## Installer le portail d’enregistrement du mot de passe MIM

1. Si vous installez le Portail d'enregistrement du mot de passe MIM, indiquez l'URL demandée pour le Portail d'enregistrement du mot de passe. Cliquez sur **Suivant**.

2. Configurez la capacité des clients et des utilisateurs à utiliser le service et le portail.

    1.  Choisissez si vous souhaitez **Ouvrir les ports 5725 et 5726 dans le pare-feu**.

    2.  Choisissez si vous souhaitez **Accorder aux utilisateurs authentifiés l’accès au site du portail MIM**.

    3.  Cliquez sur **Suivant**.

3. Fournir les informations d’accès et les informations d’identification pour l’enregistrement du mot de passe MIM.

    ![Image de la configuration du portail d’enregistrement du mot de passe MIM](media/MIM-UpgSP15.png)

    1.  Fournissez le nom du compte de service (y compris le domaine) et le mot de passe pour l'enregistrement du mot de passe MIM.

    2.  Fournissez les détails de l'hôte – nom et port (par exemple, 8080) – du portail d'enregistrement du mot de passe.

    3.  Sélectionnez l'option **Ouvrir le port dans le pare-feu** .

    4.  Cliquez sur **Suivant**.

4. Dans l'écran de configuration de l'enregistrement du mot de passe MIM suivant :

    1.  Indiquez à l'enregistrement du mot de passe MIM où est installé le service MIM (généralement sur le même système).

    2.  Déterminez si ce portail est accessible par les utilisateurs de l'extranet et de l'intranet, ou uniquement par les utilisateurs de l'intranet, comme précédemment configuré pour la réinitialisation du mot de passe FIM.

## Installer le portail de réinitialisation du mot de passe MIM

1. Si vous installez le Portail de réinitialisation de mot de passe MIM, fournissez les détails d'accès et les informations d'identification pour la réinitialisation de mot de passe MIM.

    ![Image de configuration du portail de réinitialisation du mot de passe MIM](media/MIM-UpgSP17.png)

    1.  Fournissez le nom du compte de service (y compris le domaine) et le mot de passe pour la réinitialisation de mot de passe MIM.

    2.  Fournissez les détails de l'hôte – nom et port (par exemple, 8088) – du portail de réinitialisation de mot de passe.

    3.  Sélectionnez l'option **Ouvrir le port dans le pare-feu** .

    4.  Cliquez sur **Suivant**.

2. Dans l'écran de configuration de réinitialisation de mot de passe MIM suivant :

    1.  Indiquez à la réinitialisation de mot de passe MIM où est installé le service MIM.

    2.  Spécifiez si ce portail est accessible aux utilisateurs de l'extranet et de l'intranet, ou uniquement aux utilisateurs de l'intranet.

## Terminer l’installation et la mise à niveau

1. Une fois que vous avez effectué toutes les définitions de configuration, une page d’installation s’affiche. Cliquez sur **Installer** pour commencer l'installation et la mise à niveau du service et du portail MIM.

2. L'installation et la mise à niveau du service et du portail MIM sont maintenant en cours. N'annulez pas le programme d'installation et ne redémarrez pas l'ordinateur pendant l'installation.

3. Une fois l’installation (la mise à niveau) du service et du portail MIM effectuée, l’écran suivant s’affiche. Cliquez sur **Terminer** pour terminer l'installation de quitter le programme d'installation.

4. Notez que **Forefront Identity Manager Service** a redémarré.

Remarque : si les compléments et les extensions FIM actuellement déployés sur les ordinateurs de l’utilisateur pour la réinitialisation de mot de passe libre-service, ne configurez pas les nouvelles portes de téléphone MFA pour la réinitialisation du mot de passe jusqu’à ce que tous les compléments et extensions FIM aient été mis à niveau vers MIM 2016.  Comme les compléments et extensions de FIM 2010 et FIM 2010 R2 ne reconnaissent pas les nouvelles portes, une erreur s’affiche et l’utilisateur ne peut pas effectuer la réinitialisation du mot de passe.



<!--HONumber=Jun16_HO4-->


