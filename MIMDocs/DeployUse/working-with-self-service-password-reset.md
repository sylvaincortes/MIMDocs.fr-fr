---
# required metadata

title: Utilisation de la réinitialisation de mot de passe en libre-service | Microsoft Identity Manager
description: Consultez les nouveautés de la réinitialisation de mot de passe libre-service dans MIM 2016, y compris comment SSPR fonctionne avec l’authentification multifacteur. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Utilisation de la réinitialisation du mot de passe libre-service
Microsoft Identity Manager 2016 étend la fonctionnalité de réinitialisation du mot de passe libre-service. Cette fonctionnalité a été améliorée de plusieurs manières :

-   Le portail de réinitialisation du mot de passe libre-service et l’écran de connexion de Windows permettent maintenant aux utilisateurs de déverrouiller leur compte sans modifier leur mot de passe ni appeler l’administrateur système. Les utilisateurs peuvent se retrouver sans accès à leur compte pour de nombreuses raisons légitimes, par exemple s’ils ont entré un ancien mot de passe, s’ils utilisent un ordinateur bilingue et que le clavier est configuré dans la mauvaise langue ou s’ils tentent de se connecter à une station de travail partagée déjà ouverte avec le compte d’un autre utilisateur.

-   Une nouvelle porte d'authentification, la porte téléphonique, a été ajoutée. Elle permet l'authentification des utilisateurs via un appel téléphonique.

-   La prise en charge de Microsoft Azure Multi-Factor Authentication (MFA) a été ajoutée. Vous pouvez l’utiliser pour la porte existante de message texte pour le mot de passe à usage unique ou pour la nouvelle porte téléphonique.

## Azure pour l’authentification multifacteur
Microsoft Azure Multi-Factor Authentication est un service d’authentification qui oblige les utilisateurs à vérifier leurs tentatives de connexion à l’aide d’une application mobile, d’un appel téléphonique ou d’un message texte. Vous pouvez l'utiliser avec Microsoft Azure Active Directory, et en tant que service pour les applications d'entreprise locales et cloud.

Azure MFA fournit un mécanisme d’authentification supplémentaire qui peut renforcer les processus d’authentification existants, tel que celui effectué par MIM pour l’assistance de connexion libre-service.

Avec l’authentification multifacteur Azure, les utilisateurs s’authentifient auprès du système pour vérifier leur identité quand ils tentent de réaccéder à leur compte et à leurs ressources. L'authentification peut s'effectuer via SMS ou appel téléphonique.   Plus l’authentification est forte, plus il est probable que la personne qui tente d’accéder au système soit la véritable propriétaire de l’identité. Une fois authentifié, l'utilisateur peut choisir un nouveau mot de passe pour remplacer l'ancien.

## Conditions préalables à la configuration du déverrouillage de compte et de la réinitialisation en libre-service avec MFA
Cette section part du principe que vous avez téléchargé et effectué le déploiement de Microsoft Identity Manager 2016, y compris des composants et services suivants :

-   Un ordinateur Windows Server 2008 R2 ou version ultérieure a été configuré comme serveur Active Directory, avec les services de domaine Active Directory et un contrôleur de domaine avec domaine désigné (domaine « d’entreprise »).

-   Une stratégie de groupe est définie pour le verrouillage de compte.

-   Le service de synchronisation MIM 2016 (Sync) est installé et en cours d’exécution sur un serveur joint au domaine Active Directory.

-   Le service et le portail MIM 2016, y compris le portail d’inscription SSPR et le portail de réinitialisation SSPR, sont installés et en cours d’exécution sur un serveur (qui peut être colocalisé avec le service de synchronisation).

-   Le service de synchronisation MIM est configuré pour la synchronisation d’identité AD-FIM, notamment :

    -   Configuration de l'agent de gestion Active Directory (ADMA) pour la connectivité aux services AD DS et capacité à importer des données d'identité et à les exporter vers Active Directory.

    -   Configuration de l’agent de gestion MIM (MIM MA) pour la connectivité à la base de données de service FIM et la capacité à importer des données d’identité et à les exporter vers la base de données FIM.

    -   Configuration des règles de synchronisation dans le portail MIM pour autoriser la synchronisation des données utilisateur et faciliter les activités de synchronisation dans le service MIM.

-   Les compléments et extensions MIM 2016, y compris le client intégré de connexion Windows SSPR, sont déployés sur le serveur ou sur un ordinateur client distinct.

## Préparer MIM au fonctionnement avec l’authentification multifacteur
Configurez le service de synchronisation MIM pour prendre en charge les fonctionnalités de réinitialisation du mot de passe et de déverrouillage de compte. Pour plus d’informations, consultez [Installation des compléments et extensions FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Installation de FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Portes d’authentification SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) et [Guide de laboratoire de test SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).

Dans la section suivante, vous allez configurer votre fournisseur Azure MFA dans Microsoft Azure Active Directory. Dans ce cadre, vous allez générer un fichier qui inclut les éléments d'authentification requis par MFA pour pouvoir contacter Azure Multi-Factor Authentication.  Pour pouvoir effectuer cette opération, vous avez besoin d'un abonnement Azure.

### Inscrire votre fournisseur d’authentification multifacteur dans Azure

1.  Accédez au [portail Azure Classic](http://manage.windowsazure.com) et connectez-vous en tant qu’administrateur d’un abonnement Azure.

2.  Dans le coin inférieur gauche, cliquez sur **Nouveau**.

3.  Cliquez sur **Services d'application &gt; Active Directory &gt; Fournisseur d'authentification multifacteur &gt; Création rapide**.

![Création rapide d’une image MFA sur le portail Azure](media/MIM-SSPR-Azureportal.png)

4.  Dans le champ **Nom**, entrez **SSPRMFA** et cliquez sur **Créer**.

![Image MFA du portail Azure](media/MIM-SSPR-Azureportal-2.png)

5.  Cliquez sur **Active Directory** dans le menu du portail Azure Classic, puis cliquez sur l’onglet **Fournisseurs d’authentification multifacteur**.

6.  Cliquez sur **SSPRMFA** , puis sur **Gérer** en bas de l'écran.

    ![Icône gérer le portail Azure](media/MIM-SSPR-ManageButton.png)

7.  Dans la nouvelle fenêtre, dans le volet de gauche, sous **Configurer**, cliquez sur **Paramètres**.

8.  Sous **Alerte fraude**, désactivez **Bloquer l'utilisateur en cas de signalement de fraude** . Cette action permet d'éviter le blocage de l'ensemble du service.

9. Dans la fenêtre **Azure Multi-Factor Authentication** qui s'affiche, cliquez sur **SDK** sous **Téléchargements** dans le menu de gauche.

10. Cliquez sur le lien **Télécharger** dans la colonne ZIP pour le fichier avec le langage **Kit de développement logiciel (SDK) pour ASP.NET 2.0 C#**.

    ![Image du fichier zip de téléchargement d’Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Copiez le fichier ZIP résultant pour chaque système où le service MIM est installé.  N'oubliez pas que le fichier ZIP contient des clés qui sont utilisées pour l'authentification auprès du service Azure Multi-Factor Authentication.

### Mettre à jour le fichier de configuration

1. Ouvrez une session sur l’ordinateur où le service MIM est installé, en tant qu’utilisateur ayant installé MIM.

2. Créez un dossier sous le répertoire où le service MIM a été installé, comme **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Avec l’Explorateur Windows, accédez au dossier **pf\\certs** du fichier zip téléchargé à la section précédente et copiez le fichier **cert\_key.p12** dans le nouveau répertoire.

4.  Dans le fichier zip du Kit SDK, dans le dossier **\pf**, ouvrez le fichier **pf_auth.cs**.

5.  Recherchez ces trois paramètres : `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Image du code pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  Dans **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**, ouvrez le fichier : **MfaSettings**.xml.

7.  Copiez les valeurs des paramètres `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` du fichier pf_aut.cs dans leurs éléments xml respectifs dans le fichier MfaSettings.xml.

8.  Dans le fichier zip du Kit SDK, sous \pf\certs, extrayez le fichier **cert_key.p12** et entrez son chemin d'accès complet dans le fichier MfaSettings.xml dans l'élément xml `<CertFilePath>` .

9. Dans l'élément `<username>` , entrez un nom d'utilisateur quelconque.

10. Dans l'élément `<DefaultCountryCode>`, entrez votre code de pays par défaut. Si des numéros de téléphone sont enregistrés pour des utilisateurs sans code de pays, il s'agit du code de pays qu'ils obtiendront. Si un utilisateur a un code de pays international, vous devez l'inclure dans le numéro de téléphone enregistré.

11. Enregistrez le fichier MfaSettings.xml avec le même nom au même emplacement.

#### Configurer la porte téléphonique ou la porte de message texte pour le mot de passe à usage unique

1.  Lancez Internet Explorer et accédez au portail MIM, en vous authentifiant en tant qu'administrateur MIM, puis cliquez sur  **Workflows** dans la barre de navigation de gauche.

    ![Image de navigation sur le portail MIM](media/MIM-SSPR-workflow.jpg)

2.  Sélectionnez **Flux de travail d'authentification de réinitialisation de mot de passe**.

    ![Image des flux de travail du portail MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Cliquez sur l'onglet **Activités** , puis faites défiler jusqu'à **Ajouter une activité**.

4.  Sélectionnez **Porte téléphonique** ou **Porte de message texte pour le mot de passe à usage unique**, cliquez sur **Sélectionner**, puis sur **OK**.

Les utilisateurs de votre organisation peuvent désormais s'inscrire pour la réinitialisation du mot de passe.  Lors de ce processus, les utilisateurs vont entrer leur numéro de téléphone professionnel ou mobile pour que le système sache comment les appeler (ou comment leur envoyer des messages SMS).

#### Inscrire des utilisateurs pour la réinitialisation du mot de passe

1.  Un utilisateur lance un navigateur web pour accéder au portail d'inscription pour la réinitialisation de mot de passe MIM.  (En général, ce portail est configuré avec l'authentification Windows.)  Dans le portail, ils vont fournir à nouveau leur nom d'utilisateur et leur mot de passe pour confirmer leur identité.

    Ils doivent accéder au portail d'enregistrement du mot de passe et s'authentifier à l'aide de leur nom d'utilisateur et de leur mot de passe.

2.  Dans le champ **Numéro de téléphone** ou **Téléphone portable**  , ils doivent entrer un code de pays, un espace et le numéro de téléphone, puis cliquer sur **Suivant**.

    ![Image de la vérification par téléphone MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Image de la vérification par téléphone mobile MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## Comment cela fonctionne-t-il pour vos utilisateurs ?
Maintenant que tout est configuré et opérationnel, vous souhaiterez peut-être savoir ce que vos utilisateurs devront faire quand ils réinitialiseront leur mot de passe juste avant de partir en vacances et s'apercevront, une fois revenus, qu'ils l'ont complètement oublié.

Il existe deux façons pour un utilisateur d’utiliser la fonctionnalité de réinitialisation du mot de passe et de déverrouillage de compte : à partir de l’écran de connexion Windows ou à partir du portail libre-service.

En installant les compléments et extensions MIM sur un ordinateur joint au domaine connecté via le réseau de votre organisation au service MIM, les utilisateurs peuvent résoudre un problème de mot de passe oublié via l'utilisation de la connexion à l'ordinateur.  La procédure suivante vous guide tout au long du processus.

#### Réinitialisation du mot de passe intégrée à la connexion au Bureau Windows

1.  Si votre utilisateur entre un mot de passe incorrect plusieurs fois, dans l’écran de connexion il aura la possibilité de cliquer sur **Vous rencontrez des problèmes de connexion ?**. .

    ![Image de l’écran de connexion](media/MIM-SSPR-problemsloggingin.JPG)

    Un clic sur ce lien mène à l’écran de réinitialisation du mot de passe MIM, où il pourra modifier son mot de passe ou déverrouiller son compte.

    ![Image de réinitialisation du mot de passe dans MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  L'utilisateur sera dirigé pour s'authentifier. Si MFA a été configuré, l'utilisateur recevra un appel téléphonique.

3.  En arrière-plan, Azure MFA appelle le numéro que l'utilisateur a donné quand il s'est inscrit pour le service.

4.  Quand l'utilisateur répond au téléphone, il est invité à appuyer sur la touche dièse (#). Ensuite, l'utilisateur clique sur **Suivant** dans le portail.

    Si vous avez défini d'autres portes, l'utilisateur est invité à fournir davantage d'informations dans les écrans suivants.

    > [!NOTE]
    > Si l’utilisateur s’impatiente et clique sur **Suivant** avant d’appuyer sur la touche dièse, l’authentification échoue.

5.  Après une authentification réussie, l'utilisateur a deux options : déverrouiller le compte et conserver le mot de passe actuel, ou définir un nouveau mot de passe.

6.  L'utilisateur doit entrer le nouveau mot de passe à deux reprises, puis le mot de passe est réinitialisé.

#### Accès à l'aide du portail libre-service

1.  Les utilisateurs peuvent ouvrir un navigateur web, accéder au **portail de réinitialisation de mot de passe** , entrer le nom d'utilisateur, puis cliquer sur **Suivant**.

    Si MFA a été configuré, l'utilisateur recevra un appel téléphonique. En arrière-plan, Azure MFA appelle le numéro que l'utilisateur a donné quand il s'est inscrit pour le service.

    Quand l'utilisateur répond au téléphone, il est invité à appuyer sur la touche dièse (#). Ensuite, l'utilisateur clique sur **Suivant** dans le portail.

2.  Si vous avez défini d'autres portes, l'utilisateur est invité à fournir davantage d'informations dans les écrans suivants.

    > [!NOTE]
    > Si l’utilisateur s’impatiente et clique sur **Suivant** avant d’appuyer sur la touche dièse, l’authentification échoue.

3.  L'utilisateur devra choisir s'il souhaite réinitialiser son mot de passe ou déverrouiller son compte. S'il choisit de déverrouiller son compte, le compte sera déverrouillé.

    ![Image du déverrouillage de l’accès avec l’Assistant de connexion MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Après une authentification réussie, l'utilisateur a deux options : conserver son mot de passe actuel ou en définir un nouveau.

5.  ![Image du déverrouillage réussi du compte MIM](media/MIM-SSPR-account-unlock.JPG)

6.  Si l'utilisateur choisit de réinitialiser son mot de passe, il devra taper un nouveau mot de passe à deux reprises et cliquer sur **Suivant** pour changer le mot de passe.

    ![Image de la réinitialisation du mot de passe avec l’Assistant de connexion MIM](media/MIM-SSPR-PR1.JPG)


<!--HONumber=Apr16_HO2-->


