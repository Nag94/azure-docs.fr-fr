---
title: Choisir le bon niveau tarifaire | Microsoft Azure Maps
description: Dans cet article, vous allez découvrir les différents niveaux tarifaires proposés par Microsoft Azure Maps.
author: farah-alyasari
ms.author: v-faalya
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 048ecdc9b96edcdebecb4cd7df0630aa991aa4d1
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210070"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Choix du bon niveau tarifaire dans Azure Maps

Azure Maps propose deux niveaux tarifaires, S0 et S1. Cet article a pour objectif de vous aider à choisir le niveau tarifaire qui répond le mieux à vos besoins. Pour choisir le niveau tarifaire correct, posez-vous les deux questions suivantes.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Combien d’utilisateurs simultanés dois-je pouvoir prendre en charge ? 
Les niveaux tarifaires S0 et S1 gèrent différents débits de données. Le niveau tarifaire S0 gère jusqu’à **50 requêtes par seconde**. Le niveau tarifaire S1 gère **plus de 50 requêtes par seconde**.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Quelles fonctionnalités géospatiales ai-je besoin d’utiliser ?
Le niveau tarifaire S0 est celui qui vous convient si les principales API géospatiales répondent aux besoins de votre service. Si vous souhaitez des fonctionnalités plus avancées pour votre application, le niveau tarifaire S1 vous convient sans doute le mieux. Les fonctionnalités avancées sont les suivantes : imagerie aérienne et hybride, obtention de zone d’itinéraire et géocodage par lot. Passez en revue le tableau **Fonctionnalités des niveaux tarifaires** pour sélectionner le niveau tarifaire le plus adapté pour votre application.

### <a name="pricing-tier-capabilities"></a>Fonctionnalités des niveaux tarifaires

| Fonctionnalité                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Rendu de carte                              | ✓                   | ✓       |
| Imagerie satellite                       |                     | ✓        |
| Recherche                                  | ✓                    | ✓        |
| Recherche par lot                            |                     | ✓        |
| Routage                                   | ✓                    |✓        |
| Routage par lot                            |                    | ✓        |
| Routage par matrice                          |                     | ✓        |
| Zone d’itinéraire (Isochrones)                |                     | ✓        |
| Trafic                                |✓                    |✓        |
| Time Zone (Fuseau horaire)                               |✓                    |✓        |
| Géolocalisation (préversion)                    |✓                   |✓        |
| Opérations spatiales                        |                    |✓        |
| Geofencing                                |                    |✓        |
| Données Azure Maps (préversion)                |                     | ✓        |
| Mobilité (préversion)                       |                     | ✓        |
| Météo (préversion)                        |✓                    |✓        |

Prenez en compte ces autres points :
* Quel est le type de votre entreprise ?
* Quelle est l’importance de votre application ?

### <a name="pricing-tier-targeted-customers"></a>Niveau tarifaire et clients ciblés

Pour mieux comprendre les niveaux tarifaires S0 et S1, consultez le tableau **Niveau tarifaire et clients ciblés**. Pour plus d’informations, consultez la page [Tarification Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Niveau tarifaire  |     Clients ciblés                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>Le niveau tarifaire S0 convient pout toutes les phases de production des applications : du développement « preuve de concept » à la phase de test initiale, en passant par la mise en production et le déploiement. Toutefois, ce niveau est conçu pour un développement à petite échelle, des clients avec peu d’utilisateurs simultanés, ou les deux. <p>|
| S1            |    <p>Le niveau tarifaire S1 convient aux clients qui ont des applications d’entreprise à grande échelle, des applications stratégiques ou un grand nombre d’utilisateurs simultanés. Il convient également aux clients qui ont besoin de services géospatiaux avancés.</p>|

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur l’affichage et la modification des niveaux tarifaires :

> [!div class="nextstepaction"] 
> [Gérer un niveau tarifaire](how-to-manage-pricing-tier.md)
