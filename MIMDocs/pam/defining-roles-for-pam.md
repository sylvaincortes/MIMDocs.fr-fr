---
title: "Définir des rôles pour Privileged Access Management | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: 7ba6f744f7fb7a1c5052b14669aa3de2cd10ddbb


---

# Définir des rôles pour Privileged Access Management

Avec Privileged Access Management, vous pouvez affecter des utilisateurs à des rôles privilégiés qu’ils peuvent activer si nécessaire pour un accès juste-à-temps. Ces rôles sont définis manuellement et établis dans l’environnement bastion. Cet article vous guide tout au long du processus de détermination des rôles à gérer par le biais de PAM et de leur définition avec des restrictions et des autorisations appropriées.

Une approche simple de la définition des rôles pour la gestion de l’accès privilégié consiste à compiler toutes les informations dans une feuille de calcul. Énumérez les rôles dans les rôles et utilisez les colonnes pour identifier les autorisations et les exigences de gouvernance.

Les exigences de gouvernance varient en fonction des exigences de conformité ou d’identité et de stratégies d’accès existantes. Parmi les paramètres à identifier pour chaque rôle figurent le propriétaire du rôle, les utilisateurs candidats susceptibles d’occuper ce rôle et les contrôles d’authentification, d’approbation ou de notification qui doivent être associés à l’utilisation du rôle.

Les autorisations des rôles dépendent des applications gérées. Cet article utilise Active Directory comme exemple d’application, classant les autorisations en deux catégories :

- Les autorisations nécessaires à la gestion du service Active Directory proprement dit (par exemple, configuration de la topologie de réplication)

- Les autorisations nécessaires à la gestion des données contenues dans Active Directory (par exemple, création d’utilisateurs et de groupes)

## Identifier les rôles

Commencez par identifier tous les rôles que vous souhaitez gérer avec PAM. Sur la feuille de calcul, chaque rôle potentiel aura sa propre ligne.

Pour rechercher les rôles appropriés, pensez à la gestion de chaque application :

- L’application est-elle de niveau 0, 1 ou 2 ?  
- Quels sont les privilèges ayant un impact sur la confidentialité, l’intégrité ou la disponibilité de l’application ?  
- L’application a-t-elle des dépendances vis-à-vis d’autres composants du système, tels que des bases de données, une infrastructure réseau ou de sécurité, ou la plateforme de virtualisation ou d’hébergement ?

Déterminez comment grouper ces considérations relatives aux applications. Les rôles doivent avoir des limites claires et donner uniquement les autorisations suffisantes pour effectuer des tâches administratives courantes dans l’application.

Vous devez toujours créer des rôles pour l’attribution de privilèges minimum. Elle peut s’appuyer sur les responsabilités organisationnelles actuelles (ou planifiées) des utilisateurs et comprend les privilèges dont ont besoin les utilisateurs pour effectuer leurs tâches. Elle peut aussi inclure les privilèges qui simplifient les opérations, sans créer de risque.

Voici les autres points à prendre en considération pour définir la portée des autorisations d’inclusion de rôle :

- Combien y a-t-il de personnes qui officient dans un rôle particulier ? S’il n’y en a pas au moins deux, le rôle est peut-être défini trop étroitement pour être utile ou vous avez défini les fonctions d’une personne spécifique.

- Combien de rôles une personne assume-t-elle ? Les utilisateurs sont-ils en mesure de choisir le rôle qui correspond à leur tâche ?

- La population d’utilisateurs et la façon dont ceux-ci interagissent avec les applications sont-elles compatibles avec Privileged Access Management ?

- Est-il possible de séparer l’administration de l’audit, de sorte qu’un utilisateur ayant un rôle d’administrateur ne puisse pas effacer les enregistrements d’audit de leurs actions?

## Établir les exigences de gouvernance des rôles

Au moment d’identifier des rôles candidats, commencez par compléter la feuille de calcul. Créez des colonnes pour les exigences appropriées à votre organisation. Voici quelques-unes des exigences à prendre en considération :

- Qui est le propriétaire du rôle qui sera chargé de mieux définir le rôle, de choisir les autorisations et de gérer les paramètres de gouvernance du rôle ?

- Qui sont les détenteurs du rôle (utilisateurs) qui effectuent les tâches associées ?

- Quelle est la méthode d’accès (sujet abordé dans la section suivante) qui conviendrait pour les détenteurs du rôle ?

- Un utilisateur qui active son rôle doit-il faire l’objet d’une approbation manuelle de la part du propriétaire du rôle ?

- Un utilisateur qui active son rôle doit-il être suivi d’une notification ?

- L’utilisation de ce rôle doit-elle générer une alerte ou une notification dans un système SIEM à des fins de suivi ?

- Est-il utile de limiter les utilisateurs pouvant activer le rôle aux seuls utilisateurs pouvant se connecter à des ordinateurs quand l’exécution des tâches liées au rôle nécessite un accès et quand l’hôte est suffisamment vérifié pour empêcher, le cas échéant, toute utilisation incorrecte des privilèges/informations d’identification ?

- Est-il nécessaire de fournir aux détenteurs du rôle une station de travail dédiée à l’administration ?

- Quelles sont les autorisations d’application (voir l’exemple de liste pour AD ci-dessous) associées à ce rôle ?

## Sélectionner une méthode d’accès

Les autorisations assignées aux différents rôles d’un système Privileged Access Management peuvent être identiques si différentes communautés d’utilisateurs ont des exigences de gouvernance d’accès distinctes. Par exemple, une organisation peut appliquer différentes stratégies pour leurs employés à plein temps et pour les employés qui viennent d’une autre entreprise.

Dans certains cas, un rôle peut être attribué de manière permanente à un utilisateur, ce qui le dispense de demander ou d’activer une attribution de rôle. Voici quelques exemples de scénarios d’attribution permanente :

- Compte de service administré dans une forêt existante.

- Compte d’utilisateur de la forêt existante, dont les informations d’identification sont gérées en dehors de PAM (par exemple, un compte « de secours » dans lequel un rôle, tel que « Maintenance de domaine/contrôleur de domaine », nécessaire à la résolution de problèmes de confiance et d’intégrité du contrôleur de domaine est affecté de manière permanente au compte, avec un mot de passe sécurisé physiquement).

- Compte d’utilisateur de la forêt d’administration qui s’authentifie avec un mot de passe (par exemple, un utilisateur nécessitant des autorisations d’administration permanentes 24 heures sur 24 et 7 jours sur 7 et se connectant à partir d’un appareil qui ne prend pas en charge l’authentification forte).

- Compte d’utilisateur de la forêt d’administration disposant d’une carte à puce ou d’une carte à puce virtuelle (par exemple, un compte doté d’une carte à puce hors connexion nécessaire pour effectuer des tâches de maintenance peu fréquentes).

Pour les organisations préoccupées par les risques de vol ou de mauvaise utilisation des informations d’identification, le guide [Utilisation d’Azure MFA pour l’activation](use-azure-mfa-for-activation.md) fournit des instructions de configuration MIM permettant d’exiger une vérification hors bande supplémentaire au moment de l’activation de rôle.

## Déléguer des autorisations Active Directory

Windows Server crée automatiquement des groupes par défaut tels que « Admins du domaine » lors de la création de domaines. Ces groupes simplifient la prise en main et peuvent être adaptés aux organisations de petite taille. Toutefois, les grandes organisations ou celles nécessitant un meilleur isolement des privilèges administratifs doivent vider les groupes tels que Admins du domaine et les remplacer par des groupes qui fournissent des autorisations affinées.

Le groupe Admins du domaine ne peut pas avoir de membres issus d’un domaine externe. Autre limitation, il accorde des autorisations pour trois fonctions distinctes :  
- Gestion du service Active Directory proprement dit  
- Gestion des données contenues dans Active Directory  
- Activation des ouvertures de session distantes sur les ordinateurs joints au domaine.

À la place des groupes par défaut comme Admins du domaine, créez des groupes de sécurité qui fournissent uniquement les autorisations nécessaires et utilisez MIM pour fournir de manière dynamique des comptes d’administrateur avec ces appartenances aux groupes.

### Autorisations de gestion de service

Le tableau suivant donne des exemples d’autorisations qu’il serait pertinent d’inclure dans des rôles pour gérer Active Directory.

| Rôle | Description |
| ---- | ---- |
| Maintenance de domaine/contrôleur de domaine | Appartenance au groupe Domaine\Administrateurs qui permet de dépanner et modifier le système d’exploitation du contrôleur de domaine, de promouvoir un nouveau contrôleur de domaine dans un domaine existant de la forêt et la délégation de rôle AD.
|Gestion des machines virtuelles | Gestion des machines virtuelle du contrôleur de domaine à l’aide du logiciel d’administration de la virtualisation. Vous pouvez accorder ce privilège par le biais d’un contrôle total de toutes les machines virtuelles dans l’outil d’administration ou à l’aide de la fonctionnalité de contrôle d’accès en fonction du rôle (RBAC). |
| Extension du schéma | Gestion du schéma consistant notamment à ajouter de nouvelles définitions d’objets, à modifier les autorisations sur les objets du schéma et à modifier les autorisations par défaut du schéma pour les types d’objets. |
| Sauvegarde de la base de données Active Directory | Création d’une copie de sauvegarde de la base de données Active Directory dans son intégralité, y compris tous les secrets confiés au contrôleur de domaine et au domaine. |
| Gestion des approbations et des niveaux fonctionnels | Création et suppression d’approbations associées à des domaines et forêts externes. |
| Gestion des sites, des sous-réseaux et de la réplication | Gestion des objets de la topologie de réplication Active Directory, consistant notamment à modifier des sites, des sous-réseaux et des objets lien de site, et à lancer les opérations de réplication. |
| Gestion d’objets GPO | Création, suppression et modification d’objets de stratégie de groupe (GPO) à l’échelle du domaine. |
| Gestion de zones | Création, suppression et modification de zones et d’objets DNS dans Active Directory. |
| Modification d’unités d’organisation de niveau 0 | Modification d’unités d’organisation de niveau 0 et des objets contenus dans Active Directory. |

### Autorisations de gestion de données

Le tableau suivant donne des exemples d’autorisations qu’il serait pertinent d’inclure dans des rôles pour gérer ou utiliser les données contenues dans AC.

| Rôle | Description |
| ---- | ---- |
| Modification d’unité d’organisation d’administration de niveau 1                 | Modification des unités d’organisation contenant des objets d’administration de niveau 1 dans Active Directory |
| Modification d’unité d’organisation d’administration de niveau 2                 | Modification des unités d’organisation contenant des objets d’administration de niveau 2 dans Active Directory |
| Gestion des comptes : création/suppression/déplacement | Modification des comptes d’utilisateurs standard.                                      |
| Gestion des comptes : réinitialisation/déverrouillage       | Réinitialisation des mots de passe et déverrouillage des comptes.                                  |
| Groupe de sécurité : création/modification          | Création et modification des groupes de sécurité dans Active Directory.              |
| Groupe de sécurité : suppression                 | Suppression des groupes de sécurité dans Active Directory.                         |
| Gestion des objets de stratégie de groupe                         | Gestion de tous les objets de stratégie de groupe (GPO) du domaine/forêt qui n’affectent pas les serveurs de niveau 0.             |
| Jonction des PC/administrateur local                    | Droits d’administration locaux sur toutes les stations de travail.                               |
| Jonction des serveurs/administrateur local                   | Droits d’administration locaux sur tous les serveurs.                                    |

## Exemples de définitions de rôle

Le choix des définitions de rôle dépend du niveau des serveurs gérés par les comptes privilégiés. Il dépend aussi du choix des applications gérées, car certaines applications comme Exchange ou d’autres produits d’entreprise tiers comme SAP s’accompagnent souvent de leurs propres définitions de rôles pour l’administration déléguée.

Les sections suivantes donnent des exemples de scénarios d’entreprise types.

### Niveau 0 - Forêt d’administration

Voici quelques rôles pouvant convenir aux comptes de l’environnement bastion :

- Accès d’urgence à la forêt d’administration
- Administrateurs « carton rouge » : utilisateurs ayant le rôle d’administrateur de la forêt d’administration
- Utilisateurs ayant le rôle d’administrateur de la forêt de production
- Utilisateurs dotés de droits d’administration limités délégués sur les applications de la forêt de production

### Niveau 0 - Forêt de production d’entreprise

Voici quelques rôles adaptés à la gestion des comptes et des ressources d’une forêt de production de niveau 0 :

- Accès d’urgence à la forêt de production
- Administrateurs de la stratégie de groupe
- Administrateurs DNS
- Administrateurs de l’infrastructure à clé publique (PKI)
- Administrateurs de la topologie et de la réplication AD
- Administrateurs de la virtualisation des serveurs de niveau 0
- Administrateurs du stockage
- Administrateurs des stratégies anti-programme malveillant pour les serveurs de niveau 0
- Administrateurs SCCM pour SCCM de niveau 0
- Administrateurs SCOM pour SCOM de niveau 0
- Administrateurs de sauvegarde de niveau 0
- Utilisateurs de contrôleurs hors bande et de gestion de la carte de base (pour la gestion KVM ou « LOM » à distance) connectés à des hôtes de niveau 0

### Niveau 1

Voici quelques rôles adaptés à la gestion et à la sauvegarde de serveurs de niveau 1 :

- Maintenance des serveurs
- Administrateurs de la virtualisation des serveurs de niveau 1
- Compte d’analyse de la sécurité
- Administrateurs des stratégies anti-programme malveillant pour les serveurs de niveau 1
- Administrateurs SCCM pour SCCM de niveau 1
- Administrateurs SCOM pour SCOM de niveau 1
- Administrateurs de sauvegarde des serveurs de niveau 1
- Utilisateurs de contrôleurs hors bande et de gestion de la carte de base (pour la gestion KVM ou« LOM » à distance) connectés à des hôtes de niveau 1

Voici en outre quelques rôles adaptés à la gestion d’applications d’entreprise de niveau 1 :

- Administrateurs DHCP
- Administrateurs Exchange
- Administrateurs Skype Entreprise
- Administrateurs de batteries de serveurs SharePoint
- Administrateurs d’un service cloud, par exemple, un site web d’entreprise ou un serveur DNS public
- Administrateurs de systèmes HCM, financiers ou juridiques

### Niveau 2

Voici quelques rôles adaptés à la gestion des ordinateurs et des utilisateurs non administrateurs :

- Administrateurs de comptes
- Support technique
- Administrateurs de groupes de sécurité
- Support technique des stations de travail



<!--HONumber=Jul16_HO2-->


