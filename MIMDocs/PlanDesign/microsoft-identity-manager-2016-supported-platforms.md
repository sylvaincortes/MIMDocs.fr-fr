---
title: Plateformes logicielles prises en charge | Microsoft Identity Manager
description: "Découvrez les produits et les versions compatibles avec les composants MIM 2016"
keywords: 
author: kgremban
manager: femila
ms.date: 09/29/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 69e2c327cf897dea450c43618a9b4ce3ab555cc0
ms.openlocfilehash: 522e9321d7709a7967cfea3eb1cea809dfe8202e


---

# Plateformes prises en charge pour MIM 2016

Ce tableau décrit les plateformes prises en charge et la version de chaque composant de Microsoft Identity Manager 2016. Les versions marquées d’un astérisque (*) sont uniquement prises en charge par MIM 2016 Service Pack 1.


| **Composant MIM** | **Plate-forme** | **Version** |
|-------------------|--------------|-------------|
| **Synchronisation MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
|| | Base de données de synchronisation MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | Active Directory pour l’attribution d’utilisateurs, PCNS et la synchronisation de liste d’adresses globale (facultatif)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Exchange pour l’approvisionnement de boîtes aux lettres et la synchronisation de liste d’adresses globale (facultatif)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Environnement de développement (facultatif) | Visual Studio 2012<br/>Visual Studio 2013 |
|| | Système connecté supplémentaire (facultatif) | Services de domaine Active Directory<br/>Active Directory<br/>Services LDS (Lightweight Directory Services)<br/>SQL Server 2000 ou version ultérieure<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Autres produits tiers |
| **Service MIM** (à l’exception du scénario PAM) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Base de données du service MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | Exchange pour les messages électroniques d’approbation et de gestion de groupes du service MIM (facultatif) | Exchange Server 2007 SP3 (avec la console de gestion Exchange installée)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
| **Service et portail MIM** (scénario PAM uniquement)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Active Directory pour la forêt PAM d’environnement bastion | Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Active Directory pour les forêts existantes | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
|| | Base de données du service MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
|| | Serveur de messagerie pour les messages électroniques d’approbation et de gestion de groupes du service MIM (facultatif) | Exchange Server 2007 SP3 (avec la console de gestion Exchange installée)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
|| | Navigateur | Tous les principaux navigateurs |
| **Rapports du service MIM** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
|| | Entrepôt de données | System Center 2012 Service Manager SP1 |
|| | Base de données d’entrepôt de données | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **Portails de réinitialisation de mot de passe et d’inscription MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Navigateur web | Tous les principaux navigateurs |
| **Compléments et extensions MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
|| | Intégration d’Outlook (facultatif) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (sous Windows 10) * |
|| | Applets de commande du demandeur PowerShell PAM | Windows 8.1<br/>Windows 10 |
| **Gestion des certificats MIM** (intégration du serveur et de l’autorité de certification) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| | Autorité de certification | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| | Base de données MIM CM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
| **Gestion des certificats MIM** (Application) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Gestion des certificats MIM** (client et client en bloc) | Windows | Windows 7 |
| **Suite BHOLD MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| | Base de données BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * |
|| | Serveur de messagerie (facultatif) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Navigateur web | Internet Explorer 7, 8, 9, 10 ou 11 avec Silverlight |



<!--HONumber=Sep16_HO5-->


