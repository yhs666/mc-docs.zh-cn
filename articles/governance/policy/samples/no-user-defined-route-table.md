---
title: 示例 - 无用户定义的路由表
description: 此示例策略定义禁止通过用户定义的路由表部署虚拟网络。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 09/18/2018
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: 581311b778b7c24d99d6547a3779f463ba767bc8
ms.sourcegitcommit: 1e5ca29cde225ce7bc8ff55275d82382bf957413
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "56903070"
---
# <a name="sample---no-user-defined-route-table"></a>示例 - 无用户定义的路由表

此策略禁止通过用户定义的路由表部署虚拟网络。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "type": "Microsoft.Authorization/policyDefinitions",
    "name": "no-route-table-in-ER-Network",
    "properties": {
        "mode": "all",
        "parameters": {},
        "displayName": "No User Defined Route Table",
        "description": "Forbid virtual networks to use user defined route table",
        "policyRules": {
            "if": {
                "anyOf": [
                    {
                        "allOf": [
                            {
                                "field": "type",
                                "equals": "Microsoft.Network/virtualNetworks"
                            },
                            {
                                "not": {
                                    "field": "Microsoft.Network/virtualNetworks/subnets[*].routeTable.id",
                                    "exists": false
                                }
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "type",
                                "equals": "Microsoft.Network/virtualNetworks/subnets"
                            },
                            {
                                "field": "Microsoft.Network/virtualNetworks/subnets/routeTable.id",
                                "exists": true
                            }
                        ]
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
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fno-route-table-in-ER-Network%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = New-AzPolicyDefinition -Name "no-route-table-in-ER-Network" -DisplayName "No User Defined Route Table" -description "Forbid virtual networks to use user defined route table" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/no-route-table-in-ER-Network/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/no-route-table-in-ER-Network/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```cli
az policy definition create --name 'no-route-table-in-ER-Network' --display-name 'No User Defined Route Table' --description 'Forbid virtual networks to use user defined route table' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/no-route-table-in-ER-Network/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/no-route-table-in-ER-Network/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "no-route-table-in-ER-Network"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例
