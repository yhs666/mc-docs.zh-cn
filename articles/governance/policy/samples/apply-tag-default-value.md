---
title: 示例 - 应用标记及其默认值
description: 此示例策略定义会追加指定的标记名称和值（如果未提供该标记）。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 10/30/2017
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: cb15b57b5ff91bf6275fdc8dd4011b3015d5e4d4
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58627004"
---
# <a name="sample---apply-tag-and-its-default-value"></a>示例 - 应用标记及其默认值

如果未提供标记，此策略会追加指定的标记名称和值。 由你指定要应用的标记名称和值。

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
      "displayName": "Apply tag and its default value",
      "policyType": "BuiltIn",
      "description": "Applies a required tag and its default value if it is not specified by the user.",
      "parameters": {
         "tagName": {
            "type": "String",
            "metadata": {
               "description": "Name of the tag, such as costCenter"
            }
         },
         "tagValue": {
            "type": "String",
            "metadata": {
               "description": "Value of the tag, such as headquarter"
            }
         }
      },
      "policyRule": {
         "if": {
            "field": "[concat('tags[', parameters('tagName'), ']')]",
            "exists": "false"
         },
         "then": {
            "effect": "append",
            "details": [
               {
                  "field": "[concat('tags[', parameters('tagName'), ']')]",
                  "value": "[parameters('tagValue')]"
               }
            ]
         }
      }
   },
   "id": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
   "type": "Microsoft.Authorization/policyDefinitions",
   "name": "2a0e14a6-b0a6-4fab-991a-187a4f81c498"
}
```

> [!NOTE]
> 如果手动在门户中创建策略，请使用上面的 **properties.parameters** 和 **properties.policyRule** 部分。 使用大括号 `{}` 将这两部分括在一起，使其成为有效的 JSON。

### <a name="policy-rules"></a>策略规则

定义了策略规则的 JSON，由 Azure CLI 和 Azure PowerShell 使用。

```json
{
   "if": {
      "field": "[concat('tags[', parameters('tagName'), ']')]",
      "exists": "false"
   },
   "then": {
      "effect": "append",
      "details": [
         {
            "field": "[concat('tags[', parameters('tagName'), ']')]",
            "value": "[parameters('tagValue')]"
         }
      ]
   }
}
```

### <a name="policy-parameters"></a>策略参数

定义了策略参数的 JSON，由 Azure CLI 和 Azure PowerShell 使用。

```json
{
    "tagName": {
        "type": "String",
        "metadata": {
            "description": "Name of the tag, such as costCenter"
        }
    },
    "tagValue": {
        "type": "String",
        "metadata": {
            "description": "Value of the tag, such as headquarter"
        }
    }
}
```

|Name |类型 |字段 |说明 |
|---|---|---|---|
|tagName |String |标记 |标记的名称，如 costCenter|
|tagValue |String |标记 |标记的值，如 headquarter|

通过 PowerShell 或 Azure CLI 创建分配时，可以使用 `-PolicyParameter` (PowerShell) 或 `--params` (Azure CLI) 通过字符串或文件将参数值传递为 JSON。
PowerShell 还支持 `-PolicyParameterObject`，这要求向该 cmdlet 传递一个 Name/Value 哈希表，其中，**Name** 是参数名称，**Value** 是在赋值期间传递的单个值或值数组。

在此示例参数中，定义的 _tagName_ 为 **costCenter**，_tagValue_ 为 **headquarter**。

```json
{
    "tagName": {
        "value": "costCenter"
    },
    "tagValue": {
        "value": "headquarter"
    }
}
```

## <a name="azure-portal"></a>Azure 门户


[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2Fbuilt-in-policy%2Fapply-default-tag-value%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>使用 Azure PowerShell 部署

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'allowed-custom-images' -DisplayName 'Approved VM images' -description 'This policy governs the approved VM images' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'apply-default-tag-value' -DisplayName 'Apply tag and its default value Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
```

### <a name="remove-with-azure-powershell"></a>使用 Azure PowerShell 删除

运行以下命令来删除以前的分配和定义：

```powershell
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell 说明

部署和删除脚本使用以下命令。 下表中的每条命令均链接到特定于命令的文档：

| 命令 | 注释 |
|---|---|
| [New-AzPolicyDefinition](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/New-Azpolicydefinition) | 创建新的 Azure Policy 定义。 |
| [Get-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/Get-Azresourcegroup) | 获取单个资源组。 |
| [New-AzPolicyAssignment](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/New-Azpolicyassignment) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| [Remove-AzPolicyAssignment](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/Remove-Azpolicyassignment) | 删除现有的 Azure Policy 分配。 |
| [Remove-AzPolicyDefinition](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/Remove-Azpolicydefinition) | 删除现有的 Azure Policy 定义。 |

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

```cli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'apply-default-tag-value' --display-name 'Apply tag and its default value' --description 'Applies a required tag and its default value if it is not specified by the user' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'apply-default-tag-value' --display-name 'Apply tag and its default value Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```

### <a name="remove-with-azure-cli"></a>使用 Azure CLI 进行删除

运行以下命令来删除以前的分配和定义：

```cli
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Azure CLI 说明

|                                                 命令                                                 |                                                          注释                                                          |
|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| [az policy definition create](/cli/policy/definition?view=azure-cli-latest#az-policy-definition-create) |                                         创建新的 Azure Policy 定义。                                          |
|                     [az group show](/cli/group?view=azure-cli-latest#az-group-show)                     |                                              获取单个资源组。                                              |
| [az policy assignment create](/cli/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| [az policy assignment delete](/cli/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) |                                      删除现有的 Azure Policy 分配。                                       |
| [az policy definition delete](/cli/policy/definition?view=azure-cli-latest#az-policy-definition-delete) |                                      删除现有的 Azure Policy 定义。                                       |

有多个工具可以用来与资源管理器 REST API 进行交互，例如 [ARMClient](https://github.com/projectkudu/ARMClient) 或 PowerShell。 可以在[策略定义结构](../concepts/definition-structure.md#aliases)的**别名**部分中找到通过 PowerShell 调用 REST API 的示例。

## <a name="rest-api"></a>REST API

### <a name="deploy-with-rest-api"></a>使用 REST API 进行部署

- 创建策略定义（订阅范围）。 将[策略定义](#policy-definition) JSON 用于请求正文。

  ```http
  PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/apply-default-tag-value?api-version=2016-12-01
  ```

- 创建策略分配（资源组范围）

  ```http
  PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/apply-default-tag-value-assignment?api-version=2017-06-01-preview
  ```

  将以下 JSON 示例用于请求正文：

  ```json
  {
      "properties": {
          "displayName": "Apply tag and its default value Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/apply-default-tag-value",
          "parameters": {
              "tagName": {
                  "value": "costCenter"
              },
              "tagValue": {
                  "value": "headquarter"
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>使用 REST API 进行删除

- 删除策略分配

  ```http
  DELETE https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/apply-default-tag-value-assignment?api-version=2017-06-01-preview
  ```

- 删除策略定义

  ```http
  DELETE https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/apply-default-tag-value?api-version=2016-12-01
  ```

### <a name="rest-api-explanation"></a>REST API 说明

| 服务 | 组 | 操作 | 注释 |
|---|---|---|---|
| 资源管理 | 策略定义 | [创建](https://docs.microsoft.com/zh-cn/rest/api/resources/policydefinitions/createorupdate) | 在订阅中创建新的 Azure Policy 定义。 替换项：[在管理组中创建](https://docs.microsoft.com/zh-cn/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| 资源管理 | 策略分配 | [创建](https://docs.microsoft.com/zh-cn/rest/api/resources/policyassignments/create) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| 资源管理 | 策略分配 | [删除](https://docs.microsoft.com/zh-cn/rest/api/resources/policyassignments/delete) | 删除现有的 Azure Policy 分配。 |
| 资源管理 | 策略定义 | [删除](https://docs.microsoft.com/zh-cn/rest/api/resources/policydefinitions/delete) | 删除现有的 Azure Policy 定义。 替换项：[在管理组中删除](https://docs.microsoft.com/zh-cn/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>后续步骤

- 查看其他 [Azure Policy 示例](index.md)
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)
