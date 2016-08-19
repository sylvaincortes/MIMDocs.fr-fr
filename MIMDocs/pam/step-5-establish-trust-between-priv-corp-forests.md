---
title: "Étape 5 du déploiement PAM : Lien de forêt | Microsoft Identity Manager"
description: "Établissez une approbation entre les forêts PRIV et CORP afin que les utilisateurs disposant de privilèges dans PRIV puissent toujours accéder à des ressources dans CORP."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 3a7039f5d7c950cd0d4c8ab713a7beacc5c45526


---

# Étape 5 : Établir l'approbation entre les forêts PRIV et CORP

>[!div class="step-by-step"]
[« Étape 4](step-4-install-mim-components-on-pam-server.md)
[Étape 6 »](step-6-transition-group-to-pam.md)


Pour chaque domaine CORP comme contoso.local, les contrôleurs de domaine PRIV et CONTOSO doivent être liés par une approbation. Cela permet aux utilisateurs du domaine PRIV d’accéder aux ressources du domaine CORP.

## Connecter chaque contrôleur de domaine à son homologue

Avant d’établir une approbation, vous devez configurer chaque contrôleur de domaine pour la résolution de noms DNS pour son homologue, en fonction de l’adresse IP de l’autre serveur DNS/contrôleur domaine.

1.  Si les contrôleurs de domaine ou le serveur avec le logiciel MIM sont déployés en tant que machines virtuelles, vérifiez qu’aucun autre serveur DNS ne fournit des services d’attribution de noms de domaine à ces ordinateurs.
    - Si les machines virtuelles ont plusieurs interfaces réseau, notamment des interfaces réseau connectées aux réseaux publics, vous devrez peut-être désactiver momentanément ces connexions ou substituer les paramètres d’interface réseau Windows. Vous devez absolument vous assurer qu’aucune machine virtuelle n’utilise une adresse de serveur DNS fournie par DHCP.

2.  Vérifiez que chaque contrôleur de domaine CORP existant peut acheminer les noms vers la forêt PRIV. Sur chaque contrôleur de domaine en dehors de la forêt PRIV, tel que CORPDC, lancez PowerShell et tapez la commande suivante :

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    Vérifiez que la sortie indique un enregistrement de serveur de noms pour le domaine PRIV avec l’adresse IP correcte.

3.  Si le contrôleur de domaine est incapable de router le domaine PRIV, utilisez le **Gestionnaire DNS** (accessible dans **Démarrer** > **Outils d’application** > **DNS**) pour configurer le transfert de noms DNS pour le domaine PRIV vers l’adresse IP de PRIVDC. S’il s’agit d’un domaine supérieur (par exemple, contoso.local), développez les nœuds pour ce contrôleur de domaine et son domaine, tels que **CORPDC** > **Zones de recherche directe** > **contoso.local**, et vérifiez qu’une clé nommée **priv** est présente comme type de serveur de noms (NS).

    ![Structure de fichiers de clé priv - capture d’écran](./media/PAM_GS_DNS_Manager.png)

## Établir l’approbation sur PAMSRV

Sur PAMSRV, établissez une approbation à sens unique avec chaque domaine tel que CORPDC, pour que les contrôleurs de domaine CORP approuvent la forêt PRIV.

1. Connectez-vous à PAMSRV en tant qu’administrateur de domaine PRIV (PRIV\Administrateur).

2.  Lancez PowerShell.

3.  Tapez les commandes PowerShell suivantes pour chaque forêt existante. À l’invite, entrez les informations d’identification de l’administrateur du domaine CORP (CONTOSO\Administrator).

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Tapez les commandes PowerShell suivantes pour chaque domaine dans les forêts existantes. À l’invite, entrez les informations d’identification de l’administrateur du domaine CORP (CONTOSO\Administrator).

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## Accorder aux forêts un accès en lecture à Active Directory

Pour chaque forêt existante, activez l’accès en lecture à Active Directory par les administrateurs de PRIV et par le service de surveillance.

1.  Connectez-vous au contrôleur de domaine de forêt CORP existant, (CORPDC), en tant qu’administrateur de domaine du domaine de niveau supérieur dans cette forêt (Contoso\Administrateur).  
2.  Lancez **Utilisateurs et ordinateurs Active Directory**.  
3.  Cliquez avec le bouton droit sur le domaine **contoso.local**, puis sélectionnez **Délégation de contrôle**.  
4.  Sous l’onglet Utilisateurs et groupes sélectionnés, cliquez sur **Ajouter**.  
5.  Dans la fenêtre Sélectionner les utilisateurs, les ordinateurs ou les groupes, cliquez sur **Emplacements** et remplacez l’emplacement par *priv.contoso.local*.  Comme nom d’objet, tapez *Admins du domaine* et cliquez sur **Vérifier les noms**. Quand une fenêtre contextuelle s’affiche, entrez le nom d’utilisateur *priv\administrateur* et son mot de passe.  
6.  Après Admins du domaine, ajoutez « *; MIMMonitor* ». Une fois les noms **Admins du domaine** et **MIMMonitor** soulignés, cliquez sur **OK**, puis sur **Suivant**.  
7.  Dans la liste des tâches courantes, sélectionnez **Lire toutes les informations sur l’utilisateur**, cliquez sur **Suivant**, puis sur **Terminer**.  
8.  Fermez Utilisateurs et ordinateurs Active Directory.

9.  Ouvrez une fenêtre PowerShell.  
10.  Utilisez `netdom` pour vérifier que l’historique des SID est activé et que le filtrage des SID est désactivé. Tapez :  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    La sortie doit indiquer **Activation de l’historique des SID pour cette approbation** ou **L’historique des SID est déjà activé pour cette approbation**.

    La sortie doit aussi indiquer que **Le filtrage des SID n’est pas activé pour cette approbation**. Pour plus d’informations, consultez [Disable SID filter quarantining (Désactiver la mise en quarantaine du filtre de SID)](http://technet.microsoft.com/library/cc772816.aspx).

## Démarrer les services de surveillance et de composants

1.  Connectez-vous à PAMSRV en tant qu’administrateur de domaine PRIV (PRIV\Administrateur).

2.  Lancez PowerShell.

3.  Tapez les commandes PowerShell suivantes.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

À l’étape suivante, vous déplacerez un groupe vers PAM.

>[!div class="step-by-step"]
[« Étape 4](step-4-install-mim-components-on-pam-server.md)
[Étape 6 »](step-6-transition-group-to-pam.md)



<!--HONumber=Jul16_HO3-->


