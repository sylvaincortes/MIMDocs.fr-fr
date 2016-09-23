---
title: "Planification d’un environnement bastion | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 09/16/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9eefdf21d0cab3f7c488a66cbb3984d40498f4ef
ms.openlocfilehash: fc4161f98d4367a2124e6253fe11dd1f2712d614


---

# Planification d’un environnement bastion

L’ajout d’un environnement bastion avec une forêt d’administration dédiée à un annuaire Active Directory permet aux organisations de gérer facilement les comptes d’administration, les stations de travail et les groupes dans un environnement qui offre des contrôles de sécurité plus stricts que leur environnement de production existant.

Cette architecture permet plusieurs contrôles qui ne sont pas possibles ou pas faciles à configurer dans une architecture avec une seule forêt. Cela comprend l’approvisionnement de comptes en tant qu’utilisateurs standard sans privilèges dans la forêt d’administration, qui ont des privilèges très élevés dans l’environnement de production, permettant ainsi une meilleure mise en œuvre technique de la gouvernance. Cette architecture permet également d’utiliser la fonctionnalité d’authentification sélective d’une approbation comme un moyen de limiter les ouvertures de session (et l’exposition des informations d’identification) aux seuls hôtes autorisés. Dans les situations où un niveau supérieur de garantie est souhaité pour la forêt de production sans pour autant impliquer le coût et la complexité d’une recréation complète, une forêt d’administration peut fournir un environnement qui augmente le niveau de garantie de l’environnement de production.

Des techniques supplémentaires peuvent être utilisées en plus de la forêt d’administration dédiée. Il s’agit notamment de limiter les endroits où les informations d’identification d’administration sont exposées, de limiter les privilèges des rôles des utilisateurs dans cette forêt et de s’assurer que les tâches d’administration ne sont pas effectuées sur des hôtes utilisés pour les activités utilisateur standard (par exemple l’e-mail et la navigation web).

## Bonnes pratiques préconisées

Une forêt d’administration dédiée est une forêt Active Directory standard à domaine unique utilisée pour la gestion d’Active Directory. L’un des avantages de l’utilisation des domaines et forêts d’administration est qu’on peut leur appliquer davantage de mesures de sécurité que les forêts de production en raison de leurs cas d’usage limités. De plus, comme cette forêt est distincte et qu’elle n’est pas dans une relation d’approbation avec les forêts existantes de l’organisation, si la sécurité de l’une des autres forêts est compromise, cette forêt dédiée ne sera pas affectée.

La conception d’une forêt d’administration doit faire l’objet des considérations suivantes :

### Étendue limitée

La valeur d’une forêt d’administration est le niveau élevé de garantie de sécurité et la surface d’attaque réduite. La forêt peut héberger des fonctions et des applications de gestion supplémentaires, sachant que chaque augmentation de son étendue augmente la surface d’attaque de la forêt et de ses ressources. L’objectif est de limiter les fonctions de la forêt pour minimiser la surface d’attaque.

Conformément au [modèle à niveaux](tier-model-for-partitioning-administrative-privileges.md) de partitionnement des privilèges d’administrateur, les comptes d’une forêt d’administration dédiée doivent être dans un niveau unique, généralement le niveau 0 ou 1. Si une forêt est au niveau 1, restreignez-la à une étendue d’application (par exemple les applications de finance) ou une communauté d’utilisateurs (par exemple les fournisseurs de services informatiques externalisés) spécifique.

### Approbation restreinte

La forêt de production *CORP* doit approuver la forêt d’administration *PRIV*, mais pas l’inverse. Il peut s’agir d’une approbation de domaine ou d’une approbation de forêt. Le domaine de forêt d’administration n’a pas besoin d’approuver les domaines et les forêts gérés pour gérer Active Directory, même si d’autres applications peuvent nécessiter une relation d’approbation bidirectionnelle, la validation de la sécurité et des tests.

L’authentification sélective doit être utilisée pour s’assurer que les comptes de la forêt d’administration utilisent uniquement les hôtes de production appropriés. Pour la gestion des contrôleurs de domaine et la délégation de droits dans Active Directory, ceci nécessite généralement d’accorder le droit « Autorisé à ouvrir une session » pour les contrôleurs de domaine à des comptes d’administrateur désignés de niveau 0 dans la forêt d’administration. Pour plus d’informations, consultez [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc816580.aspx) (Configuration des paramètres de l’authentification sélective).

## Conserver une séparation logique

Pour s’assurer que l’environnement bastion n’est pas affecté par les incidents de sécurité existants ou futurs dans l’annuaire Active Directory de l’organisation, les instructions suivantes doivent être utilisées lors de la préparation des systèmes pour l’environnement bastion :

- Les serveurs Windows Server ne doivent pas être joints au domaine, ni bénéficier de la distribution de logiciels ou de paramètres par l’environnement existant.

- L’environnement bastion doit contenir ses propres services de domaine Active Directory fournissant Kerberos et LDAP, DNS, l’heure et des services de date/heure à l’environnement bastion.

- MIM ne doit pas utiliser de batterie de bases de données SQL dans l’environnement existant. SQL Server doit être déployé sur des serveurs dédiés dans l’environnement bastion.

- L’environnement bastion nécessite Microsoft Identity Manager 2016. Plus spécifiquement, il faut que le Service MIM et les composants PAM soient déployés.

- Le logiciel et les supports de sauvegarde pour l’environnement bastion doivent être conservés séparément de ceux des systèmes des forêts existantes, de façon à ce qu’un administrateur de la forêt existante ne puisse pas compromettre une sauvegarde de l’environnement bastion.

- Les utilisateurs qui gèrent les serveurs de l’environnement bastion doivent se connecter à partir de stations de travail qui ne sont pas accessibles aux administrateurs de l’environnement existant, de façon à ce que les informations d’identification pour l’environnement bastion ne soient pas diffusées.

## Garantir la disponibilité des services d’administration

Comme l’administration des applications est transférée vers l’environnement bastion, vous devez prendre en compte la façon de fournir une disponibilité suffisante pour répondre aux besoins de ces applications. Les techniques sont les suivantes :

- Déployer les services de domaine Active Directory sur plusieurs ordinateurs de l’environnement bastion. Au moins deux sont nécessaire pour assurer le maintien de l’authentification, même si un serveur est redémarré temporairement pour une maintenance planifiée. Des ordinateurs supplémentaires peuvent être nécessaires pour une charge plus élevée ou pour gérer des ressources et des administrateurs dans plusieurs régions géographiques.

- Préparez des comptes de secours dans la forêt existante et dans la forêt d’administration dédiée, pour les cas d’urgence.

- Déployez SQL Server et le service MIM sur plusieurs ordinateurs de l’environnement bastion.

- Conservez une copie de sauvegarde d’Active Directory et de SQL, à effectuer chaque fois qu’il y a des modifications des utilisateurs ou des définitions de rôles dans la forêt d’administration dédiée.

## Configurer des autorisations Active Directory appropriées

La forêt d’administration doit être configurée avec des privilèges minimum, en fonction de la configuration requise pour l’administration d’Active Directory.

- Les comptes de la forêt d’administration utilisés pour administrer l’environnement de production ne doivent pas recevoir de privilèges d’administration pour la forêt d’administration, ni pour les domaines et les stations de travail qu’elle contient.

- Les privilèges d’administration sur la forêt d’administration elle-même doivent être étroitement contrôlés par un processus hors connexion, afin de réduire la possibilité qu’un attaquant ou un utilisateur interne malveillant efface les journaux d’audit. Ceci limite la possibilité que le personnel utilisant des comptes d’administration de production assouplisse les restrictions sur leurs comptes, augmentant ainsi les risques pour l’organisation.

- La forêt d’administration doit respecter les configurations de Microsoft Security Compliance Manager (SCM) pour le domaine, notamment les configurations renforcées des protocoles d’authentification.

Quand vous créez l’environnement bastion, avant d’installer Microsoft Identity Manager, identifiez et créez les comptes qui seront utilisés pour l’administration au sein de cet environnement. Ceci comprend les comptes suivants :

- Les **comptes de secours** doivent seulement pouvoir se connecter aux contrôleurs de domaine de l’environnement bastion.

- Les **administrateurs « carton rouge »** configurent d’autres comptes et effectuent la maintenance non planifiée. Aucun accès aux forêts existantes ou à des systèmes en dehors de l’environnement bastion n’est fourni à ces comptes. Les informations d’identification, par exemple les cartes à puce, doivent être physiquement sécurisées, et l’utilisation de ces comptes doit être journalisée.

- **Comptes de service** requis par Microsoft Identity Manager, SQL Server et d’autres logiciels.

## Renforcer les hôtes

Tous les hôtes, notamment les contrôleurs de domaine, les serveurs et les stations de travail jointes à la forêt d’administration, doivent avoir les dernières versions des systèmes d’exploitation et des service packs installées et conservées à jour en permanence.

- les applications nécessaires à l’administration doivent être préinstallées sur les stations de travail, afin que les comptes qui les utilisent ne doivent pas faire partie du groupe des administrateurs locaux pour les installer. La maintenance du contrôleur de domaine peut généralement être effectuée avec RDP et les outils d’administration de serveur distant.

- Les hôtes de la forêt d’administration doivent être automatiquement mis à jour avec les mises à jour de sécurité. Si cette configuration peut aboutir à un risque d’interruption des opérations de maintenance du contrôleur de domaine, elle permet également une limitation importante des risques de sécurité liés à des vulnérabilités non corrigées.

### Identifier les hôtes d’administration

Le risque d’un système ou d’une station de travail doit être mesuré par l’activité la plus risquée exécutée sur celui-ci, comme la navigation Internet, l’envoi et la réception d’e-mails, ou l’utilisation d’autres applications traitant du contenu inconnu ou non approuvé.

Les hôtes d’administration peuvent être les ordinateurs suivants :

- un ordinateur de bureau sur lequel les informations d’identification de l’administrateur sont physiquement tapées ou entrées ;

- des « serveurs d’administration ponctuels » sur lesquels des sessions et des outils d’administration sont exécutés ;

- tous les hôtes sur lesquels des actions d’administration sont effectuées, y compris ceux qui utilisent l’ordinateur de bureau d’un utilisateur standard exécutant un client RDP pour administrer à distance des serveurs et des applications ;

- des serveurs hébergeant des applications qui doivent être administrées et qui ne sont pas accessibles via RDP avec le mode d’administration restreint ou l’accès à distance Windows PowerShell.

### Déployer des stations de travail d’administration dédiées

Bien que cela soit peu pratique, la séparation des stations de travail renforcées dédiées aux utilisateurs ayant des informations d’identification administratives à fort impact peut être nécessaire. Il est important de fournir à un hôte un niveau de sécurité égal ou supérieur au niveau des privilèges accordés aux informations d’identification. Incorporez les mesures suivantes pour une protection supplémentaire :

- **Vérifiez l’intégrité de tous les supports de la build** pour limiter la possibilité de présence de programmes malveillants installés dans une image principale ou injectés dans un fichier d’installation pendant le téléchargement ou le stockage.

- Vous devez utiliser des **lignes de base de sécurité** comme configurations de démarrage. Microsoft Security Compliance Manager (SCM) peut aider à configurer les lignes de base sur les hôtes d’administration.

- **Démarrage sécurisé** pour atténuer les risques face à des attaquants ou des programmes malveillants tentant de charger du code non signé dans le processus de démarrage.

- **Restrictions logicielles** pour s’assurer que seuls des logiciels d’administration autorisés sont exécutés sur les hôtes d’administration. Les clients peuvent utiliser AppLocker pour cette tâche, avec une liste des applications autorisées, pour empêcher l’exécution de logiciels malveillants et d’applications non prises en charge.

- **Chiffrement intégral des volumes** pour limiter les risques liés à la perte physique d’ordinateurs, comme des ordinateurs portables d’administration utilisés à distance.

- **Restrictions USB** pour protéger contre les infections physiques.

- **Isolement du réseau** pour protéger contre les attaques réseau et les actions d’administration inappropriées effectuées par inadvertance. Les pare-feu des hôtes doivent bloquer toutes les connexions entrantes, sauf celles qui sont explicitement requises, et bloquer tout accès Internet sortant non nécessaire.

- **Logiciel anti-programme malveillant** pour protéger contre les menaces et les programmes malveillants connus.

- **Limitations des exploits** pour limiter les risques face aux menaces et aux exploits inconnus, notamment le kit d’outils EMET (Enhanced Mitigation Experience Toolkit).

- **Analyse de la surface des attaques** pour empêcher l’introduction de nouveaux vecteurs d’attaque dans Windows lors de l’installation de nouveaux logiciels. Des outils comme Attack Surface Analyzer (ASA) permettent d’évaluer les paramètres de configuration sur un hôte et d’identifier les vecteurs d’attaque introduits par des modifications logicielles ou de configuration.

- Vous ne devez pas accorder de **privilèges d’administrateur** aux utilisateurs sur leur ordinateur local.

- **Mode RestrictedAdmin** pour les sessions RDP sortantes, sauf si un autre mode est requis par le rôle. Pour plus d’informations, consultez [Nouveautés des services Bureau à distance dans Windows Server](https://technet.microsoft.com/library/dn283323.aspx).

Certaines de ces mesures peuvent sembler extrême, mais les révélations publiques de ces dernières années ont montré les capacités importantes et les dons des adversaires pour compromettre des cibles.

## Préparer les domaines existants à la gestion par l’environnement bastion

MIM utilise des applets de commande PowerShell pour établir une relation d’approbation entre les domaines Active Directory existants et la forêt d’administration dédiée de l’environnement bastion. Une fois l’environnement bastion déployé et avant la conversion au juste-à-temps (JIT) des utilisateurs ou des groupes, les applets de commande `New-PAMTrust` et `New-PAMDomainConfiguration` mettent à jour les relations d’approbation de domaine et créent les artefacts nécessaires pour Active Directory et MIM.

Quand la topologie Active Directory existante est modifiée, les applets de commande `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` et `Remove-PAMDomainConfiguration` peuvent être utilisées pour mettre à jour les relations d’approbation.

## Établir l’approbation pour chaque forêt

L’applet de commande `New-PAMTrust` doit être exécutée une fois pour chaque forêt existante. Elle est appelée sur l’ordinateur du service MIM dans le domaine d’administration. Les paramètres de cette commande sont le nom de domaine du domaine de plus haut niveau de la forêt existante et les informations d’identification d’un administrateur de ce domaine.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Après avoir établi l’approbation, configurez chaque domaine pour activer la gestion depuis l’environnement bastion, comme décrit dans la section suivante.

## Activer la gestion de chaque domaine

Sept spécifications sont requises pour l’activation de la gestion pour un domaine existant.

### 1. Un groupe de sécurité sur le domaine local

Il doit y avoir un groupe dans le domaine existant, dont le nom est le nom de domaine NetBIOS suivi de trois signes dollar, par exemple *CONTOSO$$$*. L’étendue du groupe doit être *domaine local* et le type de groupe doit être *Sécurité*. Ceci est nécessaire pour que les groupes soient créés dans la forêt d’administration dédiée avec le même identificateur de sécurité que les groupes de ce domaine. Créez ce groupe avec la commande PowerShell suivante, exécutée par un administrateur du domaine existant sur une station de travail jointe au domaine existant :

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### 2. Audit des succès et des échecs

Les paramètres de stratégie de groupe pour l’audit sur le contrôleur de domaine doit inclure l’audit des actions ayant réussi et échoué pour Auditer la gestion des comptes et pour Auditer l’accès au service d’annuaire. Cela peut être fait avec la Console de gestion des stratégies de groupe, exécutée par un administrateur du domaine existant, et exécutée sur une station de travail jointe au domaine existant :

3. Accédez à **Démarrer** > **Outils d’administration** > **Gestion des stratégies de groupe**.

4. Accédez à **Forêt : contoso.local** > **Domaines** > **contoso.local** > **Contrôleurs de domaine** > **Stratégie des contrôleurs de domaine par défaut**. Un message d'information s'affiche.

    ![Stratégie des contrôleurs de domaine par défaut - capture d’écran](media/pam-group-policy-management.jpg)

5. Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut** et sélectionnez **Modifier**. Une nouvelle fenêtre s'affiche.

6. Dans la fenêtre Éditeur de gestion des stratégies de groupe, sous l’arborescence Stratégie des contrôleurs de domaine par défaut, accédez à **Configuration de l’ordinateur** > **Stratégies** > **Paramètres Windows** > **Paramètres de sécurité** > **Stratégies locales** > **Stratégie d’audit**.

    ![Éditeur de gestion des stratégies de groupe - capture d’écran](media/pam-group-policy-management-editor.jpg)

5. Dans le volet d’informations, cliquez avec le bouton droit sur **Auditer la gestion des comptes**, puis sélectionnez **Propriétés**. Sélectionnez **Définir ces paramètres de stratégie**, cochez la case **Réussite**, cochez la case **Échec**, puis cliquez sur **Appliquer** et sur **OK**.

6. Dans le volet d’informations, cliquez avec le bouton droit sur **Auditer l’accès au service d’annuaire**, puis sélectionnez **Propriétés**. Sélectionnez **Définir ces paramètres de stratégie**, cochez la case **Réussite**, cochez la case **Échec**, puis cliquez sur **Appliquer** et sur **OK**.

    ![Paramètres de stratégie de réussite et d’échec - capture d’écran](media/pam-group-policy-management-editor2.jpg)

7. Fermez les fenêtres Éditeur de gestion de stratégie de groupe et Gestion des stratégies de groupe. Appliquez ensuite les paramètres d'audit en lançant une fenêtre PowerShell et en tapant :

    ```
    gpupdate /force /target:computere
    ```

Le message « La mise à jour de la stratégie d'ordinateur s'est terminée sans erreur » doit apparaître après quelques minutes.

### 3. Autoriser les connexions à l’Autorité de sécurité locale

Les contrôleurs de domaine doivent autoriser les connexions RPC sur TCP/IP pour l’Autorité de sécurité locale (LSA) à partir de l’environnement bastion. Dans les versions antérieures de Windows Server, la prise en charge de TCP/IP dans LSA doit être activée dans le Registre :

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### 4. Créer la configuration de domaine PAM

L’applet de commande `New-PAMDomainConfiguration` doit être exécutée sur l’ordinateur du service MIM dans le domaine d’administration. Les paramètres de cette commande sont le nom de domaine du domaine existant et les informations d’identification d’un administrateur de ce domaine.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### 5. Accorder des autorisations de lecture aux comptes

Les comptes de la forêt bastion utilisés pour établir des rôles (administrateurs qui utilisent les applets de commande `New-PAMUser` et `New-PAMGroup` ), ainsi que le compte utilisé par le service de surveillance MIM, ont besoin des autorisations de lecture dans ce domaine.

Les étapes suivantes permettent d’accorder à l’utilisateur *PRIV\Administrateur* un accès en lecture au domaine *Contoso* dans le contrôleur de domaine *CORPDC* :

1. Assurez-vous d’être connecté à CORPDC comme administrateur du domaine Contoso (comme Contoso\Administrateur).

2. Lancez Utilisateurs et ordinateurs Active Directory.

3. Cliquez avec le bouton droit sur le domaine **contoso.local**, puis sélectionnez **Délégation de contrôle**.

4. Sous l’onglet Utilisateurs et groupes sélectionnés, cliquez sur **Ajouter**.

5. Dans la fenêtre contextuelle Sélectionner les utilisateurs, les ordinateurs ou les groupes, cliquez sur **Emplacements** et remplacez l’emplacement par *priv.contoso.local*. Pour le nom d’objet, tapez *Admins du domaine* et cliquez sur **Vérifier les noms**. Quand une fenêtre contextuelle s’affiche, tapez *priv\administrateur* comme nom d’utilisateur et entrez le mot de passe.

6. Après Admins du domaine, tapez *; MIMMonitor*. Une fois les noms Admins du domaine et MIMMonitor soulignés, cliquez sur **OK**, puis sur **Suivant**.

7. Dans la liste des tâches courantes, sélectionnez **Lire toutes les informations sur l’utilisateur**, cliquez sur **Suivant**, puis sur **Terminer**.

18. Fermez Utilisateurs et ordinateurs Active Directory.

### 6. Un compte de secours

Si l’objectif du projet de gestion des accès privilégiés est de réduire le nombre de comptes avec des privilèges d’administrateur de domaine affectés en permanence au domaine, il doit y avoir un compte *de secours* dans le domaine, au cas où un problème surviendrait avec la relation d’approbation. Les comptes pour les accès d’urgence à la forêt de production doivent exister dans chaque domaine, et ils ne doivent être en mesure de se connecter à des contrôleurs de domaine. Pour les organisations avec plusieurs sites, des comptes supplémentaires peuvent être nécessaires pour assurer la redondance.

### 7. Mettre à jour les autorisations dans l’environnement bastion

Vérifiez les autorisations sur l’objet *AdminSDHolder* dans le conteneur Système de ce domaine. L’objet *AdminSDHolder* a une liste de contrôle d’accès (ACL) unique, qui est utilisée pour contrôler les autorisations des principaux de sécurité qui sont membres des groupes Active Directory privilégiés intégrés. Vérifiez si des modifications ont été apportées aux autorisations par défaut qui auraient un impact sur les utilisateurs avec des privilèges d’administration dans le domaine, car ces autorisations ne s’appliquent pas aux utilisateurs dont le compte est dans l’environnement bastion.

## Sélectionner des utilisateurs et des groupes à inclure

L’étape suivante consiste à définir les rôles PAM, en associant les utilisateurs et les groupes auxquels ils doivent avoir accès. Il s’agit généralement d’un sous-ensemble d’utilisateurs et de groupes pour le niveau identifié comme étant géré dans l’environnement bastion. Pour plus d’informations, consultez [Définition de rôles pour Privileged Access Management](defining-roles-for-pam.md).



<!--HONumber=Sep16_HO3-->


