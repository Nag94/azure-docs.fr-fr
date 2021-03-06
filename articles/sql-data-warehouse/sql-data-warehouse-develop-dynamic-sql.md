---
title: Utilisation du langage SQL dynamique
description: Conseils relatifs à l’utilisation de code SQL dynamique dans Azure SQL Data Warehouse pour le développement de solutions.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 87320128537a235e8047a2f826b0e59c08aef76b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692845"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Code SQL dynamique dans SQL Data Warehouse
Conseils relatifs à l’utilisation de code SQL dynamique dans Azure SQL Data Warehouse pour le développement de solutions.

## <a name="dynamic-sql-example"></a>Exemple de SQL dynamique

Quand vous développez le code d’une application pour SQL Data Warehouse, vous pouvez avoir besoin d’utiliser un SQL dynamique de façon à offrir des solutions flexibles, génériques et modulaires. SQL Data Warehouse ne prend pas en charge les données de type objet blob pour l’instant. Le fait de ne pas prendre en charge les types de données blob peut limiter la taille de vos chaînes, car les types de données blob incluent les types varchar(max) et nvarchar(max). Si vous avez utilisé ces types dans votre code d’application pour générer de grandes chaînes, vous devez arrêter le code en blocs et utiliser l’instruction EXEC à la place.

Voici un exemple simple :

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Si la chaîne est courte, vous pouvez utiliser [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) normalement.

> [!NOTE]
> Les instructions exécutées en tant qu'instructions SQL dynamiques sont toujours soumises à l'ensemble des règles de validation TSQL.
> 
> 

## <a name="next-steps"></a>Étapes suivantes
Pour obtenir des conseils supplémentaires, consultez la [vue d’ensemble du développement](sql-data-warehouse-overview-develop.md).

