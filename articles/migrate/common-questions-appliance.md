---
title: Questions courantes sur l’appliance Azure Migrate
description: Retrouvez les réponses à des questions courantes sur l’appliance Azure Migrate.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 3bb066b08447a951665e629da5ebcb75714b9f1e
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425353"
---
# <a name="azure-migrate-appliance-common-questions"></a>Appliance Azure Migrate : Questions courantes

Cet article répond à des questions courantes sur l’appliance Azure Migrate. Si vous avez d’autres questions, consultez ces articles :

- [Questions générales](resources-faq.md) sur Azure Migrate.
- [Questions](common-questions-discovery-assessment.md) sur la découverte, l’évaluation et la visualisation des dépendances.
- [Questions](common-questions-server-migration.md) sur la migration de serveur.
- Obtenir des réponses à vos questions sur le [forum Azure Migrate](https://aka.ms/AzureMigrateForum). 


## <a name="what-is-the-azure-migrate-appliance"></a>Qu’est-ce que l’appliance Azure Migrate ?

L’appliance Azure Migrate est une appliance légère utilisée par l’outil d’évaluation Azure Migrate:Server pour détecter et évaluer les serveurs locaux. L’appliance est également utilisée par l’outil de migration Azure Migrate:Server pour la migration sans agent des machines virtuelles VMware locales. 

- L’appliance est déployée localement en tant que machine virtuelle ou physique.
- Elle détecte les machines locales et envoie en continu des métadonnées et des données de performances sur les machines à Azure Migrate.
- La découverte de l’appliance se fait sans agent. Rien n’est installé sur les machines découvertes.

[En savoir plus](migrate-appliance.md) sur l’appliance.

## <a name="how-does-the-appliance-connect-to-azure"></a>Comment l’appliance se connecte-t-elle à Azure ?

L’appliance peut se connecter via Internet ou en utilisant Azure ExpressRoute avec le peering public/Microsoft.

## <a name="does-appliance-analysis-impact-performance"></a>L’analyse de l’appliance a-t-elle un impact sur les performances ?

L’appliance Azure Migrate profile les machines locales en continu pour mesurer les données de performances. Ce profilage n’a quasiment aucun impact sur les performances des machines profilées.

### <a name="can-i-harden-the-appliance-vm"></a>Peut-on renforcer la machine virtuelle de l’appliance ?

Lorsque vous créez la machine virtuelle de l’appliance avec le modèle téléchargé, vous pouvez ajouter des composants (par exemple, un antivirus) au modèle, tant que vous conservez les règles de communication et de pare-feu nécessaires à l’appliance Azure Migrate.


## <a name="what-network-connectivity-is-needed"></a>Quelle est la connectivité réseau requise ?

Passez en revue les informations relatives à la connectivité réseau :
- Évaluation VMware à l’aide de l’appliance Azure Migrate : conditions d’accès [URL](migrate-appliance.md#url-access) et [port](migrate-support-matrix-vmware.md#port-access).
- Migration sans agent VMware à l’aide de l’appliance Azure Migrate : conditions d’accès [URL](migrate-appliance.md#url-access) et [port](migrate-support-matrix-vmware-migration.md#agentless-ports).
- Évaluation Hyper-V à l’aide de l’appliance Azure Migrate : conditions d’accès [URL](migrate-appliance.md#url-access) et [port](migrate-support-matrix-hyper-v.md#port-access).


## <a name="what-data-does-the-appliance-collect"></a>Quelles données l’appliance collecte-t-elle ?

Consultez les données collectées :

- [Données de performances](migrate-appliance.md#collected-performance-data-vmware) et [métadonnées](migrate-appliance.md#collected-metadata-vmware) de la machine virtuelle VMware.
- [Données de performances](migrate-appliance.md#collected-performance-data-hyper-v) et [métadonnées](migrate-appliance.md#collected-metadata-hyper-v) de la machine virtuelle Hyper-V.


## <a name="how-is-data-stored"></a>Comment les données sont stockées ?

Les données collectées par l’appliance Azure Migrate sont stockées à l’emplacement Azure où vous avez créé le projet Azure Migrate. 

- Les données sont stockées de façon sécurisée dans un abonnement Microsoft et sont supprimées quand vous supprimez le projet Azure Migrate.
- Si la [visualisation des dépendances](concepts-dependency-visualization.md) est utilisée, les données collectées sont stockées aux États-Unis, dans un espace de travail Log Analytics créé dans l’abonnement Azure. Ces données sont supprimées lorsque vous supprimez l’espace de travail Log Analytics de votre abonnement.

## <a name="how-much-data-is-uploaded-in-continuous-profiling"></a>Quelle est la quantité de données chargée dans le profilage continu ?

Le volume de données envoyées à Azure Migrate dépend de plusieurs paramètres. Pour vous donner une idée, un projet Azure Migrate avec 10 machines (chacune disposant d’un disque et d’une carte réseau) envoie environ 50 Mo par jour. Cette valeur est approximative et change en fonction du nombre de points de données des cartes réseau et des disques. L’augmentation des données envoyées n’est pas linéaire si le nombre d’ordinateurs, de cartes réseau ou de disques augmente.

## <a name="is-data-encrypted-at-restin-transit"></a>Les données sont-elles chiffrées au repos/en transit ?

Oui, les deux.

- Les métadonnées sont envoyées de façon sécurisée au service Azure Migrate sur Internet via HTTPS.
- Les métadonnées sont stockées dans une base de données [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) et dans le [Stockage Blob Azure](../storage/common/storage-service-encryption.md) au sein d’un abonnement Microsoft. Les métadonnées sont chiffrées au repos.
- Les données pour l’analyse des dépendances sont également chiffrées en transit (HTTPS sécurisé). Elles sont stockées dans un espace de travail Log Analytics dans votre abonnement. Elles sont également chiffrées au repos.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>Comment l’appliance se connecte-t-elle au serveur vCenter ?

1. L’appliance se connecte à vCenter Server (port 443) en utilisant les informations d’identification que vous avez fournies lors de sa configuration.
2. L’appliance utilise VMware PowerCLI pour interroger vCenter Server afin de collecter des métadonnées sur les machines virtuelles gérées par vCenter Server.
3. L’appliance collecte des données de configuration sur les machines virtuelles (cœurs, mémoire, disques, cartes réseau) ainsi que l’historique des performances de chaque machine virtuelle pour le mois dernier.
4. Les métadonnées collectées sont envoyées à Azure Migrate : Server Assessment (sur Internet via HTTPS) pour l’évaluation.

## <a name="can-i-connect-the-appliance-to-multiple-vcenter-servers"></a>Peut-on connecter l’appliance à plusieurs serveurs vCenter ?

Non. Il existe un mappage un-à-un entre une appliance et le serveur vCenter Server. Pour découvrir des machines virtuelles sur plusieurs instances vCenter Server, vous devez déployer plusieurs appliances.

## <a name="how-many-vms-or-servers-can-i-discover-with-an-appliance"></a>Combien de machines virtuelles ou de serveurs puis-je découvrir avec une appliance ?

Vous pouvez détecter jusqu’à 10 000 machines virtuelles VMware, jusqu’à 5 000 machines virtuelles Hyper-V et jusqu’à 250 serveurs physiques avec une seule appliance. Si vous avez plus de machines dans votre environnement local, découvrez comment adapter l’évaluation des machines [Hyper-V](scale-hyper-v-assessment.md) et [VMware](scale-vmware-assessment.md) ainsi que des évaluations [physiques](scale-physical-assessment.md).

## <a name="can-i-delete-an-appliance"></a>Peut-on supprimer une appliance ?

La suppression d’une appliance du projet n’est actuellement pas prise en charge.

- La seule façon de supprimer l’appliance consiste à supprimer le groupe de ressources qui contient le projet Azure Migrate associé à l’appliance.
- Toutefois, la suppression du groupe de ressources a pour effet de supprimer également les autres appliances inscrites, l’inventaire détecté, les évaluations et tous les autres composants Azure associés au projet dans le groupe de ressources.


## <a name="can-i-use-the-appliance-with-a-different-subscriptionproject"></a>Puis-je utiliser l’appliance avec un autre abonnement/projet ?

Après avoir utilisé l’appliance pour lancer la détection, vous ne pouvez pas la reconfigurer avec un autre abonnement Azure ni l’utiliser dans un projet Azure Migrate différent. Vous ne pouvez pas non plus détecter des machines virtuelles sur un autre serveur VMware vCenter. Configurez une nouvelle appliance pour ces tâches.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>Puis-je configurer l’appliance sur une machine virtuelle Azure ?
Non. Cette opération n’est pas prise en charge actuellement. 

## <a name="can-i-discover-on-an-esxi-host"></a>Puis-je lancer une détection sur un hôte ESXi ?
Non. Vous avez besoin d’un serveur vCenter pour détecter des machines virtuelles VMware.

## <a name="how-do-i-update-the-appliance"></a>Comment mettre à jour l’appliance ?

Par défaut, l’appliance et ses agents installés sont mis à jour automatiquement. L’appliance vérifie les mises à jour une fois toutes les 24 heures. Si le processus de mise à jour échoue, une nouvelle tentative est mise en place. Les mises à jour automatiques mettent à jour uniquement l’appliance et les agents de l’appliance. Le système d’exploitation n’est pas mis à jour. Utilisez Microsoft Updates pour maintenir le système d’exploitation à jour.

## <a name="can-i-check-agent-health"></a>Puis-je vérifier l’intégrité de l’agent ?

Sur le portail, accédez à la page **Intégrité de l’agent** dans l’outil d’évaluation de serveur ou de migration de serveur. Vous pouvez y vérifier l’état de la connexion entre Azure et les agents de détection et d’évaluation sur l’appliance.

## <a name="next-steps"></a>Étapes suivantes
Consultez la [vue d’ensemble d’Azure Migrate](migrate-services-overview.md).
