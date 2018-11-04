---
title: Azure Policy 示例 - 审核诊断设置
description: 如果未对指定资源类型启用诊断设置，此示例策略会进行审核。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 04/27/2018
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: mvc
ms.openlocfilehash: 4f750e56f6cfe3334c40b7541d9729bf032b97b3
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409045"
---
# <a name="audit-diagnostic-setting"></a>审核诊断设置

如果未对指定资源类型启用诊断设置，则内置策略会进行审核。 指定一个资源类型的数组以检查是否已启用诊断设置。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "name": "audit-diagnostic-setting",
    "properties": {
        "displayName": "Audit diagnostic setting",
        "description": "Audit diagnostic setting for selected resource types",
        "mode": "all",
        "parameters": {
            "listOfResourceTypes": {
                "type": "Array",
                "metadata": {
                    "displayName": "Resource Types",
                    "strongType": "resourceTypes"
                }
            }
        },
        "policyRule": {
            "if": {
                "field": "type",
                "in": "[parameters('listOfResourceTypes')]"
            },
            "then": {
                "effect": "auditIfNotExists",
                "details": {
                    "type": "Microsoft.Insights/diagnosticSettings",
                    "existenceCondition": {
                        "allOf": [
                            {
                                "field": "Microsoft.Insights/diagnosticSettings/logs.enabled",
                                "equals": "true"
                            },
                            {
                                "field": "Microsoft.Insights/diagnosticSettings/metrics.enabled",
                                "equals": "true"
                            }
                        ]
                    }
                }
            }
        }
    }
}
```
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。 若要获取内置策略，请使用 ID `7f89b1eb-583c-429a-8828-af049802c1d9`。

## <a name="parameters"></a>parameters

若要传入参数值，请使用以下格式：

```json
{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}
```

## <a name="deploy-with-the-portal"></a>使用门户进行部署

分配策略时，请从可用的内置定义中选择“审核诊断设置”。

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/7f89b1eb-583c-429a-8828-af049802c1d9

New-AzureRmPolicyAssignment -name "Audit diagnostics" -PolicyDefinition $definition -PolicyParameter '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmPolicyAssignment -Name "Audit diagnostics" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```cli
az policy assignment create --scope <scope> --name "Audit diagnostics" --policy 7f89b1eb-583c-429a-8828-af049802c1d9 --params '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}'
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az policy assignment delete --name "Audit diagnostics" --resource-group myResourceGroup
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例