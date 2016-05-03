---
# required metadata

title: Configurer un serveur de gestion des identités&#58; Exchange | Microsoft Identity Manager
description: Une étape facultative consiste à déployer Exchange Server pour permettre à MIM 2016 d’envoyer des courriers électroniques et de créer des boîtes aux lettres. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 34a8c16e-3bed-4e16-939b-b9fe17dd834b

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurer un serveur de gestion d’identité : Exchange

>[!div class="step-by-step"]
[« SharePoint](prepare-server-sharepoint.md)
[Service de synchronisation MIM »](install-mim-sync.md)

> [!NOTE]
> Dans tous les exemples ci-dessous, **mimservername** représente le nom de votre contrôleur de domaine, **contoso** représente votre nom de domaine, et **Pass@word1** représente un exemple de mot de passe.

## déployer Microsoft Exchange Server
Si vous souhaitez configurer MIM pour envoyer et recevoir du courrier électronique ou approvisionner des boîtes aux lettres, Exchange doit être présent dans l'environnement. Si vous n'avez pas encore déployé Exchange, vous pouvez installer une version d'évaluation.

1. Télécharger et installer Microsoft Office 2010 Filter Packs - Version 2.0 + Microsoft Office 2010 Filter Packs - Version 2.0 SP1

    - [MS Office10 FP2.0](http://www.microsoft.com/en-us/download/details.aspx?id=17062)

    - [MS Office10 FP2.0 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=26604)

2. Télécharger et installer [Microsoft Unified Communications Managed API 4.0, Core Runtime 64 bits](http://www.microsoft.com/en-us/download/details.aspx?id=34992)

3. Télécharger et installer la [version d'évaluation de 180 jours de MS Exchange Server 2013](http://www.microsoft.com/en-us/evalcenter/evaluate-exchange-server-2013)

>[!div class="step-by-step"]  
[« SharePoint](prepare-server-sharepoint.md)
[Service de synchronisation MIM »](install-mim-sync.md)


<!--HONumber=Apr16_HO2-->


