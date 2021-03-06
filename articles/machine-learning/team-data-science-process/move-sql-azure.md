---
title: Déplacer des données vers Azure SQL Database - Team Data Science Process
description: Déplacez des données à partir de fichiers plats (formats CSV ou TSV) ou de données stockées sur un ordinateur SQL Server local vers une instance Azure SQL Database.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: f9a1424f2afe6c5153e208601b21dff9651880a8
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722456"
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Déplacement de données vers une base de données Azure SQL pour Azure Machine Learning

Cet article présente les options de déplacement des données à partir de fichiers plats (formats CSV ou TSV) ou de données stockées sur un ordinateur SQL Server local vers une instance Azure SQL Database. Ces tâches permettant de déplacer des données vers le cloud font partie du processus TDSP (Team Data Science Process).

Pour une rubrique présentant les options de déplacement de données sur un SQL Server local pour Machine Learning, voir [Déplacer des données vers SQL Server sur une machine virtuelle Azure](move-sql-server-virtual-machine.md).

Le tableau suivant récapitule les options de déplacement de données vers une base de données Azure SQL.

| <b>SOURCE</b> | <b>DESTINATION : Azure SQL Database</b> |
| --- | --- |
| <b>Fichier plat (mise en forme CSV ou TSV)</b> |[Requête SQL Bulk Insert](#bulk-insert-sql-query) |
| <b>Serveur SQL Server local</b> |1.[Exporter dans un fichier plat](#export-flat-file)<br> 2. [Assistant Migration de la base de données SQL](#insert-tables-bcp)<br> 3. [Sauvegarde et restauration de base de données](#db-migration)<br> 4. [Azure Data Factory](#adf). |

## <a name="prereqs"></a>Configuration requise
Les procédures décrites ici nécessitent :

* Un **abonnement Azure**. Si vous n’avez pas d’abonnement, vous pouvez vous inscrire à un [essai gratuit](https://azure.microsoft.com/pricing/free-trial/).
* Un **compte de stockage Azure**. Dans ce didacticiel, vous utilisez un compte de stockage Azure pour stocker des données. Si vous ne possédez pas de compte de stockage Azure, consultez l’article [Créer un compte de stockage](../../storage/common/storage-account-create.md) . Après avoir créé le compte de stockage, vous devez obtenir la clé du compte utilisée pour accéder au stockage. Consultez [Gérer les clés d’accès au compte de stockage](../../storage/common/storage-account-keys-manage.md).
* Un accès à une **base de données Azure SQL Database**. Si vous devez configurer une base de données Azure SQL Database, l’article [Bien démarrer avec Microsoft Azure SQL Database](../../sql-database/sql-database-get-started.md) fournit des informations sur la configuration d’une nouvelle instance de base de données Azure SQL Database.
* **Azure PowerShell** installé et configuré localement. Pour obtenir des instructions, consultez la rubrique [Installation et configuration d'Azure PowerShell](/powershell/azure/overview).

**Données** : les processus de migration sont illustrés à l’aide du [jeu de données NYC Taxi](https://chriswhong.com/open-data/foil_nyc_taxi/). Le jeu de données NYC Taxi contient des informations sur les données de voyage et les prix. Il est disponible sur Stockage Blob Azure : [NYC Taxi Data](https://www.andresmh.com/nyctaxitrips/). Un échantillon et une description de ces fichiers sont fournis dans la [description du jeu de données des voyages NYC Taxi](sql-walkthrough.md#dataset).

Vous pouvez adapter les procédures décrites ici à un jeu de vos propres données ou suivre les étapes décrites à l'aide du jeu de données NYC Taxi. Pour télécharger le jeu de données NYC Taxi dans votre base de données SQL Server locale, suivez la procédure décrite dans [Importer des données en bloc dans SQL Server Database](sql-walkthrough.md#dbload). Ces instructions concernent un SQL Server sur une machine virtuelle Azure, mais la procédure de téléchargement vers le serveur local SQL Server est la même.

## <a name="file-to-azure-sql-database"></a> Déplacement des données à partir d’un fichier plat source vers une instance Azure SQL Database
Les données de fichiers plats (au format CSV ou TSV) peuvent être déplacées vers une instance Azure SQL Database à l’aide d’une requête SQL Bulk Insert.

### <a name="bulk-insert-sql-query"></a> Requête SQL Bulk Insert
Les étapes de la procédure à l’aide de la requête SQL Bulk Insert sont similaires aux instructions de déplacement des données à partir d’une source de fichier plat vers SQL Server sur une machine virtuelle Azure. Pour plus d’informations, consultez [Requête SQL Bulk Insert](move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a> Déplacement de données à partir d’un ordinateur SQL Server local vers une instance Azure SQL Database
Si les données sources sont stockées sur un ordinateur SQL Server local, il existe différentes possibilités pour déplacer les données vers une instance Azure SQL Database :

1. [Exporter dans un fichier plat](#export-flat-file)
2. [Assistant Migration de la base de données SQL](#insert-tables-bcp)
3. [Sauvegarde et restauration de base de données](#db-migration)
4. [Azure Data Factory](#adf).

Les étapes des trois premières options sont similaires à celles décrites dans les sections de [Déplacer des données vers SQL Server sur une machine virtuelle Azure](move-sql-server-virtual-machine.md) qui traitent des mêmes procédures. Vous trouverez dans les instructions ci-dessous des liens vers les sections appropriées de cette rubrique.

### <a name="export-flat-file"></a>Exporter dans un fichier plat
Les étapes de l’exportation vers un fichier plat sont similaires aux instructions décrites dans [Exporter dans un fichier plat](move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>Assistant Migration de la base de données SQL
Les étapes d’utilisation de l’Assistant de migration de base de données SQL sont semblables aux instructions décrites dans [Assistant Migration de base de données SQL](move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Sauvegarde et restauration de base de données
Les étapes d’utilisation de la sauvegarde et de la restauration de bases de données sont similaires aux instructions décrites dans [Sauvegarde et restauration de bases de données](move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Azure Data Factory
Découvrez comment déplacer des données vers une instance Azure SQL Database avec Azure Data Factory (ADF) dans cette rubrique : [Déplacement de données à partir d’un serveur SQL local vers SQL Azure avec Azure Data Factory](move-sql-azure-adf.md). Cette rubrique montre comment utiliser ADF pour déplacer des données d’une base de données SQL Server locale vers une instance Azure SQL Database via Stockage Blob Azure.

Envisagez d’utiliser ADF lorsque les données doivent être migrées en permanence avec des sources hybrides locales et cloud.  ADF vous aide également lorsque les données doivent être transformées ou complétées par une nouvelle logique métier lors de la migration. ADF permet la planification et la surveillance des travaux à l'aide de scripts JSON simples qui gèrent le déplacement des données sur une base périodique. ADF dispose également d'autres fonctionnalités comme la prise en charge des opérations complexes.
