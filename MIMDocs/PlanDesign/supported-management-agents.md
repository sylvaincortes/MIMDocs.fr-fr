---
title: Connecteurs pris en charge | Microsoft Identity Manager
description: "Utilisez des connecteurs pour gérer le transfert de données entre MIM et vos annuaires."
keywords: 
author: kgremban
manager: femila
ms.date: 08/11/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 309011c81959971e696d70aa4ec5e1610cc8a2f0
ms.openlocfilehash: f0842781e3730dae5548ce02a3cb247376d12dc8


---

# Se connecter aux annuaires

Les connecteurs lient des sources de données connectées spécifiques à Microsoft Identity Manager (MIM). Un connecteur déplace des données d’une source de données connectée vers MIM. Quand des données sont modifiées dans MIM, le connecteur peut également exporter les données vers la source de données connectée pour qu’elles soient synchronisées avec MIM. Généralement, il existe au moins un connecteur par annuaire connecté.

Dans Forefront Identity Manager, les connecteurs étaient appelés « agents de gestion ». Ce terme est toujours utilisé dans certains articles ou parties du produit, mais sachez que les deux termes font référence au même concept.

Cet article décrit les connecteurs inclus dans MIM, mais le connecteur Extensible Connectivity 2.0 vous permet de vous connecter à encore plus de sources de données. Certains partenaires ont créé leurs propres connecteurs de cette façon. Une liste complète est disponible dans la section Wiki [FIM 2010 : Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010 : agents de gestion des partenaires).

## Connecteurs pris en charge dans MIM 2016

| Nom | Versions prises en charge de la source de données connectée |
| ---- | ----------------------------------------------- |
| Services de domaine Active Directory | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Services ADLDS (Active Directory Lightweight Directory Services) | Services ADLDS (Active Directory Lightweight Directory Services) |
| Liste d’adresses globale (LAG) Active Directory | Liste d’adresses globale (LAG) Active Directory – Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Toute source de données basée sur un appel ou un fichier |
| Service MIM | Microsoft Identity Manager 2016 |
| Base de données universelle IBM DB2 | IBM DB2 version 9.1, 9.5 ou 9.7 ; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 et 8.8.6 |
| Base de données Oracle | Oracle Database 10g ou 11g ; client 64 bits |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Serveurs d’annuaire Oracle (anciennement Sun et Netscape) | Sun Directory Server 6.x, 7.x et Oracle 11 |
| [Connecteur Windows PowerShell pour FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 ou version ultérieure |
| [Connecteur Microsoft Azure Active Directory pour FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Connecteur LDAP générique pour FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Serveur LDAP v3 (conforme à la spécification RFC 4510) |
| [Connecteur pour Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes version 8.0.x ou 8.5.x |
| [Connecteur SharePoint Services pour FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013 ou 2016 avec application de service de profil utilisateur |
| [Connecteur pour les services web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 ou 6.0 ; Oracle PeopleSoft 9.1 ; Oracle eBusiness 12.1 |
| Fichier texte de paires attribut-valeur | Fichiers texte de paires attribut-valeur |
| Fichier texte délimité | Fichiers texte délimités |
| Directory Services Mark-up Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Fichier texte de largeur fixe | Fichiers texte de largeur fixe |
| LDAP Data Interchange Format (LDIF) | LDAP Data Interchange Format (LDIF) |

## Rubriques connexes

[Agents de gestion dans FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)



<!--HONumber=Aug16_HO2-->


