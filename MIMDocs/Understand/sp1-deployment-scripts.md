---
title: "Scripts de déploiement MIM2016 SP1 PAM"
description: "Préparez le domaine CORP avec des identités nouvelles ou existantes à gérer avec Privileged Identity Manager à l’aide de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# Scripts de déploiement MIM2016 SP1 PAM

Dans ce Service Pack, nous vous présentons un ensemble de scripts de déploiement conçus pour faciliter le déploiement de PAM. Ces scripts sont disponibles sur le centre de téléchargement. Avant d’essayer d’utiliser ces scripts, il est important de vous assurer que les hypothèses ci-dessous s’appliquent à votre environnement.

Hypothèses importantes :
1. Le système d’exploitation sur tous les ordinateurs est au moins Windows Server 2012 R2. Si vous testez Windows Server 2016 Technical Preview 5, le contrôleur de domaine PRIV doit être installé avec la version TP5.
2. DNS doit être configuré de façon à ce que la résolution de noms entre vos contrôleurs de domaine et les serveurs de composants soit automatique.
3. Les fichiers binaires d’installation doivent être disponibles localement sur les serveurs désignés pour l’installation de SQL, SharePoint et MIM.
4. L’environnement comporte trois ordinateurs dédiés (physiques ou virtuels) exécutant indépendamment CORPDC, PRIVDC et PAMSERVER.
5. Pour l’option de validation, il est supposé qu’un ordinateur client dédié est disponible pour exécuter cette étape.

>[!NOTE] Si vous rencontrez des problèmes avec l’exécution du script, vous devrez peut-être consulter les journaux. Tous les journaux de script sont enregistrés sous %AppData%\MIMPAMInstall. Veuillez compresser le dossier dans un fichier Zip et l’envoyer, ainsi que les détails de l’opération et de l’erreur, par courrier électronique à mim2016@microsoft.com.



<!--HONumber=Sep16_HO4-->


