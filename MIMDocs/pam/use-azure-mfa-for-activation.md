---
title: Utiliser Azure MFA pour activer PAM | Microsoft Identity Manager
description: "Configurez Azure MFA comme une deuxième couche de sécurité quand les utilisateurs activent les rôles dans Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 518a7e165946049745c8eea15ecb61866d6f9c04


---

# Utilisation d’Azure MFA pour l’activation
Quand vous configurez un rôle PAM, vous pouvez choisir comment autoriser les utilisateurs qui demandent à activer le rôle. Les choix implémentés par l’activité d’autorisation PAM sont les suivants :

- Approbation du propriétaire du rôle
- Azure Multi-Factor Authentication (MFA)

Si aucun choix n’est effectué, les utilisateurs candidats sont automatiquement activés pour leur rôle.

Microsoft Azure Multi-Factor Authentication (MFA) est un service d'authentification qui oblige les utilisateurs à vérifier leurs tentatives de connexion à l'aide d'une application mobile, d'un appel téléphonique ou d'un message texte. Vous pouvez l’utiliser avec Microsoft Azure Active Directory, et en tant que service pour les applications d’entreprise locales et cloud. Pour le scénario PAM, Azure MFA fournit un mécanisme d’authentification supplémentaire qui peut être utilisé au niveau de l’autorisation, quelle que soit la façon dont un utilisateur candidat s’est authentifié précédemment sur le domaine Windows PRIV.

## Conditions préalables

Pour utiliser Azure MFA avec MIM, vous avez besoin des éléments suivants :

- accès Internet à partir de chaque service MIM fournissant PAM, pour contacter le service Azure MFA ;
- abonnement Azure ;
- licences Azure Active Directory Premium des utilisateurs candidats, ou tout autre mode de gestion des licences Azure MFA ;
- numéros de téléphone de tous les utilisateurs candidats.

## Création d’un fournisseur Azure MFA

Dans cette section, vous allez configurer votre fournisseur Azure MFA dans Microsoft Azure Active Directory.  Si vous utilisez déjà Azure MFA, que ce soit de manière autonome ou configuré avec Azure Active Directory Premium, passez à la section suivante.

1.  Ouvrez un navigateur web et connectez-vous au [portail classique Azure](https://manage.windowsazure.com) comme administrateur d’abonnement Azure.

2.  Dans le coin inférieur gauche, cliquez sur **Nouveau**.

3.  Cliquez sur **Services d’application > Active Directory > Fournisseur d’authentification multifacteur > Création rapide**.

4.  Dans le champ **Nom** , entrez **PAM**puis, dans le champ Modèle d’utilisation, sélectionnez Par utilisateur activé. Si vous avez déjà un annuaire Azure AD, sélectionnez ce dernier. Pour finir, cliquez sur **Créer**.

## Téléchargement des informations d’identification du Service Azure MFA

Vous allez générer ensuite un fichier qui inclut les éléments d’authentification permettant à PAM de contacter Azure MFA.

1. Ouvrez un navigateur web et connectez-vous au [portail classique Azure](https://manage.windowsazure.com) comme administrateur d’abonnement Azure.

2.  Cliquez sur **Active Directory** dans le menu du portail Azure, puis cliquez sur l'onglet **Fournisseurs d'authentification multifacteur** .

3.  Cliquez sur le fournisseur Azure MFA à utiliser pour PAM, puis cliquez sur **Gérer**.

4.  Dans la nouvelle fenêtre, dans le volet gauche, sous **Configurer**, cliquez sur **Paramètres**.

5.  Dans la fenêtre **Azure Multi-Factor Authentication** , cliquez sur **SDK** sous **Téléchargements**.

6.  Cliquez sur le lien **Télécharger** dans la colonne ZIP pour le fichier avec le langage **SDK pour ASP.NET 2.0 C\#**.

![Télécharger le SDK Multi-Factor Authentication - capture d’écran](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Copiez le fichier ZIP résultant pour chaque système où le service MIM est installé. 

>[!NOTE]
> Le fichier ZIP contient des clés qui sont utilisées pour l’authentification auprès du service Azure MFA.

## Configuration du service MIM pour Azure MFA

1.  Sur l’ordinateur où est installé le service MIM, connectez-vous soit comme administrateur, soit comme utilisateur ayant installé MIM.

2.  Créez un dossier de répertoire sous le répertoire où le Service MIM a été installé, tel que `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`.

3.  Avec l’Explorateur Windows, accédez au dossier **pf\\certs** du fichier ZIP téléchargé à la section précédente et copiez le fichier **cert\_key.p12** dans le nouveau répertoire.

4.  Dans l’Explorateur Windows, naviguez jusqu’au dossier **pf** du fichier ZIP, puis ouvrez le fichier **pf\_auth.cs** dans un éditeur de texte comme Wordpad.

5.  Recherchez ces trois paramètres : **LICENSE\_KEY**, **GROUP\_KEY**, **CERT\_PASSWORD**.

![Copier les valeurs à partir du fichier pf\_auth.cs - capture d’écran](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  Dans le Bloc-notes, ouvrez le fichier **MfaSettings.xml** situé dans `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`.

7.  Copiez les valeurs des paramètres LICENSE\_KEY, GROUP\_KEY et CERT\_PASSWORD du fichier pf\_auth.cs vers leurs éléments xml respectifs dans le fichier MfaSettings.xml.

8.  Dans l’élément XML **<CertFilePath>**, spécifiez le chemin complet du fichier cert\_key.p12 extrait précédemment.

9.  Dans l’élément **<username>**, entrez un nom d’utilisateur.

10.  Dans l’élément **<DefaultCountryCode>**, entrez l’indicatif du pays pour appeler vos utilisateurs, par exemple 1 pour les États-Unis et le Canada. Cette valeur est utilisée quand les utilisateurs sont inscrits avec des numéros de téléphone qui n’ont pas d’indicatif du pays. Si le numéro de téléphone d’un utilisateur a un indicatif international distinct de celui configuré pour l’organisation, cet indicatif du pays doit être inclus dans le numéro de téléphone à inscrire.

11.  Enregistrez et remplacez le fichier **MfaSettings.xml** dans le dossier Service MIM `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`. 

> [!NOTE]
> À la fin du processus, vérifiez que le fichier **MfaSettings.xml**, sa copie ou le fichier ZIP ne sont pas accessibles publiquement en lecture.

## Configurer les utilisateurs PAM pour Azure MFA

Pour qu’un utilisateur active un rôle qui nécessite Azure MFA, le numéro de téléphone de l’utilisateur doit être stocké dans MIM. Cet attribut est défini de deux façons.

Tout d’abord, la commande `New-PAMUser` copie un attribut de numéro de téléphone de l’entrée d’annuaire de l’utilisateur du domaine CORP vers la base de données du service MIM. Notez qu’il s’agit d’une opération unique.

Ensuite, la commande `Set-PAMUser` met à jour l’attribut de numéro de téléphone dans la base de données du service MIM. Par exemple, la syntaxe suivante permet de remplacer le numéro de téléphone d’un utilisateur PAM existant dans le service MIM. Leur entrée d’annuaire est inchangée.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## Configurer des rôles PAM pour Azure MFA

Une fois que tous les utilisateurs candidats d’un rôle PAM ont leurs numéros de téléphone stockés dans la base de données du service MIM, le rôle peut être configuré pour exiger Azure MFA. Cette opération s’effectue à l’aide de la commande `New-PAMRole` ou `Set-PAMRole`. Par exemple,

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Vous pouvez désactiver Azure MFA pour un rôle en spécifiant le paramètre « -MFAEnabled 0 » dans la commande `Set-PAMRole`.

## Résolution des problèmes

Vous trouverez les événements suivants dans le journal des événements de la gestion des accès privilégiés (Privileged Access Management, PAM) :

| ID  | Gravité | Généré par | Description |
|-----|----------|--------------|-------------|
| 101 | Erreur       | Service MIM            | L’utilisateur n’a pas effectué l’authentification Azure MFA (par exemple, il n’a pas répondu au téléphone) |
| 103 | Informations | Service MIM            | L’utilisateur a effectué l’authentification Azure MFA durant l’activation                       |
| 825 | Avertissement     | Service d’analyse PAM | Le numéro de téléphone a changé.                                |

Pour plus d’informations sur l’échec des appels téléphoniques (événement 101), vous pouvez également consulter ou télécharger un rapport à partir d’Azure MFA.

1.  Ouvrez un navigateur web et connectez-vous au [portail classique Azure](https://manage.windowsazure.com) comme administrateur général Azure AD.

2.  Sélectionnez **Active Directory** dans le menu du portail Azure, puis sélectionnez l’onglet **Fournisseurs d’authentification multifacteur**.

3.  Sélectionnez le fournisseur Azure MFA utilisé pour PAM, puis cliquez sur **Gérer**.

4.  Dans la nouvelle fenêtre, cliquez sur **Utilisation**, puis sur **Détails de l’utilisateur**.

5.  Sélectionnez l’intervalle de temps, puis cochez la case à côté de **Nom** dans la colonne de rapport supplémentaire. Cliquez sur **Exporter au format CSV**.

6.  Une fois le rapport généré, vous pouvez le consulter sur le portail ou, si le rapport MFA est étendu, vous pouvez le télécharger dans un fichier CSV. Les valeurs **SDK** dans la colonne **TYPE D’AUTHENTIFICATION** indiquent les lignes pertinentes pour des demandes d’activation PAM : il s’agit d’événements provenant de MIM ou d’autres logiciels locaux. Le champ **NOM D’UTILISATEUR** représente le GUID de l’objet utilisateur dans la base de données du service MIM. En cas d’échec d’un appel, la valeur de la colonne **AUTHD** est **Non**, et la valeur de la colonne **RÉSULTAT DE L’APPEL** contient les détails décrivant la raison de l’échec.



<!--HONumber=Jul16_HO3-->


