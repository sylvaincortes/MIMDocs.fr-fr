---
# required metadata

title: Configurer un serveur de gestion des identités&#58; SQL Server 2014 | Microsoft Identity Manager
description: Installez SQL Server 2014 pour préparer votre installation MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurer un serveur de gestion d’identité : SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Dans tous les exemples ci-dessous, **mimservername** représente le nom de votre contrôleur de domaine, **contoso** représente votre nom de domaine, et **Pass@word1** représente un exemple de mot de passe.

## Installer **SQL Server 2014 Standard Edition**

1. Lancez **PowerShell** en tant qu'administrateur de domaine.

2. Accédez au répertoire où se trouve le programme d'installation de SQL Server.

3. Tapez les commandes suivantes.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)


<!--HONumber=Apr16_HO2-->


