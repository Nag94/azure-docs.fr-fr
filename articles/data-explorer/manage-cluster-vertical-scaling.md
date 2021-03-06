---
title: Gérer la mise à l’échelle verticale (scale-up) d’un cluster pour répondre à la demande dans Azure Data Explorer
description: Cet article décrit les étapes à suivre pour effectuer un scale-up et un scale-down de la taille d’un cluster Azure Data Explorer en fonction des fluctuations de la demande.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 95275598febae2b6b0355a7bc3e512490dae500d
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77560437"
---
# <a name="manage-cluster-vertical-scaling-scale-up-in-azure-data-explorer-to-accommodate-changing-demand"></a>Gérer la mise à l'échelle verticale d'un cluster (montée en puissance) dans Azure Data Explorer pour prendre en compte les fluctuations de la demande

Dimensionner un cluster de manière appropriée est essentiel pour les performances de l’Explorateur de données Azure. Une taille de cluster statique peut conduire à une sous-utilisation ou à une surutilisation, ce qui n’est idéal dans aucun des cas.

La demande de cluster n'étant pas prévisible de manière précise, la meilleure approche consiste à *mettre à l’échelle* un cluster, en ajoutant et en supprimant de la capacité et des ressources d’UC en fonction des fluctuations de la demande. 

Il existe deux workflows pour la mise à l’échelle d’un cluster Azure Data Explorer :

* [Mise à l’échelle horizontale](manage-cluster-horizontal-scaling.md), également appelée scale-in et scale-out.
* Mise à l’échelle verticale, également appelée scale-up et scale-down.

Cet article explique le workflow de mise à l’échelle verticale :

## <a name="configure-vertical-scaling"></a>Configurer la mise à l’échelle verticale

1. Dans le portail Azure, accédez à votre ressource de cluster Azure Data Explorer. Sous **Paramètres**, sélectionnez **Scale-up**.

1. Dans la fenêtre **Scale up**, vous verrez la liste des références SKU disponibles pour votre cluster. Par exemple, dans la figure suivante, seuls quatre références SKU sont disponibles.

    ![Monter en puissance](media/manage-cluster-vertical-scaling/scale-up.png)

    Les références SKU sont désactivées car il s’agit de la référence actuelle ou elles ne sont pas disponibles dans la région où se trouve le cluster.

1. Pour modifier votre référence SKU, sélectionnez une nouvelle référence, puis cliquez sur **Sélectionner**.

> [!NOTE]
> * Le processus de mise à l’échelle verticale peut prendre quelques minutes, et pendant ce temps l’exécution de votre cluster est suspendue. 
> * La baisse de puissance peut altérer les performances de votre cluster.
> * Le prix correspond à une estimation des coûts des machines virtuelles du cluster et du service Azure Data Explorer. Les autres coûts ne sont pas inclus. Pour des informations complètes sur la tarification, consultez la page de l'[estimateur de coût](https://dataexplorer.azure.com/AzureDataExplorerCostEstimator.html) Azure Data Explorer et la [page de tarification](https://azure.microsoft.com/pricing/details/data-explorer/) Azure Data Explorer.

Vous venez de configurer une mise à l'échelle verticale pour votre cluster Azure Data Explorer. Ajoutez une autre règle pour une mise à l’échelle horizontale. Si vous avez besoin d’aide pour résoudre des problèmes de mise à l’échelle de clusters, [ouvrez une demande de support dans le portail Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Étapes suivantes

* Vous pouvez également [gérer la mise à l’échelle horizontale de cluster](manage-cluster-horizontal-scaling.md) afin d’adapter de manière dynamique le nombre d’instances en fonction des métriques que vous spécifiez.

* Supervisez l’utilisation de vos ressources en suivant les instructions de cet article : [Superviser les performances, l’intégrité et l’utilisation d’Azure Data Explorer avec des métriques](using-metrics.md).

