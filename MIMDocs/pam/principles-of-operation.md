---
title: "Présentation des composants de PAM | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 49f47050703095d402a1514342baf4e928f66c70


---

# Présentation des composants de PAM

Privileged Access Management isole l’accès administratif des comptes d’utilisateur ordinaires. Cette solution s’appuie sur des forêts parallèles :

- *CORP* : forêt d’entreprise à usage général qui inclut un ou plusieurs domaines. Même si vous pouvez avoir plusieurs forêts CORP, les exemples de ces articles, par souci de simplicité, partent du principe qu’il y a une forêt unique avec un domaine unique.  
- *PRIV* : forêt dédiée créée spécialement pour ce scénario PAM. Cette forêt comprend un domaine qui accueille des groupes et des comptes privilégiés, masqués par rapport à un ou plusieurs domaines CORP.

La solution MIM configurée pour PAM inclut les composants suivants :  

- **Service MIM** : implémente la logique métier permettant d’effectuer des opérations de gestion des identités et des accès, notamment la gestion des comptes privilégiés et le traitement des demandes d’élévation.   
- **Portail MIM** : portail SharePoint hébergé par SharePoint 2013, qui fournit une interface utilisateur de gestion et de configuration pour les administrateurs.
- **Base de données du service MIM** : stockée dans SQL Server 2012 ou 2014, elle contient les données et métadonnées d’identité nécessaires au service MIM.
- **Service de surveillance PAM** et **Service de composants PAM** : deux services qui gèrent le cycle de vie des comptes privilégiés, et qui aident PRIV AD à prendre en charge le cycle de vie de l’appartenance aux groupes.
- **Applets de commande PowerShell** : permettent d’indiquer au service MIM et à PRIV AD les utilisateurs et groupes correspondant aux utilisateurs et groupes de la forêt CORP pour les administrateurs PAM. Elles permettent également aux utilisateurs finals qui effectuent des demandes de type « juste-à-temps » d’accéder aux privilèges d’un compte d’administration.
- **API REST PAM et exemple de portail** : pour les développeurs qui intègrent MIM dans le scénario PAM avec des clients personnalisés à des fins d’élévation, sans devoir utiliser PowerShell ou SOAP. L'utilisation de l'API REST est illustrée par un exemple d'application web.

Une fois installé et configuré, chaque groupe créé par la procédure de migration dans la forêt PRIV est un groupe de sécurité SIDHistory intermédiaire (« shadow »), également appelé groupe principal étranger dans la mise à jour basée sur Windows Server vNext. Ce groupe de sécurité effectue une mise en miroir du groupe SID dans la forêt CORP d’origine. Par ailleurs, quand le service MIM ajoute des membres à ces groupes dans la forêt PRIV, ces appartenances ont une durée limitée.

Ainsi, quand un utilisateur demande une élévation à l'aide des applets de commande PowerShell, si sa demande est approuvée, le service MIM ajoute son compte de la forêt PRIV à un groupe de la forêt PRIV. Quand l'utilisateur se connecte avec son compte privilégié, son jeton Kerberos contient un identificateur de sécurité (SID) identique au SID du groupe dans la forêt CORP. Dans la mesure où la forêt CORP est configurée pour approuver la forêt PRIV, le compte avec élévation de privilèges, utilisé pour accéder à une ressource de la forêt CORP, apparaît pour une ressource vérifiant l'appartenance à un groupe Kerberos comme un membre des groupes de sécurité de cette ressource. Cela est dû à l'authentification inter-forêts Kerberos.

En outre, ces appartenances ont une durée limitée. Ainsi, après un délai préconfiguré, le compte d'administration de l'utilisateur cesse de faire partie du groupe dans la forêt PRIV. Ce compte ne permet donc plus d'accéder à des ressources supplémentaires.



<!--HONumber=Jun16_HO3-->


