---
title: 'Interface CLI : Connecter une application à Redis'
description: Découvrez comment utiliser Azure CLI pour automatiser le déploiement et la gestion de votre application App Service. Cet exemple montre comment connecter une application à Azure Cache pour Redis.
author: msangapu-msft
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: a5654ea8c0333e21421e0f9c55cc00d70a7be567
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74688431"
---
# <a name="connect-an-app-service-app-to-an-azure-cache-for-redis-using-cli"></a>Connecter une application App Service à un Cache Azure pour Redis à l’aide de l’interface CLI

Cet exemple de script crée un Cache Azure pour Redis et une application App Service. Il lie ensuite le Cache Azure pour Redis à l’application à l’aide des paramètres de l’application.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Si vous choisissez d’installer et d’utiliser l’interface CLI localement, vous devez utiliser Azure CLI 2.0 ou version ultérieure. Pour connaître la version de l’interface, exécutez `az --version`. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer un groupe de ressources, une application App Service, un Cache Azure pour Redis et toutes les ressources associées. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Crée un plan App Service. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Crée une application App Service. |
| [`az redis create`](/cli/azure/redis?view=azure-cli-latest#az-redis-create) | Crée une instance de Cache Azure pour Redis. |
| [`az redis list-keys`](/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys) | Liste les clés d’accès pour l’instance de Cache Azure pour Redis. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) | Crée ou met à jour un paramètre d’application pour une application App Service. Les paramètres d’application sont exposés en tant que variables d’environnement pour votre application. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure).

Vous trouverez des exemples supplémentaires de scripts CLI App Service dans la [documentation relative à Azure App Service](../samples-cli.md).
