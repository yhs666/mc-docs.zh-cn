---
title: 示例 - 允许的位置
description: 此示例策略要求所有资源都部署到已批准的位置。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 12/12/2018
ms.date: 01/14/2019
ms.author: v-biyu
ms.openlocfilehash: 6ee8b4b20bf5dadb86964eb9ff4a0f87db4a7a11
ms.sourcegitcommit: 0cb57e97931b392d917b21753598e1bd97506038
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906060"
---
# <a name="allowed-locations"></a>允许的位置

通过此策略，可限制组织在部署资源时可指定的位置。 用于强制执行异地符合性要求。 排除资源组、Microsoft.AzureActiveDirectory/b2cDirectories 以及使用“全局”区域的资源。 由你指定允许的位置的数组。

可以使用以下方法部署此示例策略：

- [Azure 门户](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure CLI](#azure-cli)
- [REST API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>示例策略

### <a name="policy-definition"></a>策略定义

结构完整的 JSON 策略定义，可以通过 REST API、“部署到 Azure”按钮以及手动在门户中使用。
```json
{
  "properties": {
    "displayName": "Allowed locations",
    "policyType": "BuiltIn",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements. Excludes resource groups, Microsoft.AzureActiveDirectory/b2cDirectories, and resources that use the 'global' region.",
    "mode": "Indexed",
    "parameters": {
      "listOfAllowedLocations": {
        "type": "Array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "location",
            "notIn": "[parameters('listOfAllowedLocations')]"
          },
          {
            "field": "location",
            "notEquals": "global"
          },
          {
            "field": "type",
            "notEquals": "Microsoft.AzureActiveDirectory/b2cDirectories"
          }
        ]
      },
      "then": {
        "effect": "Deny"
      }
    }
  },
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "type": "Microsoft.Authorization/policyDefinitions",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c"
}
```

> [!NOTE]
> 如果手动在门户中创建策略，请使用上面的 **properties.parameters** 和 **properties.policyRule** 部分。 使用大括号 `{}` 将这两部分括在一起，使其成为有效的 JSON。

### <a name="policy-rules"></a>策略规则

定义了策略规则的 JSON，由 Azure CLI 和 Azure PowerShell 使用。
```json

{
    "if": {
        "allOf": [
            {
                "field": "location",
                "notIn": "[parameters('listOfAllowedLocations')]"
            },
            {
                "field": "location",
                "notEquals": "global"
            },
            {
                "field": "type",
                "notEquals": "Microsoft.AzureActiveDirectory/b2cDirectories"
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}
```

### <a name="policy-parameters"></a>策略参数

定义了策略参数的 JSON，由 Azure CLI 和 Azure PowerShell 使用。
```json

{
    "listOfAllowedLocations": {
        "type": "Array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying resources.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}
```

## <a name="parameters"></a>parameters

|Name |类型 |字段 |说明 |
|---|---|---|---|
|listOfAllowedLocations |Array |位置 |允许的位置的列表|

通过 PowerShell 或 Azure CLI 创建分配时，可以使用 `-PolicyParameter` (PowerShell) 或 `--params` (Azure CLI) 通过字符串或文件将参数值传递为 JSON。
PowerShell 还支持 `-PolicyParameterObject`，这要求向该 cmdlet 传递一个 Name/Value 哈希表，其中，**Name** 是参数名称，**Value** 是在赋值期间传递的单个值或值数组。

在此示例参数中，仅允许 _chinaeast2_ 或 _chinaeast_ 位置。

```json
{
    "listOfAllowedLocations": {
        "value": [
            "chinaeast2",
            "chinaeast"
        ]
    }
}
```

## <a name="azure-portal"></a>Azure 门户


[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2Fbuilt-in-policy%2Fallowed-locations%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

### <a name="deploy-with-azure-powershell"></a>使用 Azure PowerShell 部署

```azurepowershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzureRmPolicyDefinition -Name "allowed-locations" -DisplayName "Allowed locations" -description "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements. Excludes resource groups, Microsoft.AzureActiveDirectory/b2cDirectories, and resources that use the 'global' region." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/allowed-locations/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/allowed-locations/azurepolicy.parameters.json' -Mode Indexed

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzureRmResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "listOfAllowedLocations": { "value": [ "chinaeast2", "chinaeast" ] } }'

# Create the Policy Assignment
$assignment = New-AzureRmPolicyAssignment -Name 'allowed-locations-assignment' -DisplayName 'Allowed locations Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
```

### <a name="remove-with-azure-powershell"></a>使用 Azure PowerShell 删除

运行以下命令来删除以前的分配和定义：

```azurepowershell
# Remove the Policy Assignment
Remove-AzureRmPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzureRmPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell 说明

部署和删除脚本使用以下命令。 下表中的每条命令均链接到特定于命令的文档：

| 命令 | 注释 |
|---|---|
| [New-AzureRmPolicyDefinition](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/new-azurermpolicydefinition) | 创建新的 Azure Policy 定义。 |
| [Get-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/get-azurermresourcegroup) | 获取单个资源组。 |
| [New-AzureRmPolicyAssignment](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/new-azurermpolicyassignment) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| [Remove-AzureRmPolicyAssignment](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/remove-azurermpolicyassignment) | 删除现有的 Azure Policy 分配。 |
| [Remove-AzureRmPolicyDefinition](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/remove-azurermpolicydefinition) | 删除现有的 Azure Policy 定义。 |

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

```
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'allowed-locations' --display-name 'Allowed locations' --description 'This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements. Excludes resource groups, Microsoft.AzureActiveDirectory/b2cDirectories, and resources that use the 'global' region.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/allowed-locations/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/allowed-locations/azurepolicy.parameters.json' --mode Indexed)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "listOfAllowedLocations": { "value": [ "chinaeast2", "chinaeast" ] } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'allowed-locations-assignment' --display-name 'Allowed locations Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```

### <a name="remove-with-azure-cli"></a>使用 Azure CLI 进行删除

运行以下命令来删除以前的分配和定义：

```azurecli
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Azure CLI 说明

| 命令 | 注释 |
|---|---|
| [az policy definition create](/cli/policy/definition?view=azure-cli-latest#az-policy-definition-create) | 创建新的 Azure Policy 定义。 |
| [az group show](/cli/group?view=azure-cli-latest#az-group-show) | 获取单个资源组。 |
| [az policy assignment create](/cli/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| [az policy assignment delete](/cli/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | 删除现有的 Azure Policy 分配。 |
| [az policy definition delete](/cli/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | 删除现有的 Azure Policy 定义。 |
## <a name="rest-api"></a>REST API

有多个工具可以用来与资源管理器 REST API 进行交互，例如 [ARMClient](https://github.com/projectkudu/ARMClient) 或 PowerShell。

### <a name="deploy-with-rest-api"></a>使用 REST API 进行部署

- 创建策略定义（订阅范围）。 将[策略定义](#policy-definition) JSON 用于请求正文。

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-locations?api-version=2018-05-01
  ```

- 创建策略分配（资源组范围）

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/allowed-locations-assignment?api-version=2018-05-01
  ```

  将以下 JSON 示例用于请求正文：

  ```json
  {
      "properties": {
          "displayName": "Allowed locations Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/allowed-locations",
          "parameters": {
              "listOfAllowedLocations": {
                  "value": [
                      "chinaeast2",
                      "chinaeast"
                  ]
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>使用 REST API 进行删除

- 删除策略分配

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/allowed-locations-assignment?api-version=2018-05-01
  ```

- 删除策略定义

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-locations?api-version=2018-05-01
  ```

### <a name="rest-api-explanation"></a>REST API 说明

| 服务 | 组 | 操作 | 注释 |
|---|---|---|---|
| 资源管理 | 策略定义 | [创建](https://docs.microsoft.com/zh-cn/rest/api/resources/policydefinitions/createorupdate) | 在订阅中创建新的 Azure Policy 定义。 替换项：[在管理组中创建](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| 资源管理 | 策略分配 | [创建](https://docs.microsoft.com/zh-cn/rest/api/resources/policyassignments/create) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| 资源管理 | 策略分配 | [删除](https://docs.microsoft.com/zh-cn/rest/api/resources/policyassignments/delete) | 删除现有的 Azure Policy 分配。 |
| 资源管理 | 策略定义 | [删除](https://docs.microsoft.com/zh-cn/rest/api/resources/policydefinitions/delete) | 删除现有的 Azure Policy 定义。 替换项：[在管理组中删除](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>后续步骤

- 查看其他 [Azure Policy 示例](index.md)
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)