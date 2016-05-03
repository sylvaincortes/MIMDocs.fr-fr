---
# required metadata

title: Installer MIM 2016 & #58 ; Synchroniser Active Directory et le service MIM | Microsoft Identity Manager
description: Utiliser des agents de gestion et le service de synchronisation MIM pour synchroniser vos bases de données Active Directory et MIM.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installer MIM 2016 : synchroniser Active Directory et le service MIM

>[!div class="step-by-step"]
[« Service et portail MIM](install-mim-service-portal.md)

> [!NOTE]
> Dans tous les exemples ci-dessous, **mimservername** représente le nom de votre contrôleur de domaine, **contoso** représente votre nom de domaine, et **Pass@word1** représente un exemple de mot de passe.

Par défaut, aucun connecteur n’est configuré pour le service de synchronisation MIM.  Une première étape classique consiste à utiliser la synchronisation MIM pour remplir la base de données du service MIM à l’aide des comptes Active Directory existants. Pour ce faire, vous allez utiliser l'application du service de synchronisation MIM.

## Créer l’agent de gestion MIM
L’agent de gestion MIM (MA) est le connecteur qui lie la synchronisation MIM au service MIM. Pour créer ce connecteur, vous devez utiliser l'Assistant Créer un agent de gestion.

Quand vous configurez un agent de gestion MIM, vous devez spécifier un compte d'utilisateur. Dans ce document, **MIMMA** est utilisé comme nom de ce compte.

> [!CAUTION]
> Le compte que vous utilisez pour votre agent de gestion MIM doit être le même que celui que vous avez spécifié pendant l'installation du service MIM.

###Pour créer l'agent de gestion MIM

1.  Ouvrez le Gestionnaire du service de synchronisation.

2.  Pour ouvrir l’Assistant Créer un agent de gestion, dans le menu **Actions**, cliquez sur **Créer**.

3.  Dans la page **Créer un agent de gestion**, indiquez les paramètres ci-après, puis cliquez sur **Suivant**.

    -   Agent de gestion pour : agent de gestion du service MIM

    -   Nom : MIMMA

4.  Dans la page **Se connecter à la base de données**, indiquez les paramètres ci-après, puis cliquez sur **Suivant**

    -   Serveur : localhost

    -   Base de données : MIMService

    -   Adresse de la base MIM Service : http://localhost:5725

    -   Mode d'authentification : Authentification intégrée de Windows

    -   Nom d'utilisateur : mimma

    -   Mot de passe : Pass@word

    -   Domaine : contoso

5.  Dans la page **Types d’objet sélectionnés**, vérifiez que les types d’objets listés ci-dessous sont sélectionnés, puis cliquez sur **Suivant**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   Groupe

6.  Dans la page **Attributs sélectionnés**, vérifiez que tous les attributs listés sont sélectionnés, puis cliquez sur **Suivant**.

7.  Dans la page **Configurer le filtre du connecteur**, cliquez sur **Suivant**.

8.  Dans la page **Configurer les mappages de types d’objet**, ajoutez le mappage suivant, puis cliquez sur **Suivant**

    -   Dans la liste **Type d’objet de source de données**, sélectionnez **Person**.

    -   Pour ouvrir la boîte de dialogue Mappage, cliquez sur **Ajouter un mappage**.

    -   Dans la liste **Type d’objet de métaverse**, sélectionnez **Person**.

    -   Pour fermer la boîte de dialogue Mappage, cliquez sur **OK**.

9.  Dans la page **Configurer un flux de valeur d’attribut**, appliquez les mappages de flux de valeur d’attribut ci-après, puis cliquez sur **Suivant**

    ||||
    |-|-|-|
    | **Direction du flux** | **Attribut de source de données** | **Attribut de métaverse** |
    |Importer|Importer|accountName|
    |Importer|Importer|company|
    |Importer|Importer|displayName|
    |Importer|Importer|employeeID|
    |Importer|Importer|employeeType|
    |Importer|Importer|firstName|
    |Importer|Importer|lastName|
    |Importer|Importer|Manager|
    |Importer|Importer|objectSid|
    |Exporter|Exporter|accountName|
    |Exporter|Exporter|company|
    |Exporter|Exporter|displayName|
    |Exporter|Exporter|domaine|
    |Exporter|Exporter|employeeID|
    |Exporter|Exporter|employeeType|
    |Exporter|Exporter|firstName|
    |Exporter|Exporter|lastName|
    |Exporter|Exporter|manager|
    |Exporter|Exporter|objectSid|

10.  Sélectionnez **Person** en tant que type d’objet de source de données.

    -   Select **Person** as the Metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    -   Select **Group** as the data source type and as the metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the following table, complete these steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    | Flow Direction | Data Source Attribute | Metaverse Attribute |
    |-|-|-|
    | Export | AccountName | accountName |
    | Export | DisplayName | displayName |
    | Export | Domain | domain |
    | Export | Scope | scope |
    | Export | Type | type |
    | Export | Member | member |
    | Export | MembershipLocked | membershipLocked |
    | Export | MembershipAddWorkflow | membershipAddWorkflow |
    | Export | Manager | manager |

11.  Dans la page **Configurer l’annulation de mise en service**, cliquez sur **Suivant**.

12.  Pour créer l’agent de gestion, dans la page **Configurer des extensions**, cliquez sur **Terminer**.

## Créer un agent de gestion Active Directory
L’agent de gestion Active Directory est un connecteur pour les services de domaine Active Directory. Pour créer ce connecteur, vous devez utiliser l'Assistant Créer un agent de gestion.

1. Pour ouvrir l’Assistant Créer un agent de gestion, dans le menu **Actions**, cliquez sur **Créer**.

2. Dans la page **Créer un agent de gestion**, indiquez les paramètres ci-après, puis cliquez sur **Suivant** :

    - Agent de gestion pour : services de domaine Active Directory
    - Nom : ADMA

3. Dans la page **Forêt Active Directory**, indiquez les paramètres ci-après, puis cliquez sur **Suivant** :

    - Nom de la forêt : contoso.local
    - Nom d'utilisateur : administrateur
    - Mot de passe : &lt;mot de passe du compte|gt;
    - Domaine : contoso

4. Dans la page **Configurer les partitions d’annuaire**, indiquez les paramètres ci-après, puis cliquez sur **Suivant** :

    - Dans la liste **Sélectionner les partitions d’annuaire**, sélectionnez **DC=CONTOSO, DC=local**.

    - Pour ouvrir la boîte de dialogue Sélectionner des conteneurs, cliquez sur **Conteneurs**.

    - Si vous souhaitez modifier le conteneur pour disposer uniquement des objets de gestion MIM dans un conteneur particulier, cliquez sur le nœud **DC=CONTOSO, DC=local**, puis cliquez sur le nœud du conteneur de votre choix.

    - Pour fermer la boîte de dialogue Sélectionner des conteneurs, cliquez sur **OK**.

5. Dans la page **Configurer la hiérarchie d’approvisionnement**, cliquez sur **Suivant**.

6. Dans la page **Sélectionner des types d’objet**, indiquez les paramètres ci-après, puis cliquez sur **Suivant** :

    - Dans la liste des **types d’objets**, sélectionnez **user** et **group**.

7. Dans la page **Sélectionner des attributs**, indiquez les paramètres ci-après, puis cliquez sur **Suivant** :

    - Sélectionnez **Afficher tout**.

8. Dans la liste **Attributs**, sélectionnez les attributs suivants :

    -   company
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   groupType
    -   manager
    -   managedBy
    -   membre
    -   objectSid
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   unicodePwd
    -   userAccountControl

9. Dans la page **Configurer le filtre du connecteur**, cliquez sur **Suivant**.

10. Dans la page **Configurer des règles de jointure et de projection**, cliquez sur **Suivant**.

11. Dans la page **Configurer le flux de valeur d’attribut**, cliquez sur **Suivant**.

12. Dans la page **Configurer l’annulation de mise en service**, cliquez sur **Suivant**.

13. Dans la page **Configurer des extensions**, cliquez sur **Terminer**.


## Créer des profils d'exécution

Créez des profils d'exécution pour les connecteurs ADMA et MIMMA.

### Créer des profils d'exécution pour le connecteur ADMA

Ce tableau présente les cinq profils d’exécution que vous allez créer pour le connecteur ADMA :

| Nom | Type |
| ---- | ---- |
| Profile1 | Importation intégrale (phase uniquement) |
| Profile2 | Synchronisation complète |
| Profile3 | Importation différentielle (phase uniquement) |
| Profile4 | Synchronisation différentielle |
| Profile5 | Exporter |

Pour créer des profils d'exécution pour le connecteur ADMA :

1. Ouvrez le Gestionnaire du service de synchronisation, puis cliquez dans le menu **Outils**, cliquez sur **Agents de gestion**.

2. Dans la liste **Agents de gestion**, sélectionnez **ADMA**.

3. Pour ouvrir la boîte de dialogue Configurer des profils d’exécution, dans le menu **Actions**, cliquez sur **Configurer des profils d’exécution**.

4. Pour chaque profil d’exécution du tableau, procédez comme suit :

    - Pour ouvrir l’Assistant Configuration du profil d’exécution, cliquez sur **Nouveau profil**.

    - Dans la zone **Nom**, tapez le nom du profil indiqué dans le tableau, puis cliquez sur **Suivant**.

    - Dans la liste **Type**, sélectionnez le type d’étape indiqué dans le tableau, puis cliquez sur **Suivant**.

    - Cliquez sur **Terminer** pour créer le profil d’exécution.

5. Pour fermer la boîte de dialogue Configurer des profils d’exécution, cliquez sur **OK**.

### Créer des profils d'exécution pour le connecteur MIMMA

Ce tableau affiche les cinq profils d’exécution correspondants pour le connecteur MIMMA :

| Nom | Type |
| -------- | -------- |
| Profile1 | Importation intégrale (phase uniquement) |
| Profile2 | Synchronisation complète |
| Profile3 | Importation différentielle (phase uniquement) |
| Profile4 | Synchronisation différentielle |
| Profile5 | Exporter |

Pour créer des profils d'exécution pour le connecteur MIMMA :

1. Ouvrez le Gestionnaire du service de synchronisation, puis cliquez dans le menu **Outils**, cliquez sur **Agents de gestion**.

2. Dans la liste **Agents de gestion**, sélectionnez **MIMMA**.

3. Pour ouvrir la boîte de dialogue Configurer des profils d’exécution, dans le menu **Actions**, cliquez sur **Configurer des profils d’exécution**.

4. Pour chaque profil d’exécution du tableau, procédez comme suit :

    - Pour ouvrir l’Assistant Configuration du profil d’exécution, cliquez sur **Nouveau profil**.

    - Dans la zone **Nom**, tapez le nom du profil indiqué dans le tableau, puis cliquez sur **Suivant**.

    - Dans la liste **Type**, sélectionnez le type d’étape indiqué dans le tableau, puis cliquez sur **Suivant**.

    - Cliquez sur **Terminer** pour créer le profil d’exécution.

5. Pour fermer la boîte de dialogue Configurer des profils d’exécution, cliquez sur **OK**.

## Configuration du service MIM

À l’aide du portail MIM, vous allez créer la règle de synchronisation entrante des utilisateurs Active Directory pour le Service MIM.

Pour créer la règle de synchronisation entrante des utilisateurs Active Directory :

1. Dans la page d’accueil du portail MIM, dans la barre de navigation, cliquez sur **Administration**.

2. Pour ouvrir la page Règles de synchronisation, cliquez sur **Règles de synchronisation**.

3. Pour ouvrir l’Assistant Création d’une règle de synchronisation, dans la barre d’outils, cliquez sur **Nouveau**.

4. Sous l’onglet **Général**, indiquez les informations ci-après, puis cliquez sur **Suivant** :

    -   Nom complet : Règle de synchronisation entrante des utilisateurs Active Directory
    -   Direction du flux de données : Entrant

5. Sous l’onglet **Étendue**, indiquez les informations ci-après, puis cliquez sur **Suivant** :

    -   Type de ressource de métaverse : personne
    -   Système externe : ADMA
    -   Type de ressource système externe : personne

6. Sous l’onglet **Relation**, indiquez les informations ci-après, puis cliquez sur **Suivant** :

    -   Pour configurer les critères de la relation, sélectionnez **ObjectSID** dans la liste MetaverseObject:person(Attribute) et dans la liste ConnectedSystemObject:person(Attribute).

    -   Sélectionnez **Créer une ressource dans MIM**.

7. Dans la page **Flux de valeur d’attribut entrant**, indiquez les informations ci-après, puis cliquez sur **Suivant** :

    | Règle de flux | Source | Destination |
    |-|-|-|
    |Règle 1|samAccountName|f|
    |Règle 2|displayName|displayName|
    |Règle 3|EmployeeType|EmployeeType|
    |Règle 4|givenName|givenName|
    |Règle 5|sn|lastName|
    |Règle 6|Manager|manager|
    |Règle 7|objectSID|ObjectSID|
    |Règle 8|"Contoso"|domaine|

    Pour chaque ligne de ce tableau, procédez comme suit :

    -   Pour ouvrir la boîte de dialogue Définition de flux, cliquez sur **Nouveau flux de valeur d’attribut**.

    -   Sous l’onglet **Source**, sélectionnez l’attribut indiqué pour cette ligne dans le tableau.

    -   Sous l’onglet **Destination**, sélectionnez l’attribut indiqué pour cette ligne dans le tableau.

    -   Pour appliquer la configuration du flux de valeur d’attribut, cliquez sur **OK**.

8. Sous l’onglet **Résumé**, cliquez sur **Envoyer**.

## Initialisation de l'environnement de test
Pour pouvoir tester votre configuration avec vos données Active Directory, vous devez l'initialiser. Ce processus comporte quatre étapes :

### Activer l’approvisionnement

1. Ouvrez le Gestionnaire du service de synchronisation.

2. Pour ouvrir la boîte de dialogue Options, dans le menu **Outils**, cliquez sur **Options**.

3. Sélectionnez **Activer l’approvisionnement des règles de synchronisation**.

4. Pour fermer la boîte de dialogue Options, cliquez sur **OK**.

### Initialiser MIMMA

Exécutez un cycle de synchronisation complet sur ce connecteur. Le cycle complet comprend les profils d’exécution suivants :

    -   Full Import

    -   Full Synchronization

    -   Export

    -   Delta Import

1. Ouvrez le Gestionnaire du service de synchronisation, puis cliquez dans le menu **Outils** sur **Agents de gestion**.

2. Dans la liste **Agents de gestion**, sélectionnez **MIMMA**.

3. Pour ouvrir la boîte de dialogue Exécuter l’agent de gestion, dans le menu **Actions**, cliquez sur **Exécuter**.

4. Pour chaque profil de la liste ci-dessus, procédez comme suit :

    - Pour ouvrir la boîte de dialogue Exécuter l’agent de gestion, dans le menu **Actions**, cliquez sur **Exécuter**.

    - Dans la liste **Profils d’exécution**, sélectionnez le profil d’exécution à configurer.

    - Pour démarrer le profil d’exécution, cliquez sur **OK**.

#### Configuration de la précédence des flux de valeur d'attribut

Pendant l'initialisation du connecteur MIM, les règles de synchronisation configurées ont été placées dans le métaverse.

Changez la précédence des flux de valeur d'attribut pour les attributs fournis par ce connecteur afin de garantir que les attributs déjà présents dans Active Directory peuvent passer dans le métaverse, puis dans la base de données du service MIM.

### Initialisation d'ADMA

Pour initialiser le Connecteur Active Directory, vous devez exécuter une importation intégrale et une synchronisation complète sur ce dernier. L'importation intégrale est nécessaire pour transférer les objets existants d'Active Directory vers l'espace connecteur. La synchronisation complète est nécessaire, car les règles de synchronisation ont changé en projetant les nouvelles règles de synchronisation de l'espace connecteur MIM vers le métaverse. Vous allez

    -   Full Import

    -   Full Synchronization

1. Ouvrir le Gestionnaire du service de synchronisation, puis cliquer dans le menu **Outils** sur **Agents de gestion**.

2. Dans la liste **Agents de gestion**, sélectionnez **ADMA**.

3. Pour ouvrir la boîte de dialogue Exécuter l’agent de gestion, dans le menu **Actions**, cliquez sur **Exécuter**.

4. Pour chaque profil de la liste ci-dessus, procédez comme suit :

    - Pour ouvrir la boîte de dialogue Exécuter l’agent de gestion, dans le menu **Actions**, cliquez sur **Exécuter**.

    - Dans la liste **Profils d’exécution**, sélectionnez le profil d’exécution à configurer.

    - Pour démarrer le profil d’exécution, cliquez sur **OK**.

### Remplissage de la base de données du service MIM

Pour remplir la base de données du service MIM avec les objets, vous devez exécuter un cycle de synchronisation sur le connecteur MIMMA. Le cycle comprend les exécutions des profils d'exécution suivants :

    -   Export

    -   Full Import

    -   Full Sync

    1. Open the Synchronization Service Manager and, on the **Tools** menu, click **Management Agents**.

    2. In the **Management Agents** list, select **MIMMA**.

    3. To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

    4. For each run profile listed above, complete the following steps:

        - To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

        - In the **Run profiles** list, select the run profile you want to run.

        - To start the run profile, click **OK**.

>[!div class="step-by-step"]
[« Service et portail MIM](install-mim-service-portal.md)


<!--HONumber=Apr16_HO2-->


