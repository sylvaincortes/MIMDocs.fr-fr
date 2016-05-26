---
# required metadata

title: Déployer MIM 2016 | Gestionnaire d’identité Microsoft
description: Liste complète des étapes de déploiement de Microsoft Identity Manager 2016, de la préparation de l’environnement à la configuration des portails.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Déployer MIM 2016
Les articles de cette section fournissent des instructions détaillées pour déployer Microsoft Identity Manager (MIM) 2016 dans les scénarios de libre-service pour utilisateur final sur un nouveau serveur sur lequel FIM ou MIM n’ont pas été déployés précédemment.

> [!NOTE]
> La topologie de déploiement décrite dans cette section est destinée uniquement à la prise en main et à l'apprentissage de MIM.  Le [guide de planification de la capacité](/microsoft-identity-manager/plan-design/capacity-planning-guide) fournit plus d’informations sur les topologies des déploiements de production.  Nous vous recommandons de consulter cette documentation avant de déployer MIM dans un environnement de production.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## D’abord, préparez un domaine
MIM fonctionne avec Active Directory (AD). Par conséquent, suivez ces étapes pour configurer votre contrôleur de domaine Active Directory.
- [Configuration du domaine](preparing-domain.md)

## Ensuite, préparez un serveur de gestion d’identité
Une fois votre domaine en place et configuré, préparez votre serveur de gestion d’identité d’entreprise. La configuration comprend :
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (facultatif)

## Enfin, installation des composants de Microsoft Identity Manager 2016.
Une fois que vous avez défini le domaine et le serveur, vous êtes prêt à installer les composants MIM et à les configurer pour la synchronisation avec Active Directory.
- [Service de synchronisation MIM](install-mim-sync.md)
- [Service et portail MIM](install-mim-service-portal.md)
- [Synchroniser les bases de données Active Directory et de service MIM](install-mim-sync-ad-service.md)


<!--HONumber=Apr16_HO4-->


