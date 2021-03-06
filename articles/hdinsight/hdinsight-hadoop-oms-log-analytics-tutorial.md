---
title: Utiliser les journaux Azure Monitor pour surveiller les clusters Azure HDInsight
description: Découvrez comment utiliser les journaux d’activité Azure Monitor pour surveiller les travaux en cours d’exécution dans un cluster HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: e4b33e132e660fba7d06ff33c7db06c7727dd26c
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162784"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Utiliser les journaux d’activité Azure Monitor pour superviser les clusters HDInsight

Découvrez comment activer les journaux d’activité Azure Monitor pour surveiller les opérations de cluster Hadoop dans HDInsight, et comment ajouter une solution de supervision HDInisght.

[Journaux Azure Monitor](../log-analytics/log-analytics-overview.md) est un service dans Azure Monitor qui supervise vos environnements cloud et locaux tout en assurant leur disponibilité et leurs performances. Il collecte les données générées par les ressources de votre cloud et de vos environnements locaux et d’autres outils d’analyse pour fournir une analyse sur plusieurs sources.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Conditions préalables requises

* Un espace de travail Log Analytics. Considérez cet espace de travail comme un environnement des journaux d’activité Azure Monitor avec son propre référentiel de données, et ses propres sources de données et solutions. Pour obtenir des instructions, consultez la rubrique [Créer un espace de travail Log Analytics](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* Un cluster Azure HDInsight. Vous pouvez actuellement utiliser les journaux d’activité Azure Monitor avec les types de cluster HDInsight suivants :

  * Hadoop
  * hbase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Pour savoir comment créer un cluster HDInsight, consultez [Bien démarrer avec Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* Module Az Azure PowerShell.  Consultez [Présentation du nouveau module Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az). Vérifiez que vous avez la version la plus récente. Si nécessaire, exécutez `Update-Module -Name Az`.

> [!NOTE]  
> Il est recommandé de placer le cluster HDInsight et l’espace de travail Log Analytics dans la même région pour obtenir de meilleures performances. Notez que les journaux d’activité Azure Monitor ne sont pas disponibles dans toutes les régions Azure.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Activer des journaux Azure Monitor avec le portail

Cette section vous explique comment configurer un cluster Hadoop HDInsight existant pour utiliser un espace de travail Azure Log Analytics pour surveiller les travaux, les journaux d’activité de débogage, etc.

1. Dans le [Portail Azure](https://portal.azure.com/), sélectionnez votre cluster.  Pour obtenir des instructions, consultez la page [Énumération et affichage des clusters](./hdinsight-administer-use-portal-linux.md#showClusters). Le cluster est ouvert dans une nouvelle page du portail.

1. À gauche, sous **Supervision**, sélectionnez **Azure Monitor**.

1. Dans la vue principale, sous **Intégration à Azure Monitor**, sélectionnez **Activer**.

1. Dans la liste déroulante **Sélectionner un espace de travail**, sélectionnez un espace de travail Log Analytics existant.

1. Sélectionnez **Enregistrer**.  L’enregistrement du paramètre prend quelques instants.

    ![Activer la supervision pour les clusters HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/azure-portal-monitoring.png "Activer la supervision pour les clusters HDInsight")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Activer des journaux Azure Monitor avec Azure PowerShell

Vous pouvez activer les journaux Azure Monitor à l’aide de l’applet de commande [Enable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightmonitoring) du module Az Azure PowerShell.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables monitoring and relevant logs will be sent to the specified workspace.
Enable-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster `
    -WorkspaceId $WorkspaceId `
    -PrimaryKey $PrimaryKey

# Gets the status of monitoring installation on the cluster.
Get-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster
```

Pour désactiver, utilisez l’applet de commande [Disable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring) :

```powershell
Disable-AzHDInsightMonitoring -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>Installer des solutions de gestion de clusters HDInsight

HDInsight fournit des solutions de gestion de clusters que vous pouvez ajouter pour les journaux d’activité Azure Monitor. Les [solutions de gestion](../log-analytics/log-analytics-add-solutions.md) ajoutent des fonctionnalités aux journaux d’activité Azure Monitor en fournissant des données et des outils d’analyse supplémentaires. Ces solutions collectent des indicateurs de performance importants de vos clusters HDInsight et fournissent les outils pour rechercher les indicateurs. Elles fournissent également des visualisations et des tableaux de bord pour la plupart des types de cluster pris en charge dans HDInsight. En vous appuyant sur les mesures collectées avec la solution, vous êtes en mesure de créer des règles et des alertes personnalisées de surveillance.

Voici les solutions HDInsight disponibles :

* HDInsight Hadoop Monitoring
* HDInsight HBase Monitoring
* HDInsight Interactive Query Monitoring
* HDInsight Kafka Monitoring
* HDInsight Spark Monitoring
* HDInsight Storm Monitoring

Pour obtenir des instructions sur l’installation d’une solution de gestion, consultez la rubrique sur les [solutions de gestion dans Azure](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Pour faire des essais, installez une solution de supervision HDInsight Hadoop. Vous voyez alors une vignette **HDInsightHadoop** sous **Résumé**. Sélectionnez la vignette **HDInsightHadoop**. La solution HDInsightHadoop ressemble à ceci :

![Vue de la solution de supervision HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Étant donné que le cluster est tout nouveau, le rapport ne contient aucune activité.

## <a name="configuring-performance-counters"></a>Configuration des compteurs de performances

Azure Monitor prend également en charge la collecte et l’analyse des métriques de performances pour les nœuds de votre cluster. Pour plus d’informations sur l’activation et la configuration de cette fonctionnalité, voir [Sources de données de performances Linux dans Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#linux-performance-counters).

## <a name="cluster-auditing"></a>Audit de cluster

HDInsight prend en charge l’audit de cluster avec des journaux Azure Monitor en important les types de journaux suivants :

* `log_gateway_audit_CL` : ce tableau répertorie les journaux d’audit de nœuds de passerelle de cluster qui affichent des tentatives de connexion réussies et ayant échoué.
* `log_auth_CL` : ce tableau répertorie les journaux SSH contenant des tentatives de connexion réussies et ayant échoué.
* `log_ambari_audit_CL` : ce tableau répertorie les journaux d’audit d’Ambari.
* `log_ranger_audti_CL` : ce tableau répertorie les journaux d’audit d’Apache Ranger sur des clusters ESP.

## <a name="next-steps"></a>Étapes suivantes

* [Interroger les journaux Azure Monitor pour surveiller les clusters HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)
