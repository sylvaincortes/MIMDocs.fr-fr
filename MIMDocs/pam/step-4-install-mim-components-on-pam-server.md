---
title: "Étape 4 du déploiement PAM : Installer MIM | Microsoft Identity Manager"
description: Installez et configurez le portail et le service MIM sur vos stations de travail et serveur Privileged Access Management.
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 92939d32da25896d07bec61e4633f58230a78181


---

# Étape 4 - Installer les composants MIM sur le serveur PAM et la station de travail

>[!div class="step-by-step"]
[« Étape 3](step-3-prepare-pam-server.md)
[Étape 5 »](step-5-establish-trust-between-priv-corp-forests.md)


Sur PAMSRV, connectez-vous en tant que PRIV\Administrateur pour pouvoir installer le Service et portail MIM et l’exemple d’application web de portail.

  > [!NOTE]
  > Vous devez être administrateur de domaine. Si vous n’exécutez pas les commandes suivantes en tant qu’administrateur de domaine, les contrôles de validation d’approbation de l’étape suivante ne sont pas effectués.

Si vous avez téléchargé MIM, décompressez l'archive d'installation MIM dans un nouveau dossier.

##  Exécutez le programme d'installation du service et du portail.  

Suivez les instructions du programme d'installation, puis terminez l'installation.

1.  Lors de la sélection des fonctionnalités de composant, incluez le Service MIM (avec Privileged Access Management, mais pas la création de rapports MIM) et le Portail MIM.  

    ![Installation personnalisée - capture d’écran](./media/PAM_GS_MIM_2015_Service_Portal.png)

2.  Quand vous configurez des services usuels et la connexion de base de données MIM, spécifiez **Créer une base de données**.

    > [!NOTE]
    > Si vous installez le Service MIM plusieurs fois pour la haute disponibilité, spécifiez **Utiliser une base de données existante** pour toutes les installations suivantes.

3.  Quand vous configurez une connexion de serveur de messagerie, définissez comme serveur de messagerie le nom d’hôte d’un serveur Exchange ou SMTP pour l’environnement CORP (utilisez corpdc.contoso.local si vous n’avez pas de serveur de messagerie) et décochez les cases **Utiliser SSL** et **Le serveur de messagerie est Exchange Server 2007 ou Exchange Server 2010**.

4.  Choisissez de générer un nouveau certificat auto-signé.

5.  Définissez les informations d’identification de compte suivantes :
    - Nom du compte de service : *MIMService*  
    - Mot de passe du compte de service : *Pass@word1* (ou le mot de passe que vous avez créé à l’étape 2)  
    - Domaine du compte de service : *PRIV*  
    - Compte de messagerie du service : *MIMService@priv.contoso.local*  

6.  Acceptez les valeurs par défaut pour le nom d’hôte du serveur de synchronisation et spécifiez *PRIV\MIMMA* comme compte d’agent de gestion MIM. Un message d’avertissement apparaît pour signaler que le service de synchronisation MIM n’existe pas. Ceci est normal, car le service de synchronisation MIM n’est pas utilisé dans ce scénario.

7.  Définissez *PAMSRV* comme adresse de serveur de Service MIM.

8.  Définissez *http://pamsrv.priv.contoso.local:82* comme URL de la collection de sites SharePoint.

9. Laissez l'URL du portail d'enregistrement vide.

10. Cochez la case permettant d’ouvrir les ports 5725 et 5726 dans le pare-feu, puis la case permettant d’accorder à tous les utilisateurs authentifiés l’accès au site du portail MIM.

11. Ne renseignez pas le nom d’hôte de l’API REST PAM et définissez *8086* comme numéro de port.

  ![Informations de liaison pour l’API REST PAM - capture d’écran](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Configurez le compte de l’API REST PAM MIM pour utiliser le même compte que SharePoint (car le portail MIM est colocalisé sur ce serveur) :
    - Nom du compte du pool d’applications : *SharePoint*  
    - Mot de passe du compte du pool d’applications : *Pass@word1* (ou le mot de passe que vous avez créé à l’étape 2)  
    - Domaine du compte du pool d’applications : *PRIV*  

    ![Informations d’identification du compte du pool d’applications - capture d’écran](./media/PAM_GS_Configure_Component_Service.png)

    Un message d’avertissement peut s’afficher pour signaler que le compte de service n’est pas sécurisé dans sa configuration actuelle. C’est normal.

13. Configurez le service de composant MIM PAM :
    - Nom du compte de service : *MIMComponent*
    - Mot de passe du compte de service : *Pass@word1* (ou le mot de passe que vous avez créé à l’étape 2)  
    - Domaine du compte de service : *PRIV*

  ![Informations d’identification du compte de service de composant PAM - capture d’écran](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Configurez le service de surveillance PAM :
    - Nom du compte de service : *MIMMonitor*  
    - Mot de passe du compte de service : *Pass@word1* (ou le mot de passe que vous avez créé à l’étape 2)  
    - Domaine du compte de service : *PRIV*  

  ![Informations d’identification du compte de service de surveillance PAM - capture d’écran](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Dans la page Entrez les informations pour les portails de mot de passe MIM, laissez les cases à cocher vides et continuez. Cliquez sur **Suivant** pour poursuivre l’installation.

Une fois l'installation terminée, le serveur redémarre, puis il vérifie que le portail MIM est actif et il autorise les utilisateurs à visualiser leurs propres ressources d'objets dans MIM.

## Configurer des règles de stratégie de gestion du portail MIM

1. Une fois que PAMSRV a redémarré, connectez-vous en tant que PRIV\Administrateur.

2. Lancez Internet Explorer et connectez-vous au portail MIM à l’adresse http://pamsrv.priv.contoso.local:82/identitymanagement. Le premier accès à cette page peut prendre un certain temps.

3. Si nécessaire, connectez-vous en tant que PRIV\Administrateur pour Internet Explorer.

4. Dans Internet Explorer, ouvrez **Options Internet**, accédez à l’onglet **Sécurité**, puis ajoutez le site à la zone **Intranet local**, s’il ne s’y trouve pas déjà. Fermez la boîte de dialogue Options Internet.

5. Accédez au portail MIM avec Internet Explorer et cliquez sur **Règles de stratégie de gestion**.

6. Recherchez la règle de stratégie de gestion **Gestion des utilisateurs : les utilisateurs peuvent lire leurs propres attributs**.

7. Sélectionnez cette règle de stratégie de gestion, décochez la case **La stratégie est désactivée**, cliquez sur **OK**, puis sur **Envoyer**.

## Vérifier les connexions de pare-feu

Le pare-feu doit autoriser les connexions entrantes vers les ports TCP 5725, 5726, 8086 et 8090.

1.  Lancez **Pare-feu Windows avec fonctions avancées de sécurité** (dans Outils d’administration).  
2.  Cliquez sur **Règles de trafic entrant**.  
3.  Vérifiez que ces deux règles figurent dans la liste :  
    - Forefront Identity Manager Service (STS)
    - Forefront Identity Manager Service (Webservice)  
4.  Cliquez sur **Nouvelle règle** > **Port** > **TCP**, puis tapez les ports locaux spécifiques *8086* et *8090*. Continuez l’Assistant en acceptant les valeurs par défaut, donnez un nom à la règle, puis cliquez sur **Terminer**.  
5.  Une fois l’Assistant terminé, fermez l’application Pare-feu Windows.

6.  Lancez le **Panneau de configuration**.  
7.  Sous Réseau et Internet, sélectionnez **Afficher l’état et la gestion du réseau**.  
8.  Vérifiez qu’il y a un réseau actif nommé priv.contoso.local et un réseau de domaine.  
9. Fermez le **Panneau de configuration**.

## Configurer l’exemple d’application web

Dans cette section, vous allez installer et configurer l’exemple d’application web pour l’API REST PAM MIM.

1.  À partir de l’archive d’exemples d’applications web, téléchargez les [exemples Identity Management](https://github.com/Azure/identity-management-samples) sous forme de fichier zip.

2. Décompressez le contenu du dossier **identity-management-samples-master\Privileged-Access-Management-Portal\src** dans un nouveau dossier nommé **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3.  Créez un site web dans IIS nommé MIM Privileged Access Management Example Portal, avec comme chemin physique C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal et le port 8090.  Vous pouvez pour cela exécuter la commande PowerShell suivante :

  ```
  New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
  ```

4.  Configurez l’exemple d’application web pour pouvoir rediriger les utilisateurs vers l’API REST PAM MIM. À l’aide d’un éditeur de texte tel que le Bloc-notes, modifiez le fichier **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. Dans la section **<system.webServer>**, ajoutez les lignes suivantes :

  ```
  <httpProtocol>
    <customHeaders>
      <add name="Access-Control-Allow-Credentials" value="true"  />
      <add name="Access-Control-Allow-Headers" value="content-type" />
      <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
    </customHeaders>
  </httpProtocol>
  ```

5.  Configurez l'exemple d'application web. À l’aide d’un éditeur de texte tel que le Bloc-notes, modifiez le fichier **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Affectez à **pamRespApiUrl** la valeur *http://pamsrv.priv.contoso.local:8086/api/pamresources/*.

6.  Redémarrez IIS avec la commande suivante pour que ces modifications prennent effet.

  ```
  iisreset
  ```

7.  (Facultatif) Vérifiez que l’utilisateur peut s’authentifier auprès de l’API REST. Ouvrez un navigateur web en tant qu’administrateur sur PAMSRV.  Accédez à l’URL de site web http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, authentifiez-vous (si nécessaire) et vérifiez qu’un téléchargement a lieu.

## Installer les applets de commande du demandeur MIM PAM

Installez les applets de commande du demandeur MIM PAM sur la station de travail configurée à l’Étape 1.

1.  Connectez-vous à CORPWKSTN en tant qu’administrateur.

2.  Téléchargez les **Compléments et extensions** sur l’ordinateur CORPWKSTN, s’ils n’y sont pas encore.

3.  À partir de l’archive, décompressez le dossier **Add-ins and extensions** dans un nouveau dossier.

4.  Exécutez le programme d’installation **setup.exe**.

5.  Lors de l’installation personnalisée, spécifiez que le **Client PAM** doit être installé, mais pas le **Complément MIM pour Outlook** ni les **Extensions d’authentification et de mot de passe MIM**.

6.  Dans l’adresse du serveur PAM, spécifiez *pamsrv.priv.contoso.local* comme nom d’hôte du serveur PRIV MIM.

Une fois l'installation terminée, redémarrez CORPWKSTN pour terminer l'enregistrement du nouveau module PowerShell.

Lors de l’étape suivante, vous allez établir l’approbation entre les forêts PRIV et CORP.

>[!div class="step-by-step"]
[« Étape 3](step-3-prepare-pam-server.md)
[Étape 5 »](step-5-establish-trust-between-priv-corp-forests.md)



<!--HONumber=Jul16_HO3-->


