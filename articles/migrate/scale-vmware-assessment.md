---
title: Évaluer un grand nombre de machines virtuelles VMware pour la migration vers Azure avec Azure Migrate | Microsoft Docs
description: Décrit comment évaluer de grands nombres de machines virtuelles VMware pour les migrer vers Azure à l’aide du service Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: f76134ffc6a7becb9b5719dcb3d826130b7cfa86
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453276"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Évaluer de grands nombres de machines virtuelles VMware pour les migrer vers Azure


Cet article explique comment évaluer de grands nombres (1 000 à 35 000) de machines virtuelles VMware locales pour les migrer vers Azure à l’aide de l’outil Évaluation de serveur Azure Migrate.

[Azure Migrate](migrate-services-overview.md) fournit un hub d’outils qui vous permettent de découvrir, d’évaluer et de migrer des applications, une infrastructure et des charges de travail vers Microsoft Azure. Le hub comprend des outils Azure Migrate et des offres d’ISV (fournisseurs de logiciels indépendants) tiers. 

Dans cet article, vous apprendrez comment :
> [!div class="checklist"]
> * Planifier l’évaluation à grande échelle.
> * Configurer les autorisations Azure et préparer VMware pour l'évaluation.
> * Créer un projet Azure Migrate et une évaluation.
> * Examiner l’évaluation au fur et à mesure que vous planifiez la migration.


> [!NOTE]
> Si vous voulez tester une preuve de concept afin d’évaluer quelques machines virtuelles avant d’effectuer une évaluation à grande échelle, suivez notre [série de tutoriels](tutorial-prepare-vmware.md)

## <a name="plan-for-assessment"></a>Planifier l’évaluation

Lorsque vous planifiez l'évaluation d'un grand nombre de machines virtuelles VMware, quelques points sont à prendre en compte :

- **Planification de projets Azure Migrate** : Découvrez comment déployer les projets Azure Migrate. Par exemple, si vos centres de données sont situés dans des zones géographiques différentes, ou si vous avez besoin de stocker des métadonnées de découverte, d’évaluation ou de migration dans une autre zone géographique, vous pouvez avoir besoin de plusieurs projets. 
- **Planifier des appliances** : Azure Migrate utilise une appliance Azure Migrate locale, déployée en tant que machine virtuelle VMware, pour découvrir en permanence les machines virtuelles. L’appliance surveille les changements d’environnement tels que l’ajout de machines virtuelles, de disques ou d’adaptateurs réseau. Elle envoie également à Azure les métadonnées et les données de performance concernant ces opérations. Vous devez déterminer le nombre d'appliances à déployer.
- **Planifier des comptes pour la découverte** : L'appliance Azure Migrate utilise un compte avec accès à vCenter Server afin de découvrir les machines virtuelles pour l'évaluation et la migration. Si vous découvrez plus de 10 000 machines virtuelles, configurez plusieurs comptes.


## <a name="planning-limits"></a>Limites de planification
 
Utilisez les limites résumées dans ce tableau pour la planification.

**Planification** | **Limites**
--- | --- 
**Projets Azure Migrate** | Évaluez jusqu'à 35 000 machines virtuelles par projet.
**Appliance Azure Migrate** | Une appliance peut découvrir jusqu'à 10 000 machines virtuelles sur un vCenter Server.<br/> Une appliance ne peut se connecter qu'à un seul vCenter Server.<br/> Une appliance ne peut être associée qu’à un seul projet Azure Migrate.<br/>  Un nombre quelconque d’appliances peut être associé à un même projet Azure Migrate. <br/><br/> 
**Groupe** | Vous pouvez ajouter jusqu’à 35 000 machines virtuelles dans un groupe unique.
**Évaluation d’Azure Migrate** | Vous pouvez évaluer jusqu’à 35,000 machines virtuelles par évaluation.

En tenant compte de ces limites, voici quelques exemples de déploiements :


**Serveur vCenter** | **Machines virtuelles sur serveur** | **Recommandation** | **Action**
---|---|---
Une | < 10 000 | Un projet Azure Migrate.<br/> Une appliance.<br/> Un compte vCenter pour la découverte. | Configurez l'appliance, puis connectez-vous au vCenter Server avec un compte.
Une | > 10 000 | Un projet Azure Migrate.<br/> Plusieurs appliances.<br/> Plusieurs comptes vCenter. | Configurez une appliance par tranche de 10 000 machines virtuelles.<br/><br/> Configurez des comptes vCenter et divisez l'inventaire pour limiter l'accès d'un compte à moins de 10 000 machines virtuelles.<br/> Connectez chaque appliance au vCenter Server avec un compte.<br/> Vous pouvez analyser les dépendances entre les machines découvertes avec différentes appliances.
Multiple | < 10 000 |  Un projet Azure Migrate.<br/> Plusieurs appliances.<br/> Un compte vCenter pour la découverte. | Configurez les appliances, puis connectez-vous au vCenter Server avec un compte.<br/> Vous pouvez analyser les dépendances entre les machines découvertes avec différentes appliances.
Multiple | > 10 000 | Un projet Azure Migrate.<br/> Plusieurs appliances.<br/> Plusieurs comptes vCenter. | Si la découverte du vCenter Server est inférieure à 10 000 machines virtuelles, configurez une appliance pour chaque vCenter Server.<br/><br/> Si la découverte du vCenter Server est supérieure à 10 000 machines virtuelles, configurez une appliance par tranche de 10 000 machines virtuelles.<br/> Configurez des comptes vCenter et divisez l'inventaire pour limiter l'accès d'un compte à moins de 10 000 machines virtuelles.<br/> Connectez chaque appliance au vCenter Server avec un compte.<br/> Vous pouvez analyser les dépendances entre les machines découvertes avec différentes appliances.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Planifier la découverte dans un environnement multilocataire

Si vous planifiez un environnement multilocataire, vous pouvez définir l’étendue de la découverte sur le vCenter Server.

- Vous pouvez définir l’étendue de découverte de l’appliance sur des centres de données, des clusters ou un dossier de clusters, des hôtes ou un dossier d’hôtes, ou des machines virtuelles individuelles vCenter Server.
- Si votre environnement est partagé entre plusieurs locataires et que vous souhaitez découvrir chaque locataire séparément, vous pouvez accéder au compte vCenter que l'appliance utilise pour la découverte. 
    - Vous pouvez également définir l’étendue sur des dossiers de machines virtuelles si les locataires partagent des hôtes. Azure Migrate ne peut pas découvrir les machines virtuelles si le compte vCenter s’est vu octroyer un accès au niveau du dossier de machine virtuelle vCenter. Si vous souhaitez définir l’étendue de la découverte en fonction de dossiers de machines virtuelles, vous pouvez le faire en vérifiant que le compte vCenter dispose d’un accès en lecture seule affecté au niveau de la machine virtuelle. Pour en savoir plus sur la définition de l’étendue de la découverte, cliquez [ici](tutorial-assess-vmware.md#set-the-scope-of-discovery).

## <a name="prepare-for-assessment"></a>Préparer pour l’évaluation

Préparez Azure et VMware pour l’évaluation des serveurs. 

1. Vérifiez les [spécifications et limitations de la prise en charge VMware](migrate-support-matrix-vmware.md).
2. Configurez des autorisations pour permettre à votre compte Azure d’interagir avec Azure Migrate.
3. Préparez VMware pour l’évaluation.

Suivez les instructions de [ce tutoriel](tutorial-prepare-vmware.md) pour configurer ces paramètres.


## <a name="create-a-project"></a>Création d’un projet

Conformément à vos exigences de planification, procédez comme suit :

1. Créez un projet Azure Migrate.
2. Ajoutez l’outil Évaluation de serveur Azure Migrate aux projets.

[En savoir plus](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Créer et examiner une évaluation

1. Créez des évaluations pour les machines virtuelles VMware.
1. Passez en revue les évaluations en préparation de la planification de la migration.


Suivez les instructions de [ce tutoriel](tutorial-assess-vmware.md) pour configurer ces paramètres.
    

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous découvrirez comment :
 
> [!div class="checklist"] 
> * Planifier la mise à l'échelle des évaluations Azure Migrate pour les machines virtuelles VMware
> * Préparer Azure et VMware pour l’évaluation
> * Créer un projet Azure Migrate et effectuer des évaluations
> * Passer en revue les évaluations en préparation de la migration.

Maintenant, [découvrez comment](concepts-assessment-calculation.md) les évaluations sont calculées, et comment les [modifier](how-to-modify-assessment.md).
