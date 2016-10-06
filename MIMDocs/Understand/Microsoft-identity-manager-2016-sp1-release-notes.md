---
title: "Microsoft Identity Manager 2016 | Microsoft Identity Manager"
description: "Découvrez le fonctionnement de MIM 2016 pour créer une expérience de gestion d’identité plus sûre et plus pratique dans le cloud et sur site."
keywords: 
author: barclayn
manager: mbaldwin
ms.date: 09/28/2016
ms.topic: article
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 94813519554652a5554af914611d06b8a4d96ea4
ms.openlocfilehash: b791b18fa3775295e9c199086aa11a0d6c6a55e7


---
# Nouveautés de Microsoft Identity Manager 2016 Service Pack 1 #

Dans le cadre des parutions régulières pour la maintenance et la mise à jour de Microsoft Identity Manager, nous avons le plaisir d’annoncer [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). Ce document décrit les mises à jour, les améliorations, les fonctionnalités et les modifications incluses dans cette version.

Si vous rencontrez des problèmes lors d’un déploiement de production de MIM SP1, veuillez contacter le support technique Microsoft.

Nous souhaitons également recueillir vos commentaires ! Si vous avez des commentaires ou des problèmes à adresser à l’équipe produit, envoyez-nous un courrier électronique à [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## Mises à jour dans ce Service Pack #

### MIM

- **Compatibilité multi-navigateur du portail MIM pour l’utilisateur final en libre-service :** dans ce Service Pack, nous intégrons la prise en charge de la plupart des navigateurs principaux. Les utilisateurs peuvent désormais accéder au portail MIM et interagir avec celui-ci pour la gestion des groupes et des profils en libre-service à partir des navigateurs Edge, Chrome et Safari.

- **Prise en charge du service MIM pour Exchange Online :** le service MIM prend depuis longtemps en charge l’envoi et la réception de messages électroniques pour les approbations et les notifications. Avant SP1, MIM ne prenait en charge que Exchange Server ou SMTP. Avec le Service Pack 1, le service MIM peut envoyer et recevoir des demandes, ainsi que des notifications par courrier électronique à l’aide d’un compte en ligne Office 365 Exchange.

- **Validation du format de fichier d’image lors du téléchargement :** MIM est désormais en mesure de valider le format de fichier des images lorsque celles-ci sont téléchargées sur le portail.

### PAM (Privileged Access Management)

- **Prise en charge de la forêt PAM « PRIV » (bastion) pour le niveau fonctionnel Windows Server 2016 :** le service MIM PAM peut être configuré dans un environnement avec des contrôleurs de domaine exécutés au niveau fonctionnel de la forêt des services de domaine Active Directory de Windows Server 2016. Lors de la configuration, le ticket Kerberos d’un utilisateur sera limité à la période restante de l’activation de son rôle.

    >[!Note]
    Si vous choisissez de conserver le niveau fonctionnel de forêt de Windows Server 2012 R2 dans votre domaine CORP, nous vous recommandons d’installer [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) et [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) sur le contrôleur de domaine CORP.

- **Élévation de compte privilégiée dans les groupes exclusive à la forêt « PRIV » (bastion) :** désormais, les administrateurs peuvent indiquer au service MIM les groupes et les utilisateurs exclusifs à la forêt « PRIV ». Ceci permet d’inclure ces utilisateurs et groupes dans les rôles PAM.  Ils peuvent ensuite être activés pour un rôle et se voir attribuer une appartenance à des groupes dans la forêt « PRIV ».

- **Scripts de déploiement PAM :** les scripts de déploiement PAM permettent aux administrateurs de simplifier l’installation de l’environnement PAM.

- **Applets de commande PAM pour la configuration du silo de stratégies d’authentification :** le Service Pack 1 introduit de nouvelles applets de commande pour renforcer la sécurité de votre forêt bastion. Ces applets de commande créent automatiquement un silo de stratégies d’authentification, lié à un modèle de stratégie d’authentification.

    >[!Note]
    Ces applets de commande s’exécutent automatiquement dans le cadre des scripts de déploiement.


## Prise en charge de plates-formes
Vous trouverez des informations mises à jour sur la prise en charge de la plateforme dans le document appelé [Plateformes prises en charge pour MIM 2016](/microsoft-identity-manager/plan-design/microsoft-identity-manager-2016-supported-platforms).  Les nouvelles plateformes prises en charge dans ce Service Pack incluent SQL Server 2016, SharePoint 2016

## Problèmes résolus dans cette version depuis la mise à disposition générale de MIM 2016

### PAM
- New-PAMGroup ne créait pas d’objets MIM pour les groupes de domaine local dans la forêt PRIV
- New-PAMDomainConfiguration échouait avec un message d’erreur « netdom »
- Le service d’analyse PAM consignait des avertissements pour les groupes dans la forêt PRIV

## Procédure de mise à niveau vers le Service Pack 1

Les clients qui mettent à niveau vers Microsoft Identity Manager 2016 Service Pack 1 doivent respecter les instructions suivantes pour tous les services applicables à leur déploiement.

>[!Note] Les clients exécutant Forefront Identity Manager 2010 R2 SP1 ou une version antérieure doivent d’abord mettre à niveau leur environnement vers Microsoft Identity Manager 2016 publié en août 2015, puis suivre les étapes ci-dessous.

Avant de commencer

Vous devez mettre à niveau le moteur de synchronisation MIM avant de mettre à niveau le service et le portail MIM.
Vous devez sauvegarder les bases de données MIMService et MIM Sync.

  1. Désinstallez le composant Microsoft Identity Manager que vous mettez à niveau
  2. Une fois la désinstallation terminée, ouvrez la page de démarrage située sur votre support d’installation « FIMSplash.htm »
  3. Sélectionnez le composant MIM à mettre à niveau
  4. Poursuivez l’installation en suivant les invites
    * Installation du service et du portail MIM : lorsque vous choisissez Exchange Online en tant que compte de messagerie, entrez l’adresse de messagerie et les informations d’identification du compte Exchange Online dans l’écran suivant.



<!--HONumber=Sep16_HO4-->


