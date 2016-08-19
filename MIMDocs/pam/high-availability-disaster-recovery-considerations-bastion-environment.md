---
title: "Récupération d’urgence PAM | Microsoft Identity Manager"
description: "Découvrez comment configurer Privileged Access Management pour une haute disponibilité et la récupération d’urgence."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 9164e48bf10fa27ff6c87ba3816b586a940dda69


---

# Considérations relatives à la haute disponibilité et à la récupération d’urgence pour l’environnement bastion
Cet article décrit les considérations relatives à la haute disponibilité et à la récupération d’urgence lors du déploiement des services de domaine Active Directory (AD DS) et de Microsoft Identity Manager 2016 (MIM) pour Privileged Access Management (PAM).

Les entreprises se focalisent sur la haute disponibilité et la récupération d’urgence des charges de travail dans Windows Server, SQL Server et Active Directory. Toutefois, la fiabilité de la disponibilité de l’environnement bastion pour Privileged Access Management est également importante. L’environnement bastion est une partie essentielle de l’infrastructure informatique de l’organisation, car les utilisateurs interagissent avec ses composants afin d’assumer des rôles d’administration. Pour plus d’informations sur la haute disponibilité en général, vous pouvez télécharger le livre blanc intitulé [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc) (Vue d’ensemble de la haute disponibilité Microsoft).

## Scénarios de haute disponibilité et de récupération d’urgence

Lors de la planification de la haute disponibilité et de la récupération d’urgence, vous devez prendre en compte les questions suivantes :

- Quelles sont les fonctions qui pourraient être affectées par une panne ?
- Quelles sont les fonctions métier qui sont critiques pour l’activité commerciale et/ou pour les opérations informatiques ?
- Quels sont les risques qui pourraient entraîner une panne dans ces systèmes ?

L’étendue de ces considérations ayant un impact sur le coût total de déploiement et d’exploitation, les organisations peuvent accorder à certaines fonctions une priorité plus élevée et accepter les risques de pannes temporaires pour les fonctions de priorité inférieure. Le tableau suivant présente un classement de priorités possible :

| **Fonction de forêt bastion** | **Priorité relative lors de la récupération** | **Atténuation si la fonction est indisponible** |
| --------------------------- | --------------------- | -------------- |
| Établissement d’approbation         | Faible | Patienter jusqu’à ce que l’environnement bastion soit restauré |
| Migration d’utilisateurs et de groupes   | Faible | Patienter jusqu’à ce que l’environnement bastion soit restauré |
| Administration MIM          | Faible | Patienter jusqu’à ce que l’environnement bastion soit restauré |
| Activation de rôle privilégié  | Moyenne | Comptes dédiés reposant sur une carte à puce pour ajouter manuellement des utilisateurs aux groupes d’administration |
| Gestion des ressources         | Importante | Comptes dédiés reposant sur une carte à puce pour ajouter manuellement des utilisateurs aux groupes d’administration |
| Surveillance des utilisateurs et des groupes dans une forêt existante | Faible | Patienter jusqu’à ce que l’environnement bastion soit restauré |

Examinons maintenant chacune de ces fonctions de la forêt bastion.

### Établissement d’approbation

Il doit y avoir une approbation de forêt entre les domaines de la forêt existante et la forêt de l’environnement bastion pour que les utilisateurs qui s’authentifient dans l’environnement bastion puissent administrer des ressources dans les forêts existantes. Une configuration supplémentaire peut être nécessaire, par exemple pour autoriser la migration d’utilisateurs de domaines existants sur des versions antérieures de Windows Server.

L’établissement de l’approbation exige que les contrôleurs de domaine de forêt existants soient en ligne, de même que les composants MIM et Active Directory de l’environnement bastion.  En cas de panne de l’un de ces composants durant l’établissement de l’approbation, l’administrateur peut réessayer une fois que la panne a été résolue.  En cas de récupération des contrôleurs de domaine de forêt existants ou de l’environnement bastion après une panne, MIM propose également les applets de commande PowerShell `Test-PAMTrust` et `Test-PAMDomainConfiguration`, que vous pouvez utiliser pour vérifier qu’une approbation est toujours en place.

### Migration d’utilisateurs et de groupes

Une fois l’approbation établie, vous pouvez créer des groupes d’ombres dans l’environnement bastion, ainsi que des comptes d’utilisateur pour les membres de ces groupes et les approbateurs. Ainsi, ces utilisateurs peuvent activer des rôles privilégiés et ré-obtenir des appartenances de groupes effectives.

La migration d’utilisateurs et de groupes exige que les contrôleurs de domaine de forêt existants soient en ligne, de même que les composants MIM et Active Directory de l’environnement bastion.   Si les contrôleurs de domaine de forêt existantes sont inaccessibles, aucun utilisateur ou groupe supplémentaire ne peut être ajouté à l’environnement bastion, mais les groupes et les utilisateurs existants ne sont pas affectés. Si une panne de l’un des composants se produit lors de la migration, l’administrateur peut réessayer une fois la panne résolue.

### Administration MIM
Une fois les utilisateurs et les groupes migrés, un administrateur peut configurer dans MIM les attributions de rôles liant les utilisateurs comme candidats à l’activation dans les rôles.  Ils peuvent également configurer les stratégies MIM pour l’approbation et Azure MFA.  

L’administration MIM exige que les composants MIM et Active Directory de l’environnement bastion soient en ligne.

### Activation de rôle privilégié
Quand un utilisateur souhaite activer un rôle privilégié, il doit s’authentifier auprès du domaine de l’environnement bastion et soumettre une demande à MIM.  MIM inclut des API SOAP et REST, ainsi que des interfaces utilisateur dans PowerShell et dans une page web.

L’activation de rôles privilégiés exige que les composants MIM et Active Directory de l’environnement bastion soient en ligne.  En outre, si MIM est configuré pour utiliser [Azure MFA pour l’activation](use-azure-mfa-for-activation.md) du rôle sélectionné, un accès à Internet est nécessaire pour contacter le service Azure MFA.

### Gestion des ressources
Une fois qu’un utilisateur a été activé dans le rôle, le contrôleur de domaine peut générer pour lui un ticket Kerberos utilisable par les contrôleurs de domaine dans les domaines existants, et il reconnaît les nouvelles appartenances de groupe temporaires de l’utilisateur.

La gestion des ressources exige qu’un contrôleur de domaine pour le domaine de ressources soit en ligne, ainsi qu’un contrôleur de domaine dans l’environnement bastion.  Une fois qu’un utilisateur est activé, il n’est pas obligatoire que MIM ou SQL soit en ligne dans l’environnement bastion pour que son ticket Kerberos soit émis.  (Notez qu’avec Windows Server 2012 R2 comme niveau fonctionnel pour l’environnement bastion, MIM doit être en ligne pour mettre fin à l’appartenance au groupe temporaire).

### Surveillance des utilisateurs et des groupes dans la forêt existante
MIM inclut également un service de surveillance PAM qui vérifie régulièrement les utilisateurs et les groupes dans les domaines existants, et qui met à jour la base de données MIM et Active Directory en conséquence.  Il n’est pas obligatoire que ce service soit en ligne pour l’activation de rôle ou lors de la gestion des ressources.

La surveillance exige que les contrôleurs de domaine de forêt existants soient en ligne, de même que les composants MIM et Active Directory de l’environnement bastion.  

## Options de déploiement
La [Vue d’ensemble de l’environnement](environment-overview.md) illustre une topologie de base qui permet de découvrir la technologie mais qui n’est pas destinée à la haute disponibilité. Cette section décrit comment étendre cette topologie pour fournir une haute disponibilité pour les organisations ayant un seul site et pour celles disposant de plusieurs sites existants.

### Mise en réseau

Le trafic réseau entre les ordinateurs de l’environnement bastion doit être isolé des réseaux existants, par exemple en utilisant un autre réseau physique ou virtuel.  En fonction des risques pour l’environnement bastion, il peut également être nécessaire d’avoir des interconnexions physiques indépendantes entre les ordinateurs.  Certaines technologies de cluster de basculement ont des exigences supplémentaires en ce qui concerne les interfaces réseau.

Les ordinateurs hébergeant les services de domaine Active Directory (AD DS) et ceux hébergeant les Services MIM dans l’environnement bastion nécessitent une connectivité bidirectionnelle aux ressources dans la forêt existante pour que :
- Les utilisateurs soient authentifiés par les contrôleurs de domaine de forêt PRIV.
- Les utilisateurs demandent l’activation.
- Les utilisateurs disposent de tickets Kerberos consommables par les ressources dans la forêt existante.
- MIM puisse surveiller les domaines de la forêt existants.
- MIM puisse envoyer des e-mails par le biais de serveurs e-mail situés dans la forêt existante.

### Topologies de haute disponibilité minimales
Une organisation peut sélectionner les fonctions dans son environnement bastion qui nécessitent une haute disponibilité, avec les contraintes suivantes :

- La haute disponibilité pour toute fonction fournie par l’environnement bastion exige au moins deux contrôleurs de domaine.  
- La haute disponibilité pour les demandes d’activation exige au moins deux ordinateurs hébergeant le Service MIM, ainsi qu’une haute disponibilité pour l’ordinateur SQL Server.
- La haute disponibilité de SQL Server avec des clusters de basculement exige au moins deux serveurs fournissant SQL Server, qui ne soient pas contrôleurs de domaine.
- Le Service MIM ne doit pas être installé sur le contrôleur de domaine afin de réduire la surface d’attaque de chaque serveur.

La plus petite topologie de haute disponibilité pour toutes les fonctions dans un environnement bastion comprend au moins quatre serveurs et un stockage partagé. Deux des serveurs doivent être configurés comme contrôleurs de domaine, fournissant les services de domaine Active Directory. Les deux autres serveurs peuvent être configurés comme cluster de basculement fournissant SQL Server, et fournir le Service MIM.

De plus, un déploiement classique de l’environnement bastion comprend également une station de travail d’administration privilégiée pour la gestion de ces serveurs, ainsi qu’un composant de surveillance.

Le diagramme suivant illustre une architecture possible :

![topologie bastion - diagramme](media/bastion1.png)

Vous pouvez configurer des serveurs supplémentaires pour chacune de ces fonctions, pour obtenir de meilleures performances dans des conditions de charge ou à des fins de redondance géographique comme décrit ci-dessous.

### Déploiements prenant en charge plusieurs sites
Le choix de la topologie de déploiement pour les ressources déployées sur plusieurs sites dépend de trois facteurs :  
- Objectifs et risques pour la haute disponibilité et la récupération d’urgence.  
- Capacité matérielle pour l’hébergement de l’environnement bastion.  
- Modèle de travail d’administration pour chaque site.

L’une des approches les plus simples consiste à héberger l’environnement bastion sur un site spécifique.  Dans des conditions normales, les utilisateurs se connectent au déploiement MIM dans l’environnement bastion de ce site et demandent l’activation, qui s’appliquera aux ressources de chaque site.  Dans le cas où la liaison réseau est interrompue ou le site hébergeant l’environnement bastion est indisponible, les informations d’identification hors connexion peuvent être accessibles à un autre site, afin d’effectuer une administration temporaire jusqu’à ce que le réseau soit reconnecté.  Cette approche peut convenir aux situations où il est prévu que l’administration locale d’un site particulier (par exemple une filiale) soit rare et limitée à la reconnexion de ce site au reste du réseau d’une organisation.

![Bastion unique pour la topologie multisite - diagramme](media/bastion2.png)

Pour la haute disponibilité et la récupération d’urgence entre sites, vous pouvez aussi déployer les composants de l’environnement bastion dans chaque site, ceux-ci partageant un annuaire PRIV commun et une base de données SQL commune.  Dans cette topologie, si la liaison réseau est interrompue, les utilisateurs de chaque site peuvent continuent à travailler indépendamment.

![Bastion multiple pour topologie multisite - diagramme](media/bastion3.png)

L’une des contraintes de cette approche de déploiement est que SQL Server exige un cluster qui s’étend sur les deux sites, ce qui peut être complexe à déployer. Dans ce cas, envisagez comme alternative de répliquer uniquement l’annuaire Active Directory (la forêt PRIV) de l’environnement bastion.  En cas de défaillance du réseau entre les sites, les utilisateurs du site B ayant déjà activé leurs rôles privilégiés pourront continuer à administrer les ressources du site B.

![Bastion répliqué pour topologie multisite - diagramme](media/bastion4.png)

Si chaque site représente une limite administrative distincte, vous pouvez aussi déployer plusieurs environnements bastion indépendants.  Chaque environnement bastion aurait les mêmes logiciels, mais leurs noms de domaine seraient différents et les annuaires et les bases de données de chaque environnement bastion ne seraient pas communs. Un utilisateur souhaitant gérer les ressources d’un site particulier activerait un compte d’utilisateur dans l’environnement bastion dans ce site.

![Bastions indépendants pour topologie multisite - diagramme](media/bastion5.png)

Pour finir, des déploiements plus complexes sont possibles, car plusieurs environnements bastions peuvent être configurés indépendamment pour gérer les ressources dans un domaine particulier.

![Bastion complexe pour topologie multisite - diagramme](media/bastion6.png)

### Environnement bastion hébergé
Certaines organisations ont également pris en compte l’établissement d’un environnement bastion séparé de tous leurs sites existants. Le logiciel d’environnement bastion peut être hébergé sur une plateforme de virtualisation sur les réseaux de l’entreprise ou chez un fournisseur d’hébergement.  Lors de l’évaluation de cette approche, n’oubliez pas ce qui suit :

- Pour vous protéger contre les attaques provenant des domaines existants, l’administration de l’environnement bastion doit être isolée des comptes d’administration du domaine existant.
- L’environnement bastion nécessite une connectivité TCP/IP aux contrôleurs de domaine dans le domaine existant.  Vous trouverez une liste des ports dans [Comment faire pour configurer un pare-feu pour les domaines et les approbations](https://support.microsoft.com/kb/179442).
- Un déploiement virtualisé des services de domaine Active Directory nécessite des fonctionnalités spécifiques de la plateforme de virtualisation, comme décrit dans [Déploiement et configuration des contrôleurs de domaine virtualisés](https://technet.microsoft.com/library/jj574223.aspx).
- Un déploiement à haute disponibilité de SQL Server pour le Service MIM nécessite une configuration de stockage spécialisée, décrite dans la section [Stockage de base de données SQL Server](#sql-server-database-storage) ci-dessous.  Il est possible que les fournisseurs d’hébergement n’offrent pas tous actuellement un hébergement Windows Server avec des configurations de disques convenant aux clusters de basculement SQL Server.

## Préparation du déploiement et procédures de récupération
La préparation d’un déploiement de l’environnement bastion à haute disponibilité ou prêt à la récupération d’urgence nécessite la prise en considération du mode d’installation de Windows Server Active Directory, de SQL Server et de sa base de données sur le stockage partagé, ainsi que du Service MIM et de ses composants PAM.

### Windows Server
Windows Server contient une fonctionnalité intégrée pour la haute disponibilité permettant à plusieurs ordinateurs de travailler ensemble en tant que cluster de basculement. Les serveurs en cluster sont reliés par des câbles physiques et des logiciels. En cas de défaillance d’un ou plusieurs nœuds, d’autres nœuds prennent le relais pour fournir les services requis (processus appelé « basculement »).   Vous trouverez plus de détails dans [Vue d’ensemble du clustering de basculement](https://technet.microsoft.com/library/hh831579.aspx).

Vérifiez que le système d’exploitation et les applications dans l’environnement bastion reçoivent les mises à jour pour les problèmes de sécurité. Certaines de ces mises à jour peuvent nécessiter un redémarrage du serveur. Vous devez donc coordonner les horaires d’application des mises à jour sur les serveurs pour éviter toute interruption prolongée. Une approche consiste à utiliser la [mise à jour adaptée aux clusters](https://technet.microsoft.com/library/hh831694.aspx) pour les serveurs dans un cluster de basculement Windows Server.

Les serveurs de l’environnement bastion seront joints à un domaine et dépendants des services de domaine. Vérifiez qu’ils ne sont pas configurés par inadvertance avec une dépendance envers un contrôleur de domaine particulier pour les services tels que DNS.

### Active Directory dans l’environnement bastion
Les services de domaine Active Directory Windows Server prennent en charge la haute disponibilité et la récupération d’urgence en mode natif.

#### Préparation
Un déploiement de production de gestion des accès privilégiés classique comprend au moins deux contrôleurs de domaine dans l’environnement bastion. Des instructions de configuration du premier contrôleur de domaine dans l’environnement bastion sont fournies à l’étape 2 des articles sur le déploiement, [Préparer le contrôleur de domaine PRIV](step-2-prepare-priv-domain-controller.md).

Vous trouverez la procédure d’ajout d’un contrôleur de domaine supplémentaire dans [Installer un contrôleur de domaine Windows Server 2012 répliqué dans un domaine existant (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Si le contrôleur de domaine doit être hébergé sur une plateforme de virtualisation comme Hyper-V, passez en revue les avertissements dans [Déploiement et configuration des contrôleurs de domaine virtualisés](https://technet.microsoft.com/library/jj574223.aspx).

#### Récupération
Après une panne, vérifiez qu’au moins un contrôleur de domaine est disponible dans l’environnement bastion avant de redémarrer les autres serveurs.

Dans un domaine, Active Directory distribue les rôles FSMO (Flexible Single Master opération) parmi les contrôleurs de domaine, comme décrit dans [How Operations Masters Work (Fonctionnement des maîtres d’opérations)](https://technet.microsoft.com/library/cc780487.aspx).  Si un contrôleur de domaine a échoué, il peut être nécessaire de transférer un ou plusieurs des [rôles de contrôleur de domaine](https://technet.microsoft.com/library/cc786438.aspx) auxquels ce contrôleur de domaine a été affecté.

Après avoir déterminé qu’un contrôleur de domaine ne retournerait pas en production, vérifiez si des rôles ont été assignés à ce contrôleur de domaine et réattribuez-les si nécessaire. Vous trouverez des instructions dans [View the Current Operations Master Role Holde (Afficher les détenteurs actuels du rôle de maître d’opérations)](https://technet.microsoft.com/library/cc816893.aspx) et ses articles connexes.

Nous vous recommandons également de vérifier les paramètres DNS des ordinateurs joints à l’environnement bastion, ainsi que les contrôleurs de domaine dans les domaines CORP qui ont une relation d’approbation avec ce contrôleur de domaine, pour vous assurer qu’aucun d’entre eux n’est codé en dur avec une dépendance envers l’adresse IP de cet ordinateur contrôleur de domaine.

### Stockage de base de données SQL Server
Un déploiement à haute disponibilité nécessite des clusters de basculement SQL Server, et les instances de cluster de basculement SQL Server reposent sur un stockage partagé entre tous les nœuds pour le stockage des journaux et des bases de données. Le stockage partagé peut être constitué de disques de cluster Clustering de basculement Windows Server, de disques sur un réseau de zone de stockage (SAN) ou de partages de fichiers sur un serveur SMB.  Notez qu’il doivent être dédiés à l’environnement bastion. Le partage du stockage avec d’autres charges de travail en dehors de l’environnement bastion n’est pas recommandé, car cela peut mettre en danger l’intégrité de l’environnement bastion.

### SQL Server
Le Service MIM nécessite un déploiement de SQL Server dans l’environnement bastion.   Pour la haute disponibilité, SQL peut être déployé à l’aide d’une instance de cluster de basculement (FCI, Failover Cluster Instance). Contrairement aux instances autonomes, dans les instances FCI la haute disponibilité de SQL Server est protégée par la présence de nœuds redondants dans l’instance FCI. En cas de défaillance ou de mise à niveau planifiée, la propriété du groupe de ressources est déplacée vers un autre nœud de cluster de basculement Windows Server.

Si vous avez besoin de la prise en charge de la récupération d’urgence mais pas de la haute disponibilité, vous pouvez utiliser la copie des journaux de transaction, la réplication des transactions, la réplication d’instantané ou la mise en miroir de bases de données plutôt que le clustering de basculement.   

#### Préparation
Quand vous installez SQL Server dans l’environnement bastion, il doit être indépendant de tout ordinateur SQL Server déjà présent dans les forêts CORP.  De plus, nous vous recommandons de déployer le serveur SQL Server sur un serveur dédié, distinct de celui du contrôleur de domaine.
Vous trouverez plus d’informations dans le guide SQL Server des [Instances de cluster de basculement AlwaysOn](https://msdn.microsoft.com/library/ms189134.aspx).

#### Récupération
Si SQL Server a été configuré pour la récupération d’urgence à l’aide de la copie des journaux de transaction, vous devez prendre des mesures pour mettre à jour SQL Server lors de la récupération.  En outre, le redémarrage de chaque instance du Service MIM est nécessaire.

Si SQL Server a échoué ou que la connectivité entre SQL Server et le Service MIM a été perdue, il est recommandé de redémarrer chaque Service MIM après la restauration de SQL Server.  Cela garantit que le Service MIM rétablit sa connexion à SQL Server.

### Service MIM
Le Service MIM est nécessaire pour traiter les demandes d’activation.  Pour qu’un ordinateur qui héberge le Service MIM puisse être mis hors service à des fins de maintenance alors que des demandes d’activation sont toujours en cours de réception, vous pouvez déployer plusieurs ordinateurs exécutant le Service MIM.  Notez que le Service MIM ne participe pas aux opérations Kerberos une fois qu’un utilisateur a été ajouté à un groupe.  

#### Préparation
Nous vous recommandons de déployer le Service MIM sur plusieurs serveurs joints au domaine PRIV.
Pour la haute disponibilité, consultez la documentation de Windows Server relative à la [Configuration matérielle requise pour le clustering de basculement et options de stockage](https://technet.microsoft.com/library/jj612869.aspx) et à la [Création d’un cluster de basculement Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx).

Pour le déploiement de production sur plusieurs serveurs, vous pouvez utiliser l’équilibrage de la charge réseau (NLB) pour distribuer la charge de traitement.  Vous devez également avoir un alias unique (par exemple, enregistrement A ou CNAME) pour qu’un nom commun soit exposé à l’utilisateur.

>[!IMPORTANT]
> Si vous utilisez une technologie d’équilibrage de la charge autre que la fonctionnalité d’équilibrage de la charge réseau dans Windows Server 2012 R2, vérifiez que votre solution redirige une session vers le même serveur et non vers un serveur aléatoire.

Dans un déploiement MIM à plusieurs serveurs, chaque Service MIM a un nom d’hôte externe, un nom de service et un nom de partition de service.  La valeur par défaut du nom de service est le nom de l’ordinateur, et la valeur par défaut du nom d’hôte externe et du nom de partition de service est configurée pendant l’installation du Service MIM dans la page qui vous invite à spécifier l’adresse du serveur du Service MIM. Ces trois noms sont stockés dans le fichier %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config en tant qu’attributs `externalHostName`, `serviceName` et `servicePartitionName` du nœud de configuration `resourceManagementService`.  

Quand un Service MIM reçoit une demande, le nom de partition de service est stocké en tant qu’attribut sur cette demande.   Par la suite, seules les autres installations de Service MIM ayant le même nom de partition de service sont autorisées à interagir avec cette demande.  Ainsi, si le scénario PAM comprend des approbations manuelles ou d’autres traitements de demande à long terme, vérifiez que chaque Service MIM a le même attribut `servicePartitionName` dans ce fichier de configuration.

#### Récupération
Après une panne, vérifiez qu’au moins un contrôleur de domaine Active Directory et un ordinateur SQL Server sont disponibles dans l’environnement bastion avant le redémarrage du Service MIM.  

Une instance de workflow peut être exécutée uniquement par un serveur de Service MIM ayant le même nom de partition de service et le même nom de service que le serveur de Service MIM qui l’a démarrée.  Si un ordinateur donné échoue lors de l’hébergement d’un Service MIM qui traitait des demandes et que cet ordinateur ne sera pas remis en service, il sera nécessaire d’installer le Service MIM sur un nouvel ordinateur. Sur le nouvel ordinateur exécutant le Service MIM après l’installation, modifiez le fichier *resourcemanagementservice.exe.config* et définissez les attributs `serviceName` et `servicePartitionName` du nouveau déploiement MIM pour qu’ils soient identiques au nom d’hôte et au nom de partition de service de l’ordinateur qui a échoué.

### Composants PAM MIM
Le programme d’installation du Service et portail MIM incorpore également des composants PAM supplémentaires, notamment des modules PowerShell et deux services.

#### Préparation
Vous devez installer les composants Privileged Access Management sur chaque ordinateur de l’environnement bastion où le Service MIM est installé.  Vous ne pouvez pas les ajouter par la suite.

#### Récupération
Après la récupération suite à une panne, vérifiez que le Service MIM est en cours d’exécution sur au moins un serveur.  Ensuite, vérifiez que le service de surveillance PAM MIM est également en cours d’exécution sur ce serveur, à l’aide de `net start "PAM Monitoring service"`.

Si le niveau fonctionnel de la forêt de l’environnement bastion est Windows Server 2012 R2, vérifiez que le service de composant PAM MIM est également en cours d’exécution sur ce serveur, à l’aide de la commande `net start "PAM Component service"`.



<!--HONumber=Jul16_HO3-->


