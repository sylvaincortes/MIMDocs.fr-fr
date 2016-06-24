---
# required metadata

title: Utilisation de la création de rapports hybrides Identity Manager | Microsoft Identity Manager
description: Découvrez comment combiner les données locales et les données du cloud dans des rapports hybrides dans Azure et comment gérer et afficher ces rapports.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Utilisation des rapports hybrides Identity Manager

## Rapports hybrides disponibles
Les trois premiers rapports Microsoft Identity Manager (MIM) disponibles dans Azure AD sont **Activité de réinitialisation de mot de passe**, **Enregistrement de réinitialisation de mot de passe** et **Activité des groupes en libre-service**.

-   Le rapport Activité de réinitialisation de mot de passe affiche chaque instance quand un utilisateur a effectué une réinitialisation de mot de passe via la réinitialisation du mot de passe libre-service. En outre, il fournit les portes ou **méthodes** utilisées pour l'authentification.

    ![Rapports hybrides Azure - Image d’activité de réinitialisation du mot de passe](media/MIM-Hybrid-passwordreset.jpg)

-   Le rapport Activité d'inscription de réinitialisation de mot de passe s'affiche chaque fois qu'un utilisateur demande la réinitialisation du mot de passe libre-service et des **méthodes** d'authentification employées, par exemple un numéro de téléphone mobile ou des questions/réponses.
    Notez que pour l'inscription de réinitialisation de mot de passe, aucune distinction n'est faite entre la porte SMS et la porte MFA. Toutes les deux sont associées à un **téléphone mobile**.

-   L’activité des groupes en libre-service affiche chacune des tentatives effectuées par une personne pour entrer ou sortir d’un groupe, ainsi que les tentatives de création d’un groupe.

> [!NOTE]
> Les rapports présentent des données dont l'antériorité peut aller jusqu'à un mois.
>
> Pour désinstaller les rapports hybrides, désinstallez l'agent MIMreportingAgent.msi.

## Conditions préalables

1.  Installez Microsoft Identity Manager 2016, ainsi que le service MIM.

2.  Assurez-vous que vous avez un locataire Azure AD Premium avec un administrateur disposant d'une licence dans votre annuaire.

3.  Assurez-vous que vous disposez d'une connectivité Internet sortante entre le serveur Microsoft Identity Manager et Azure.

## Installer la fonctionnalité de création de rapports Microsoft Identity Manager dans Azure AD
Une fois l’agent de création de rapports installé, les données d’activité de Microsoft Identity Manager sont exportées de Microsoft Identity Manager vers le journal des événements Windows. L’agent de création de rapports MIM traite les événements et les charge dans Azure. Dans Azure, les événements sont analysés, déchiffrés et filtrés pour les rapports appropriés.

1.  Installez Microsoft Identity Manager 2016.

2.  Téléchargez les agents de création de rapports Microsoft Identity Manager :

    1.  Connectez-vous au Portail de gestion Azure AD, puis cliquez sur l'icône Active Directory.

    2.  Double-cliquez sur l'annuaire dont vous êtes un administrateur général, et pour lequel vous disposez d'un abonnement Azure AD Premium.

    3.  Cliquez sur **Configuration** , puis téléchargez l'agent de création de rapports.

3.  Installez l'agent de création de rapports Microsoft Identity Manager :

    1.  Créez un annuaire sur l'ordinateur.

    2.  Extrayez les fichiers `MIMHybridReportingAgent.msi` et `tenant.cert` dans l'annuaire.

    3.  Exécutez le programme d'installation de l'agent.

    4.  Assurez-vous que le service de l’agent de création de rapports MIM est en cours d’exécution.

    5.  Redémarrez le service MIM.

4.  Vérifiez que la création de rapports Microsoft Identity Manager fonctionne dans Azure.

    Vous pouvez créer des données de rapport en utilisant le portail de réinitialisation du mot de passe libre-service de Microsoft Identity Manager pour réinitialiser le mot de passe d'un utilisateur. Assurez-vous que la réinitialisation du mot de passe s'est correctement effectuée, puis vérifiez que les données s'affichent dans le Portail de gestion Azure AD.

## Afficher les rapports hybrides dans le portail Azure Classic

1.  Connectez-vous au [portail Azure Classic](https://manage.windowsazure.com/) avec votre compte d’administrateur général du locataire.

2.  Cliquez sur l'icône **Active Directory** .

3.  Sélectionnez l'annuaire de locataire dans la liste des annuaires disponibles pour votre abonnement.

4.  Cliquez sur **Rapports** , puis sur **Activité de réinitialisation de mot de passe**.

5.  Veillez à sélectionner **Identity Manager** dans le menu déroulant de la source.

> [!WARNING]
> Il peut s'écouler un certain temps avant que les données de Microsoft Identity Manager n'apparaissent dans Azure AD.

## Arrêter la création de rapports hybrides
Pour arrêter le chargement des données de création de rapports de Microsoft Identity Manager vers Azure Active Directory, désinstallez l’agent de création de rapports hybrides. À l’aide de l’outil **Ajouter ou supprimer des programmes** de Windows, désinstallez la fonctionnalité de création de rapports hybrides Microsoft Identity Manager.

## Événements Windows utilisés pour la création de rapports hybrides
Les événements générés par Microsoft Identity Manager sont enregistrés dans le journal des événements Windows et sont consultables dans l’Observateur d’événements sous Journaux des applications et des services-&gt; **Journal des demandes Identity Manager**. Chaque demande MIM est exportée en tant qu’événement dans le journal des événements Windows au format JSON. Le contenu peut être exporté vers votre logiciel SIEM.

|Type d'événement|ID|Détails de l'événement|
|--------------|------|-----------------|
|Informations|4121|Données d’événement MIM qui incluent toutes les données de la demande.|
|Informations|4137|Extension de l’événement MIM 4121, au cas où le volume de données serait trop important pour un événement unique. L'en-tête de cet événement se présente sous la forme suivante : `"Request: <GUID> , message <xxx> out of <xxx>`|


<!--HONumber=Apr16_HO2-->


