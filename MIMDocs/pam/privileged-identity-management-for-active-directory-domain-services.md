---
title: "Qu’est-ce que PAM pour les services AD DS ? | Microsoft Identity Manager"
description: "Découvrez Privileged Access Management et comment il peut vous aider à gérer et protéger votre environnement Active Directory."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: bbc5c6760bc035d57f9d76d102246abbfe298e8b


---

# Privileged Access Management pour les services de domaine Active Directory
Privileged Access Management (PAM) est une solution basée sur Microsoft Identity Manager (MIM), Windows Server 2012 R2 et Windows Server Technical Preview. Elle permet aux organisations de restreindre l'accès privilégié au sein d'un environnement Active Directory existant.

> [!NOTE]
> PAM est une instance de [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) implémentée à l’aide de Microsoft Identity Manager (MIM).

Privileged Access Management remplit deux objectifs :

- Rétablissement du contrôle d’un environnement Active Directory compromis en conservant un environnement bastion distinct connu pour être non affecté par des attaques malveillantes.  
- Isolement de l’utilisation des comptes privilégiés pour réduire le risque de vol de ces informations d’identification.

## Quels sont les problèmes que PAM aide à résoudre ?
L’un des plus graves problèmes auxquels font face les entreprises aujourd’hui est l’accès aux ressources dans un environnement Active Directory. Les découvertes de vulnérabilités, d’escalades de privilèges non autorisées et d’autres types d’accès non autorisé (y compris les agressions de type « pass-the-hash », « pass-the-ticket », « spear phishing » et Kerberos) sont particulièrement perturbantes.

Aujourd'hui, il est trop facile pour les pirates d'obtenir des informations d'identification de compte Admins du domaine et il est trop difficile de détecter ces attaques après coup. L’objectif de PAM est de réduire les opportunités pour les utilisateurs malveillants d’obtenir l’accès, tout en augmentant votre contrôle et prise de conscience de l’environnement.

PAM complique la tâche des personnes malveillantes qui tentent de pénétrer sur un réseau et d’obtenir un accès de compte privilégié. PAM renforce la protection des groupes privilégiés qui contrôlent l’accès à différents ordinateurs appartenant à un domaine et aux applications installées sur ces ordinateurs. Il ajoute également des fonctionnalités de surveillance et de visibilité, ainsi que des contrôles plus affinés permettant aux organisations de savoir qui sont leurs administrateurs privilégiés et ce qu’ils font. PAM permet aux entreprises d’en savoir plus sur l’utilisation des comptes d’administration dans l’environnement.

## Configuration de PAM
PAM repose sur le principe de l’administration juste-à-temps, qui est lié à l’[administration JEA (Just Enough Administration)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA est une boîte à outils Windows PowerShell qui définit un ensemble de commandes pour effectuer des activités privilégiées et un point de terminaison où les administrateurs peuvent obtenir l'autorisation pour exécuter ces commandes. Dans JEA, un administrateur décide que les utilisateurs disposant d’un certain privilège peuvent effectuer une tâche particulière. Chaque fois qu’un utilisateur éligible doit effectuer cette tâche, il active cette autorisation. Les autorisations expirent après un laps de temps spécifié. Ainsi, un utilisateur malveillant ne peut pas dérober l’accès.

La configuration et l’utilisation de PAM comportent quatre étapes.

![Étapes dans PAM : préparation, protection, utilisation, surveillance - diagramme](media/MIM_PIM_SetupProcess.png)

1.  **Préparation**: identifiez les groupes dans votre forêt existante qui disposent de privilèges importants. Recréez ces groupes sans membres dans la forêt bastion.

2.  **Protection** : configurez la protection du cycle de vie et de l’authentification, telle que l’authentification multifacteur (MFA), pour le cas où les utilisateurs demandent une administration juste-à-temps. L'authentification multifacteur permet de prévenir les attaques par programmation à partir de logiciels malveillants ou suite à un vol d'informations d'identification.

3.  **Utilisation** : une fois que les exigences d’authentification ont été satisfaites et qu’une demande a été approuvée, un compte d’utilisateur est ajouté temporairement à un groupe privilégié dans la forêt bastion. Pendant un laps de temps prédéfini, l’administrateur dispose de tous les privilèges et autorisations d’accès qui sont affectés à ce groupe. Au bout de ce laps de temps, le compte est supprimé du groupe.

4.  **Surveillance** : PAM ajoute des fonctionnalités d’audit, d’alertes et de création de rapports pour les demandes d’accès privilégié. Vous pouvez consulter l’historique de l’accès privilégié et voir qui a effectué une activité. Vous pouvez décider si l’activité est valide ou non et identifier facilement toute activité non autorisée, par exemple une tentative d’ajout d’un utilisateur directement à un groupe privilégié dans la forêt d’origine. Cette étape est importante non seulement pour identifier les logiciels malveillants, mais également pour suivre les pirates « infiltrés ».

## Fonctionnement de PAM
PAM est basé sur les nouvelles fonctionnalités des services AD DS, en particulier pour l’authentification et l’autorisation des comptes de domaine, ainsi que sur les nouvelles fonctionnalités de Microsoft Identity Manager. PAM sépare les comptes privilégiés d’un environnement Active Directory existant. Quand un compte privilégié doit être utilisé, il doit tout d'abord être demandé, puis approuvé. Après l'approbation, l'autorisation est accordée au compte privilégié via un groupe principal étranger dans une nouvelle forêt bastion plutôt que dans la forêt actuelle de l'utilisateur ou de l'application. L'utilisation d'une forêt bastion offre à l'organisation un meilleur contrôle. Vous pouvez par exemple déterminer quand un utilisateur peut être membre d'un groupe privilégié et comment l'utilisateur doit s'authentifier.

Vous pouvez également déployer Active Directory, le service MIM et d'autres parties de cette solution dans une configuration haute disponibilité.

L'exemple suivant illustre plus en détail le fonctionnement de PIM.

![Processus et participants PIM - diagramme](media/MIM_PIM_howitworks.png)

La forêt bastion émet des appartenances de groupe de durée limitée, qui à leur tour génèrent des tickets TGT (Ticket Granting Ticket) de durée limitée. Les services ou applications basés sur Kerberos peuvent honorer et appliquer ces tickets TGT, si les applications et les services existent dans les forêts qui approuvent la forêt bastion.

Il n’est pas obligatoire de déplacer les comptes d’utilisateur quotidiens vers une nouvelle forêt. Il en va de même pour les ordinateurs, les applications et leurs groupes. Ils restent là où ils sont aujourd'hui dans une forêt existante. Prenons comme exemple une organisation qui a quelques inquiétudes aujourd'hui concernant ces problèmes de sécurité, mais qui ne prévoit pas pour le moment d'effectuer de mise à niveau de l'infrastructure de serveur vers la prochaine version de Windows Server. Cette organisation peut quand même tirer parti de cette solution combinée en utilisant MIM et une nouvelle forêt bastion pour mieux contrôler l'accès aux ressources existantes.

PAM offre les avantages suivants :

-   **Isolation/étendue des privilèges** : les utilisateurs ne détiennent pas de privilèges sur des comptes qui sont également utilisés pour des tâches sans privilèges telles que la vérification du courrier électronique ou la navigation sur Internet. Les utilisateurs doivent demander des privilèges. Les demandes sont approuvées ou refusées en fonction des stratégies MIM définies par un administrateur PAM. Tant qu'aucune demande n'a été approuvée, l'accès privilégié n'est pas disponible.

-   **Step-up et proof-up** : il s’agit de nouvelles stimulations d’authentification et d’autorisation qui permettent de gérer le cycle de vie de comptes d’administration distincts. L'utilisateur peut demander l'élévation d'un compte d'administration et cette demande passe par les flux de travail MIM.

-   **Journalisation supplémentaire** : en plus des flux de travail MIM intégrés, il existe une journalisation supplémentaire pour PAM qui identifie la demande, comment elle a été autorisée et tous les événements qui se produisent après l’approbation.

-   **Flux de travail personnalisable**: vous pouvez configurer les flux de travail MIM pour différents scénarios et utiliser plusieurs flux de travail, en fonction des paramètres de l'utilisateur demandeur ou des rôles demandés.

## Comment les utilisateurs demandent-ils un accès privilégié ?
Un utilisateur peut soumettre une demande de plusieurs manières :  
- API des services web des Services MIM  
- Point de terminaison REST  
- Windows PowerShell (`New-PAMRequest`)

## Quels sont les flux de travail et les options de surveillance disponibles ?
En guise d'exemple, supposons qu'un utilisateur était membre d'un groupe d'administration avant la configuration de PIM. Dans le cadre de cette configuration, l'utilisateur est supprimé du groupe d'administration et une stratégie est créée dans MIM. La stratégie spécifie que si cet utilisateur demande des privilèges d'administrateur et qu'il est authentifié par l'authentification multifacteur, la demande est approuvée et un compte distinct pour l'utilisateur est ajouté au groupe privilégié dans la forêt bastion.

En supposant que la demande soit approuvée, le flux de travail Action communique directement avec la forêt bastion Active Directory pour ajouter un utilisateur à un groupe. Par exemple, quand Jean demande à administrer la base de données de ressources humaines, son compte d'administration est ajouté au groupe privilégié dans la forêt bastion en quelques secondes. L’appartenance au groupe de son compte d’administration expire après un certain délai. Avec Windows Server (version d'évaluation technique), cette appartenance est associée dans Active Directory avec une limite de temps ; avec Windows Server 2012 R2 dans la forêt bastion, ce délai est appliqué par MIM.

> [!NOTE]
> Quand vous ajoutez un nouveau membre à un groupe, la modification doit être répliquée vers d'autres contrôleurs de domaine dans la forêt bastion. La latence de réplication peut affecter la capacité des utilisateurs à accéder aux ressources. Pour plus d’informations sur la latence de réplication, consultez [How Active Directory Replication Topology Works (Fonctionnement de la topologie de réplication Active Directory)](https://technet.microsoft.com/library/cc755994.aspx).
>
> En revanche, un lien ayant expiré est évalué en temps réel par le Gestionnaire de comptes de sécurité (SAM). Même si l’ajout d’un membre du groupe doit être répliqué par le contrôleur de domaine qui reçoit la demande d’accès, la suppression d’un membre de groupe est évaluée instantanément sur n’importe quel contrôleur de domaine.

Ce flux de travail est conçu spécifiquement pour ces comptes d'administration. Les administrateurs (ou même les scripts) qui n'ont besoin que d'un accès occasionnel à des groupes privilégiés peuvent demander précisément cet accès. MIM enregistre la demande et les modifications dans Active Directory, et vous pouvez les afficher dans l’Observateur d’événements ou envoyer les données à des solutions de surveillance d’entreprise telles que System Center 2012 - Operations Manager Audit Collection Services (ACS) ou d’autres outils tiers.



<!--HONumber=Jul16_HO3-->


