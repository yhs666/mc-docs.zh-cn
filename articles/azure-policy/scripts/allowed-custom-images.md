---
title: Azure Policy 示例 - 已批准的 VM 映像
description: 此策略要求在环境中仅部署已批准的自定义映像。
services: azure-policy
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
origin.date: 06/03/2018
ms.date: 07/23/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: 8cd9f7c076c58945081f78c2745b4112fcfd9958
ms.sourcegitcommit: 2a147231bf3d0a693adf58fceee76ab0fbcd6dbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2018
ms.locfileid: "39335322"
---
# <a name="approved-vm-images"></a>已批准的 VM 映像

此策略要求在环境中仅部署已批准的自定义映像。 指定已批准的映像 ID 的数组。

可以使用以下方法部署此示例策略：

- [Azure 门户](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure CLI](#azure-cli)
- [REST API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>示例策略

### <a name="policy-definition"></a>策略定义

结构完整的 JSON 策略定义，可以通过 REST API、“部署到 Azure”按钮以及手动在门户中使用。

```json
{
    "type": "Microsoft.Authorization/policyDefinitions",
    "name": "allowed-custom-images", 
    "properties": {
        "displayName": "Approved VM images",
        "description": "This policy governs the approved VM images",
        "parameters": {
            "imageIds": {
                "type": "array",
                "metadata": {
                    "description": "The list of approved VM images. Example values: '/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage' or /Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/chinaeast/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510'",
                    "displayName": "Approved VM images"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Compute/virtualMachines"
                    },
                    {
                        "not": {
                            "field": "Microsoft.Compute/imageId",
                            "in": "[parameters('imageIds')]"
                        }
                    }
                ]
            },
            "then": {
                "effect": "deny"
            }
        }
    }   
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
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines"
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "in": "[parameters('imageIds')]"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

### <a name="policy-parameters"></a>策略参数

定义了策略参数的 JSON，由 Azure CLI 和 Azure PowerShell 使用。

```json
{
    "imageIds": {
        "type": "array",
        "metadata": {
            "description": "The list of approved VM images",
            "displayName": "Approved VM images"
        }
    }
}
```

## <a name="parameters"></a>parameters

|Name |类型 |字段 |说明 |
|---|---|---|---|
|imageIds |Array |Microsoft.Compute/imageIds |已批准的 VM 映像的列表|

通过 PowerShell 或 Azure CLI 创建分配时，可以使用 `-PolicyParameter` (PowerShell) 或 `--params` (Azure CLI) 通过字符串或文件将参数值传递为 JSON。
PowerShell 还支持 `-PolicyParameterObject`，这要求向该 cmdlet 传递一个 Name/Value 哈希表，其中，**Name** 是参数名称，**Value** 是在赋值期间传递的单个值或值数组。

在此示例参数中，只会允许资源组 _YourResourceGroup_ 中的 _ContosoStdImage_ 或位于“中国东部”的 Windows Server 2016 Datacenter 的 2018 年 5 月份映像版本。

```json
{
    "imageIds": {
        "value": [
            "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
            "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/chinaeast/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
        ]
    }
}
```

## <a name="azure-portal"></a>Azure 门户

[![“部署到 Azure”](../media/deploy/deploybutton.png)](https://portal.azure.cn/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

### <a name="deploy-with-azure-powershell"></a>使用 Azure PowerShell 部署

```powershell
# Create the Policy Definition (Subscription scope)
$definition = New-AzureRmPolicyDefinition -Name 'allowed-custom-images' -DisplayName 'Approved VM images' -description 'This policy governs the approved VM images' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzureRmResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/chinaeast/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
$assignment = New-AzureRmPolicyAssignment -Name 'allowed-custom-images-assignment' -DisplayName 'Approved VM images Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
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
| [New-AzureRmPolicyDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermpolicydefinition) | 创建新的 Azure Policy 定义。 |
| [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup) | 获取单个资源组。 |
| [New-AzureRmPolicyAssignment](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermpolicyassignment) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| [Remove-AzureRmPolicyAssignment](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermpolicyassignment) | 删除现有的 Azure Policy 分配。 |
| [Remove-AzureRmPolicyDefinition](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermpolicydefinition) | 删除现有的 Azure Policy 定义。 |

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

```azurecli
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'allowed-custom-images' --display-name 'Approved VM images' --description 'This policy governs the approved VM images' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a subscription
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/chinaeast/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'allowed-custom-images-assignment' --display-name 'Approved VM images Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
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

有多个工具可以用来与资源管理器 REST API 进行交互，例如 [ARMClient](https://github.com/projectkudu/ARMClient) 或 PowerShell。 可以在[策略定义结构](../policy-definition.md#aliases)的**别名**部分中找到通过 PowerShell 调用 REST API 的示例。

### <a name="deploy-with-rest-api"></a>使用 REST API 进行部署

- 创建策略定义（订阅范围）。 将[策略定义](#policy-definition) JSON 用于请求正文。

  ```http
  PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
  ```

- 创建策略分配（资源组范围）

  ```http
  PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

  将以下 JSON 示例用于请求正文：

  ```json
  {
      "properties": {
          "displayName": "Approved VM images Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images",
          "parameters": {
              "imageIds": {
                  "value": [
                      "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
                      "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/chinaeast/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
                  ]
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>使用 REST API 进行删除

- 删除策略分配

  ```http
  DELETE https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

- 删除策略定义

  ```http
  DELETE https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
  ```

### <a name="rest-api-explanation"></a>REST API 说明

| 服务 | 组 | 操作 | 注释 |
|---|---|---|---|
| 资源管理 | 策略定义 | [创建](https://docs.microsoft.com/rest/api/resources/policydefinitions/createorupdate) | 在订阅中创建新的 Azure Policy 定义。 替代方法：[在管理组中创建](https://docs.microsoft.com/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| 资源管理 | 策略分配 | [创建](https://docs.microsoft.com/rest/api/resources/policyassignments/create) | 创建新的 Azure Policy 分配。 在此示例中，我们向其提供了一个定义，但它也可以接受计划。 |
| 资源管理 | 策略分配 | [删除](https://docs.microsoft.com/rest/api/resources/policyassignments/delete) | 删除现有的 Azure Policy 分配。 |
| 资源管理 | 策略定义 | [删除](https://docs.microsoft.com/rest/api/resources/policydefinitions/delete) | 删除现有的 Azure Policy 定义。 替代方法：[在管理组中删除](https://docs.microsoft.com/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>后续步骤

- 查看其他 [Azure Policy 示例](../json-samples.md)
- 查看 [Azure Policy 定义结构](../policy-definition.md)
- 参阅[将策略应用于 Windows VM](../../virtual-machines/windows/policy.md) 中提供的用于虚拟机的 Azure Policy 示例