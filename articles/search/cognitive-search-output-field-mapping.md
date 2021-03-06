---
title: Mapper les champs d’entrée aux champs de sortie
titleSuffix: Azure Cognitive Search
description: Extrayez et enrichissez des champs de données sources, puis mappez-les à des champs de sortie dans un index de Recherche cognitive Azure.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f0537af684632a08a39e3e681900d62238365073
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280979"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>Guide pratique pour mapper des champs enrichis par IA sur un index pouvant faire l’objet d’une recherche

Dans cet article, vous allez apprendre à mapper des champs d’entrée enrichis sur des champs de sortie dans un index pouvant faire l’objet d’une recherche. Une fois que vous avez [défini un ensemble de compétences](cognitive-search-defining-skillset.md), vous devez mapper les champs de sortie de n’importe quelle compétence qui fournit directement des valeurs sur un champ donné dans votre index de recherche. 

Les mappages de champs de sortie sont nécessaires au déplacement de contenu de documents enrichis vers l’index.  Le document enrichi est en fait une arborescence d’informations, et même si l’index prend en charge les types complexes, vous pouvez parfois souhaiter transformer les informations de l’arborescence enrichie en un type plus simple (par exemple, un tableau de chaînes). Les mappages de champs de sortie vous permettent d’effectuer des transformations de formes de données en aplatissant les informations.

## <a name="use-outputfieldmappings"></a>Utiliser outputFieldMappings
Pour mapper les champs, ajoutez `outputFieldMappings` à la définition de l’indexeur comme indiqué ci-dessous :

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

Le corps de la demande est structuré comme suit :

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```

Pour chaque mappage de champ de sortie, définissez l’emplacement des données dans l’arborescence de documents enrichis (sourceFieldName) ainsi que le nom du champ tel que référencé dans l’index (targetFieldName).

## <a name="flattening-information-from-complex-types"></a>Aplatissement d’informations à partir de types complexes 

Le chemin dans un sourceFieldName peut représenter un élément ou plusieurs éléments. Dans l’exemple ci-dessus, ```/document/content/sentiment``` représente une valeur numérique unique, tandis que ```/document/content/organizations/*/description``` représente plusieurs descriptions d’organisation. 

Dans les cas où il existe plusieurs éléments, ceux-ci sont « aplatis » en un tableau qui contient chacun des éléments. 

Plus concrètement, si nous considérons l’exemple ```/document/content/organizations/*/description```, les données du champ *descriptions* ressembleraient à un tableau de descriptions aplati avant d’être indexées :

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```

S’agissant d’un principe important, nous allons examiner un autre exemple. Imaginez que vous disposez d’un tableau de types complexes parmi l’arborescence d’enrichissement. Supposons qu’un membre appelé customEntities dispose d’un tableau de types complexes comme celui décrit ci-dessous.

```json
"document/customEntities": 
[
    {
        "name": "heart failure",
        "matches": [
            {
                "text": "heart failure",
                "offset": 10,
                "length": 12,
                "matchDistance": 0.0
            }
        ]
    },
    {
        "name": "morquio",
        "matches": [
            {
                "text": "morquio",
                "offset": 25,
                "length": 7,
                "matchDistance": 0.0
            }
        ]
    }
    //...
]
```

Par ailleurs, vous possédez une index qui comporte un champ appelé « diseases » de type Collection(Edm.String) dans lequel vous souhaitez stocker chaque nom d’entité. 

Cela peut se faire facilement en utilisant le symbole « \* », comme suit :

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/customEntities/*/name",
            "targetFieldName": "diseases"
        }
    ]
```

Cette opération a simplement pour effet d’« aplatir » chaque nom d’élément customEntities dans un même tableau de chaînes, comme ceci :

```json
  "diseases" : ["heart failure","morquio"]
```

## <a name="next-steps"></a>Étapes suivantes
Une fois que vous avez mappé les champs enrichis sur des champs pouvant faire l’objet d’une recherche, vous pouvez définir les attributs de chacun des champs pouvant faire l’objet d’une recherche [dans le cadre de la définition de l’index](search-what-is-an-index.md).

Pour en savoir plus sur le mappage de champs, consultez [Mappages de champs dans les indexeurs de Recherche cognitive Azure](search-indexer-field-mappings.md).
