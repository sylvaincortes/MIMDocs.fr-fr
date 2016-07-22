---
title: "Configurer un serveur de gestion des identités&#58; SQL Server 2014 | Microsoft Identity Manager"
description: "Installez SQL Server 2014 pour préparer votre installation MIM 2016."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c023d147d0fcc1525fefbe866c952e217f7bee6b
ms.openlocfilehash: 2c0ff0bdbba4bcf979def8d5c7aa381947d6cc87


---

# Configurer un serveur de gestion d’identité : SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Cette procédure pas à pas utilise des exemples de noms et de valeurs tirés d’une société appelée Contoso. Remplacez-les par les vôtres. Exemple :
> - Nom du contrôleur de domaine : **mimservername**
> - Nom de domaine : **contoso**
> - Mot de passe : **Pass@word1**

## Installer **SQL Server 2014 Standard Edition**

1. Lancez **PowerShell** en tant qu'administrateur de domaine.

2. Accédez au répertoire où se trouve le programme d'installation de SQL Server.

3. Tapez les commandes suivantes.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)



<!--HONumber=Jun16_HO4-->


