---
title: Notifications de modification de mot de passe | Microsoft Identity Manager
description: "Découvrir les étapes pour installer et configurer le service de notification de modification de mot de passe MIM sur votre contrôleur de domaine."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: e25f4b3a60f2c432cd33c8f84c750110cbe605ee


---

# Déployer le service de notification de modification de mot de passe MIM sur un contrôleur de domaine

## Installer le service de notification de modification de mot de passe
Le service PCNS (Password Change Notification Service) est un service que vous installez sur les contrôleurs de domaine et qui autorise la synchronisation des mots de passe par MIM avec d’autres systèmes, tels que le serveur d’annuaire d’un autre fournisseur. Pour la synchronisation des mots de passe, installez le service PCNS sur chaque serveur contrôleur de domaine.

1.  Connectez-vous en tant qu'administrateur de domaine sur un serveur exécutant Windows Server avec le rôle Services de domaine Active Directory.

2.  Copiez le dossier d'installation du service PCNS sur l'ordinateur.

3.  Recherchez le fichier *Password Change Notification Service.msi* , cliquez dessus avec le bouton droit et créez un raccourci.

4.  Recherchez le fichier de raccourci, cliquez dessus avec le bouton droit et affichez ses **Propriétés**.

5.  Dans le champ cible, ajoutez le préambule *msiexec.exe /i* avant le chemin d’accès au fichier msi et le suffixe *SCHEMAONLY = TRUE* après le chemin d’accès au fichier msi. Par exemple, si le dossier d’installation est *C:\PCNS*, la commande à exécuter ressemblera à ce qui suit : (tout sur une seule ligne).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Enregistrez les modifications dans le fichier de raccourci.

7.  Exécutez le fichier de raccourci pour démarrer l'installation du service PCNS en mode d'extension de schéma. Quand l'écran suivant s'affiche, cliquez sur **Suivant**.

8.  Vous serez averti que le programme d'installation va mettre à jour le schéma Active Directory pour le service de notification de modification de mot de passe. Cliquez sur **OK** pour poursuivre la mise à jour du schéma.

9. Une fois que le processus d'extension de schéma est terminé et que l'écran suivant s'est affiché, cliquez sur **Terminer**.

10. Réexécutez le fichier *Password Change Notification Service.msi* , cette fois-ci directement (aucune chaîne d'exécution n'est nécessaire).  Quand l'écran suivant s'affiche, cliquez sur **Suivant**.

11. Acceptez les termes du contrat de licence et cliquez sur **Suivant**.

12. Cliquez pour commencer l'installation.

13. Quand l'écran indiquant la réussite de l'installation apparaît, cliquez sur **Terminer**.

14. Redémarrez votre ordinateur pour que les modifications de configuration apportées au service de notification de modification de mot de passe MIM prennent effet. Vous pouvez pour cela cliquer sur **Oui** dans la fenêtre contextuelle qui s’affiche, ou vous pouvez redémarrer l’ordinateur plus tard.

## Configuration du service de notification de modification de mot de passe
Une fois reconnecté au serveur contrôleur de domaine en tant qu’administrateur de domaine, accédez à *C:\Program Files\Microsoft Password Change Notification*. Exécutez *pcnscfg.exe*.



<!--HONumber=Jul16_HO3-->


