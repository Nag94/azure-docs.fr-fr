---
title: Se connecter à SSMS
description: Utilisez SQL Server Management Studio (SSMS) pour vous connecter à Azure Synapse Analytics et l’interroger.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2109402a874ff8c722bd05e1e5cb62b461cb2292
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198618"
---
# <a name="connect-to-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>Se connecter à Azure Synapse Analytics avec SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Utilisez SQL Server Management Studio (SSMS) pour vous connecter à un entrepôt de données et l’interroger dans Azure Synapse. 

## <a name="prerequisites"></a>Prérequis
Pour utiliser ce didacticiel, vous avez besoin des éléments suivants :

* Un pool SQL existant. Pour en créer un, consultez la page [Créer un pool SQL](sql-data-warehouse-get-started-provision.md).
* SQL Server Management Studio (SSMS) installé. [Installez SSMS](https://msdn.microsoft.com/library/hh213248.aspx) gratuitement si vous ne l’avez pas déjà.
* Le nom complet du serveur SQL. Pour obtenir ces informations, consultez [Se connecter à un pool SQL](sql-data-warehouse-connect-overview.md).

## <a name="1-connect-to-your-sql-pool"></a>1. Vous connecter à votre pool SQL
1. Ouvrez SSMS.
2. Ouvrez l’Explorateur d’objets en sélectionnant **Fichier** > **Se connecter à l'Explorateur d'objets**.
   
    ![Explorateur d’objets SQL Server](media/sql-data-warehouse-query-ssms/connect-object-explorer.png)
3. Renseignez les champs dans la fenêtre Se connecter au serveur.
   
    ![Se connecter au serveur](media/sql-data-warehouse-query-ssms/connect-object-explorer1.png)
   
   * **Nom du serveur**. Saisissez le **nom du serveur** précédemment identifié.
   * **Authentification**. Sélectionnez **Authentification SQL Server** ou **Authentification intégrée Active Directory**.
   * **Nom d’utilisateur** et **Mot de passe**. Entrez un nom d’utilisateur et mot de passe si l’authentification SQL Server a été sélectionnée plus haut.
   * Cliquez sur **Connecter**.
4. Pour voir plus d’informations, développez votre serveur SQL Azure. Vous pouvez afficher les bases de données associées au serveur. Développez AdventureWorksDW pour voir les tables de votre exemple de base de données.
   
    ![Explorer AdventureWorksDW](media/sql-data-warehouse-query-ssms/explore-tables.png)

## <a name="2-run-a-sample-query"></a>2. Exécuter un exemple de requête
Maintenant qu’une connexion à votre base de données a été établie, passons à l’écriture d’une requête.

1. Cliquez avec le bouton droit sur votre base de données dans l’Explorateur d’objets SQL Server.
2. Sélectionnez **Nouvelle requête**. Une nouvelle fenêtre de requête s’ouvre.
   
    ![Nouvelle requête]( media/sql-data-warehouse-query-ssms/new-query.png)
3. Copiez la requête T-SQL suivante dans la fenêtre de requête :
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Exécutez la requête en cliquant sur `Execute` ou utilisez le raccourci : `F5`.
   
    ![Exécuter une requête](media/sql-data-warehouse-query-ssms/execute-query.png)
5. Passez en revue les résultats de la requête. Dans cet exemple, la table FactInternetSales a 60 398 lignes.
   
    ![Résultats de la requête](media/sql-data-warehouse-query-ssms/results.png)

## <a name="next-steps"></a>Étapes suivantes
Maintenant que vous pouvez vous connecter et exécuter des requêtes, essayez de [visualiser les données avec Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md ).
Pour configurer votre environnement pour l’authentification Azure Active Directory, consultez [S'authentifier auprès d'un pool SQL](sql-data-warehouse-authentication.md).
