---
title: Déployer des ressources dans un groupe d’administration
description: Décrit comment déployer des ressources au niveau du groupe d’administration dans un modèle Azure Resource Manager.
ms.topic: conceptual
ms.date: 03/02/2020
ms.openlocfilehash: 3b2eeaf2c63a50cda1a32fee94c1e5b99822075d
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78228097"
---
# <a name="create-resources-at-the-management-group-level"></a>Créer des ressources au niveau du groupe d’administration

En règle générale, vous déployez des ressources Azure sur un groupe de ressources dans votre abonnement Azure. Toutefois, vous pouvez également créer des ressources au niveau du groupe d'administration. Vous utilisez les déploiements au niveau du groupe d'administration pour effectuer des actions qui ont un sens à ce niveau, notamment l’affectation d’un [contrôle d’accès en fonction du rôle](../../role-based-access-control/overview.md) ou l’application de [stratégies](../../governance/policy/overview.md).

## <a name="supported-resources"></a>Ressources prises en charge

Vous pouvez déployer les types de ressources suivants au niveau de groupe d'administration :

* [deployments](/azure/templates/microsoft.resources/deployments)
* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>schéma

Le schéma que vous utilisez pour les déploiements au niveau du groupe d'administration est différent de celui utilisé pour les déploiements de groupes de ressources.

Pour les modèles, utilisez :

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

Fichiers de fichiers de paramètres, utilisez :

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentParameters.json#
```

## <a name="deployment-commands"></a>Commandes de déploiement

Les commandes utilisées pour les déploiements de groupes d’administration sont différentes de celles utilisées pour les déploiements de groupes de ressources.

Pour Azure PowerShell, utilisez [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment). 

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -ManagementGroupId "myMG" `
  -Location "West US" `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json
```

Pour l’API REST, utilisez [Déploiements - Créer au niveau du groupe d’administration](/rest/api/resources/deployments/createorupdateatmanagementgroupscope).

## <a name="deployment-location-and-name"></a>Emplacement et nom du déploiement

Pour les déploiements au niveau du groupe d'administration, vous devez fournir un emplacement de déploiement. L’emplacement du déploiement est distinct de l’emplacement des ressources que vous déployez. L’emplacement de déploiement indique où stocker les données de déploiement.

Vous pouvez fournir un nom de déploiement ou utiliser le nom de déploiement par défaut. Le nom par défaut est le nom du fichier de modèle. Par exemple, le déploiement d’un modèle nommé **azuredeploy.json** crée le nom de déploiement par défaut **azuredeploy**.

Pour chaque nom de déploiement, l’emplacement est immuable. Il n’est pas possible de créer un déploiement dans un emplacement s’il existe un déploiement du même nom dans un autre emplacement. Si vous obtenez le code d’erreur `InvalidDeploymentLocation`, utilisez un autre nom ou le même emplacement que le déploiement précédent pour ce nom.

## <a name="use-template-functions"></a>Utiliser des fonctions de modèle

Pour les déploiements au niveau du groupe d'administration, il existe quelques considérations importantes liées à l’utilisation des fonctions de modèle :

* La fonction [resourceGroup()](template-functions-resource.md#resourcegroup)**n’est pas** prise en charge.
* La fonction [subscription()](template-functions-resource.md#subscription) n’est **pas** prise en charge.
* Les fonctions [reference()](template-functions-resource.md#reference) et [list()](template-functions-resource.md#list) sont prises en charge.
* La fonction [resourceId()](template-functions-resource.md#resourceid) est prise en charge. Elle permet d’obtenir l’ID des ressources qui sont utilisées dans les déploiements au niveau du groupe d'administration. Ne fournissez pas de valeur pour le paramètre de groupe de ressources.

  Par exemple, pour obtenir l’ID de ressource d’une définition de stratégie, utilisez :
  
  ```json
  resourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  L’ID de ressource retourné possède le format suivant :
  
  ```json
  /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-policies"></a>Création des stratégies

### <a name="define-policy"></a>Définir une stratégie

L'exemple suivant montre comment [définir](../../governance/policy/concepts/definition-structure.md) une stratégie au niveau du groupe d'administration.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
      "properties": {
        "policyType": "Custom",
        "parameters": {},
        "policyRule": {
          "if": {
            "field": "location",
            "equals": "northeurope"
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    }
  ]
}
```

### <a name="assign-policy"></a>Attribuer la stratégie

L’exemple suivant assigne une définition de stratégie existante au groupe d'administration. Si la stratégie utilise des paramètres, fournissez-les en tant qu’objet. Si la stratégie n’utilise pas de paramètres, utilisez l’objet vide par défaut.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string"
    },
    "policyName": {
      "type": "string"
    },
    "policyParameters": {
      "type": "object",
      "defaultValue": {}
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
      "properties": {
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

## <a name="template-sample"></a>Exemple de modèle

* [Créez un groupe de ressources, une stratégie et une attribution de stratégie](https://github.com/Azure/azure-docs-json-samples/blob/master/management-level-deployment/azuredeploy.json).

## <a name="next-steps"></a>Étapes suivantes

* Pour en savoir plus sur l’attribution de rôles, voir [Gérer l’accès aux ressources Azure avec RBAC et les modèles Azure Resource Manager](../../role-based-access-control/role-assignments-template.md).
* Pour obtenir un exemple de déploiement des paramètres d’espace de travail pour Azure Security Center, consultez [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Pour en savoir plus sur la création de modèles Azure Resource Manager, consultez [Création de modèles](template-syntax.md).
* Pour obtenir la liste des fonctions disponibles dans un modèle, consultez [Fonctions de modèle](template-functions.md).