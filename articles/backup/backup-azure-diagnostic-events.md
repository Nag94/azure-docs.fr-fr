---
title: Utilisation des paramètres de diagnostic pour les coffres Recovery Services
description: Article décrivant comment utiliser les anciens et nouveaux événements de diagnostic pour Sauvegarde Azure
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 7abf8873aafeb996476d818376057bfd8732d906
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77583943"
---
# <a name="using-diagnostics-settings-for-recovery-services-vaults"></a>Utilisation des paramètres de diagnostic pour les coffres Recovery Services

Sauvegarde Azure envoie des événements de diagnostic qui peuvent être collectés et utilisés à des fins d’analyse, d’alerte et de création de rapports. 

Vous pouvez configurer les paramètres de diagnostic d’un coffre Recovery Services via le Portail Azure, en accédant au coffre et en cliquant sur l’élément de menu **Paramètres de diagnostic**. Cliquer sur **+ Ajouter un paramètre de diagnostic** vous permet d’envoyer un ou plusieurs événements de diagnostic à un compte de stockage, un Event Hub ou un espace de travail Log Analytics (LA).

![Panneau Paramètres de diagnostic](./media/backup-azure-diagnostics-events/diagnostics-settings-blade.png)

## <a name="diagnostics-events-available-for-azure-backup-users"></a>Événements de diagnostic disponibles pour les utilisateurs de Sauvegarde Azure

Sauvegarde Azure fournit les événements de diagnostic suivants, chacun d’entre eux fournissant des données détaillées sur un ensemble spécifique d’artefacts liés à la sauvegarde :

* CoreAzureBackup
* AddonAzureBackupAlerts
* AddonAzureBackupProtectedInstance
* AddonAzureBackupJobs
* AddonAzureBackupPolicy
* AddonAzureBackupStorage

[Modèle de données pour les événements de diagnostics de Sauvegarde Azure](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

Les données de ces événements peuvent être envoyées à un compte de stockage, à un espace de travail LA ou à un Event Hub. Si vous envoyez ces données à un espace de travail LA, vous devez sélectionner l’option **Spécifique de la ressource** dans l’écran **Paramètre de diagnostic** (voir plus d’informations dans les sections ci-dessous).

## <a name="using-diagnostics-settings-with-log-analytics-la"></a>Utilisation des paramètres de diagnostic avec Log Analytics (LA)

En s’alignant sur la feuille de route Azure Log Analytics, Sauvegarde Azure vous permet d’envoyer des données de diagnostic de coffre à des tables LA dédiées pour LA. Celles-ci sont appelées [Tables Spécifique de la ressource](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#resource-specific).

Pour envoyer les données de diagnostic du coffre à LA :

1.  Accédez à votre coffre, puis cliquez sur **Paramètres de diagnostic**. Cliquez sur **+ Ajouter le paramètre de diagnostic**.
2.  Donnez un nom au paramètre de diagnostic.
3.  Cochez la case **Envoyer à Log Analytics** et sélectionnez un espace de travail Log Analytics.
4.  Sélectionnez **Spécifique de la ressource** dans le bouton bascule et cochez les six événements suivants : **CoreAzureBackup**, **AddonAzureBackupAlerts**, **AddonAzureBackupProtectedInstance**, **AddonAzureBackupJobs**, **AddonAzureBackupPolicy** et **AddonAzureBackupStorage**.
5.  Cliquez sur **Save**(Enregistrer).

![Mode Spécifique de la ressource](./media/backup-azure-diagnostics-events/resource-specific-blade.png)

Une fois que les données circulent dans l’espace de travail, les tables dédiées pour chacun de ces événements sont créées dans votre espace de travail. Vous pouvez interroger directement ces tables et également effectuer des jointures ou des unions entre ces tables si nécessaire.

> [!IMPORTANT]
> Les six événements ci-dessus, à savoir CoreAzureBackup, AddonAzureBackupAlerts, AddonAzureBackupProtectedInstance, AddonAzureBackupJobs, AddonAzureBackupPolicy et AddonAzureBackupStorage, sont pris en charge **uniquement** dans le mode Spécifique de la ressource. **Notez que si vous essayez d’envoyer des données pour ces six événements en mode Microsoft Azure Diagnostics, aucune donnée n’est dirigée vers l’espace de travail.**

## <a name="legacy-event"></a>Événement hérité

Traditionnellement, toutes les données de diagnostics relatives à la sauvegarde d’un coffre sont contenues dans un seul événement appelé « AzureBackupReport ». Les six événements décrits ci-dessus sont, par essence, une décomposition de toutes les données contenues dans AzureBackupReport. 

Actuellement, nous continuons à prendre en charge l’événement AzureBackupReport pour la compatibilité descendante, dans les cas où les utilisateurs ont des requêtes personnalisées existantes sur cet événement, par exemple, des alertes de journal personnalisées, des visualisations personnalisées, etc. Cependant, nous vous conseillons de choisir les nouveaux événements pour tous les nouveaux paramètres de diagnostics sur le coffre car cette méthode facilite considérablement l’utilisation des requêtes de journal, offre une meilleure détectabilité des schémas et de leur structure, améliore les performances en termes de latence d’ingestion et de temps de requête. La prise en charge de l’utilisation du mode Diagnostics Microsoft Azure finira par être abandonnée, et le choix des nouveaux événements peut vous éviter des migrations complexes ultérieurement.

Vous pouvez choisir de créer des paramètres de diagnostic distincts pour AzureBackupReport et les six nouveaux événements, jusqu’à ce que vous ayez migré toutes vos requêtes personnalisées pour utiliser les données des nouvelles tables. L’image ci-dessous montre un exemple de coffre avec deux paramètres de diagnostic. Le premier paramètre, nommé **Setting1** envoie des données d’événement AzureBackupReport à un espace de travail dans le mode Diagnostics Azure. Le deuxième paramètre, nommé **Setting2** envoie les données des six nouveaux événements Sauvegarde Azure à un espace de travail LA dans le mode Spécifique de la ressource.

![Deux paramètres](./media/backup-azure-diagnostics-events/two-settings-example.png)

> [!IMPORTANT]
> L’événement AzureBackupReport est pris en charge **uniquement** dans le mode Diagnostics Azure. **Notez que si vous essayez d’envoyer des données pour cet événement en mode Spécifique de la ressource, aucune donnée n’est dirigée vers l’espace de travail LA.**

> [!NOTE]
> La bascule de Diagnostics Azure / Spécifique de la ressource n’apparaît que si l’utilisateur coche **Envoyer à Log Analytics**. Pour envoyer des données à un compte de stockage ou à un Event Hub, un utilisateur peut simplement sélectionner la destination requise et vérifier les événements souhaités, sans aucune entrée supplémentaire. Là encore, il est recommandé de ne plus choisir l’événement AzureBackupReport hérité.

## <a name="users-sending-azure-site-recovery-events-to-log-analytics-la"></a>Utilisateurs envoyant des événements d’Azure Site Recovery à Log Analytics (LA)

Les événements Sauvegarde Azure et Azure Site Recovery sont envoyés à partir du même coffre Recovery Services. Comme Azure Site Recovery n’est pas intégré à des tables Spécifique de la ressource, les utilisateurs souhaitant envoyer des événements Azure Site Recovery à LA sont dirigés pour utiliser le mode Diagnostics Azure **uniquement** (voir l’image ci-dessous). **Choisir le mode Spécifique de la ressource pour les événements Azure Site Recovery Events empêchera l’envoi des données requises à l’espace de travail LA.** .

![Événements Site Recovery](./media/backup-azure-diagnostics-events/site-recovery-settings.png)

Pour résumer les nuances ci-dessus :

* Si vous avez déjà configuré Diagnostics LA avec Diagnostics Azure et que vous avez écrit des requêtes personnalisées, **ne modifiez pas** ce paramètre jusqu’à ce que vous migriez vos requêtes pour utiliser les données des nouveaux événements.
* Si vous souhaitez également effectuer une intégration dans de nouvelles tables (comme cela est recommandé), créez un **nouveau** paramètre de diagnostics, choisissez **Spécifique de la ressource** et sélectionnez les six nouveaux événements comme indiqué ci-dessus.
* Si vous envoyez actuellement des événements d’Azure Site Recovery à LA, **évitez** de choisir le mode Spécifique de la ressource pour ces événements, sinon les données de ces événements ne seront pas transmises à votre espace de travail LA. Au lieu de cela, créez un **paramètre de diagnostic supplémentaire**, sélectionnez **Diagnostics Azure** , puis choisissez les événements Azure Site Recovery pertinents.

L’image ci-dessous représente l’exemple d’un utilisateur ayant trois paramètres de diagnostic pour un coffre. Le premier paramètre, nommé **Setting1** envoie des données d’événement AzureBackupReport à un espace de travail LA dans le mode Diagnostics Azure. Le deuxième paramètre, nommé **Setting2** envoie les données des six nouveaux événements Sauvegarde Azure à un espace de travail LA dans le mode Spécifique de la ressource. Le premier paramètre, nommé **Setting3** envoie des données d’événement Azure Site Recovery à un espace de travail LA dans le mode Diagnostics Azure.

![Trois paramètres](./media/backup-azure-diagnostics-events/three-settings-example.png)

## <a name="next-steps"></a>Étapes suivantes

[En savoir plus sur le modèle de données Log Analytics pour les événements de diagnostic](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)
