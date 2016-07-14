---
title: "Configuration matérielle et logicielle requise | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

Aucune configuration matérielle requise au-delà de celle des plateformes logicielles sous-jacentes. Un espace mémoire ou disque suffisant et une connectivité réseau sont nécessaires. Cet article fournit la configuration minimale requise pour un déploiement de base. Il n’a pas pour but d’illustrer les performances, l’extensibilité ou la haute disponibilité, et il ne représente pas une topologie de déploiement recommandée pour les grandes entreprises ou les grands environnements de production.

## Installation à partir des packages logiciels

Les logiciels suivants peuvent être téléchargés à partir du centre d'évaluation TechNet ou MSDN :  
- Microsoft Identity Manager 2016
  - Service et portail : contient le programme d'installation du service MIM, du portail MIM et du scénario PAM
  - Compléments et extensions : contient le programme d'installation des applets de commande PowerShell de demande

Vous pouvez télécharger les logiciels suivants à partir de GitHub :  
- PAMSamplePortal : contient l'exemple d'application web pour l'API REST

## Logiciel requis

- Windows Server 2012 R2  
- Windows 8.1 Entreprise ou Windows 10 Entreprise  
- SQL Server 2012 Service Pack 1 ou SQL Server 2014  

## Logiciel d'évaluation

Si vous n’avez pas de licences pour Windows, SQL Server ou Windows Server, vous pouvez télécharger des versions d’évaluation.

### Centre d'évaluation TechNet

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8,1 Entreprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Entreprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Centre de téléchargement Microsoft

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 et ses composants requis](https://www.microsoft.com/download/details.aspx?id=42039)

## Configuration matérielle requise

Pour chaque composant de PAM, consultez la configuration minimale requise des produits logiciels.

Pour CORPDC :  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) ou version ultérieure

Pour CORPWKSTN :  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Pour PRIVDC :  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Pour PAMSRV :
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) ou [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jul16_HO2-->


