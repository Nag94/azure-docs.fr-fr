---
title: Intégration du contrôle de code source
description: Expérience DevOps de base de données pour l’entreprise destinée au pool SQL avec intégration du contrôle de code source natif à l’aide d’Azure Repos (Git et GitHub).
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: integration
ms.date: 08/23/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 1a4939e8c349f36fe745becb811717983caa95c0
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198209"
---
# <a name="source-control-integration-for-sql-pool"></a>Intégration du contrôle de code source pour le pool SQL

Ce tutoriel explique comment intégrer votre projet de base de données SSDT (SQL Server Data Tools) au contrôle de code source.  L’intégration du contrôle de code source est la première étape de la génération de votre pipeline d’intégration et de déploiement continus avec la ressource de pool SQL dans Azure Synapse Analytics. 

## <a name="before-you-begin"></a>Avant de commencer

- Inscrivez-vous pour créer une [organisation Azure DevOps](https://azure.microsoft.com/services/devops/).
- Parcourez le tutoriel [Créer et connecter](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal).
-  [Installez Visual Studio 2019](https://visualstudio.microsoft.com/vs/older-downloads/). 

## <a name="set-up-and-connect-to-azure-devops"></a>Configurer Azure DevOps et s’y connecter

1. Dans votre organisation Azure DevOps, créez un projet destiné à héberger votre projet de base de données SSDT par le biais d’un dépôt Azure.

   ![Créer un projet](media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Créer un projet")

2. Ouvrez Visual Studio et connectez-vous à votre organisation et à votre projet Azure DevOps à l’étape 1 en sélectionnant « Gérer des connexions ».

   ![Gérer les connexions](media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Gérer les connexions")

   ![Connexion](media/sql-data-warehouse-source-control-integration/3-connect.png "Se connecter")

3. Clonez votre dépôt Azure à partir de votre projet sur votre ordinateur local.

   ![Cloner le dépôt](media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Cloner le dépôt")

## <a name="create-and-connect-your-project"></a>Créer et connecter votre projet

1. Dans Visual Studio, créez un projet de base de données SQL Server avec un répertoire et un dépôt Git local dans votre **dépôt cloné local**.

   ![Créer un projet](media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Création d’un projet")  

2. Cliquez avec le bouton droit sur votre projet SQL vide, puis importez votre entrepôt de données dans le projet de base de données

   ![Importer un projet](media/sql-data-warehouse-source-control-integration/6-import-new-project.png "Importer un projet")  

3. Dans Team Explorer dans Visual Studio, validez tous les changements apportés à votre dépôt Git local. 

   ![Commiter](media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "Commit")  

4. Les changements étant validés localement dans le dépôt cloné, synchronisez-les et envoyez-les (push) à votre dépôt Azure dans votre projet Azure DevOps.

   ![Synchroniser et envoyer (push) - Préproduction](media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Synchroniser et envoyer (push) - Préproduction")

   ![Synchroniser et envoyer (push)](media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Synchroniser et envoyer (push)")  

## <a name="validation"></a>Validation

1. Vérifiez que les changements ont été envoyés (push) à votre dépôt Azure en mettant à jour une colonne de table dans votre projet de base de données à partir de Visual Studio SQL Server Data Tools (SSDT).

   ![Valider la mise à jour de colonne](media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Valider la mise à jour de colonne")

2. Validez le changement et envoyez-la (push) depuis votre dépôt local vers votre dépôt Azure.

   ![Envoyer (push) les changements](media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Envoi (push) des modifications")

3. Vérifiez que le changement a été envoyé (push) dans votre dépôt Azure.

   ![Vérifier](media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Vérifier les changements")

4. (**Facultatif**) Utilisez la comparaison de schémas et mettez à jour les changements apportés à votre entrepôt de données cible à l’aide de SSDT pour vous assurer que les définitions d’objets dans votre dépôt Azure et le dépôt local reflètent votre entrepôt de données.

## <a name="next-steps"></a>Étapes suivantes

- [Développement pour un pool SQL](sql-data-warehouse-overview-develop.md)
