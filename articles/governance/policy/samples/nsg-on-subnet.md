---
title: 示例 - 每个子网上的 NSG X
description: 此示例策略定义要求每个虚拟子网使用特定网络安全组。
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 01/23/2019
ms.date: 10/12/2019
ms.author: v-tawe
ms.openlocfilehash: 0d5e47d357f1e19f02fa0b57770bff7e86d460d9
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526578"
---
# <a name="sample---network-security-group-x-on-every-subnet"></a>示例 - 每个子网上的网络安全组 x

此策略要求每个虚拟子网使用特定网络安全组。 指定要使用的网络安全组的 ID。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "type": "Microsoft.Authorization/policyDefinitions",
    "name": "enforce-nsg-on-subnet",
    "properties": {
        "displayName": "NSG X on every subnet",
        "description": "This policy enforces a specific NSG on every subnet",
        "mode": "all",
        "parameters": {
            "nsgId": {
                "type": "String",
                "metadata": {
                    "description": "Resource Id of the Network Security Group",
                    "displayName": "NSG Id"
                }
            }
        },
        "policyRule": {
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
                                    "field": "Microsoft.Network/virtualNetworks/subnets[*].networkSecurityGroup.id",
                                    "equals": "[parameters('nsgId')]"
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
                                "not": {
                                    "field": "Microsoft.Network/virtualNetworks/subnets/networkSecurityGroup.id",
                                    "equals": "[parameters('nsgId')]"
                                }
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

[![将策略示例部署到 Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fenforce-nsg-on-subnet%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = New-AzPolicyDefinition -Name "enforce-nsg-on-subnet" -DisplayName "NSG X on every subnet" -description "This policy enforces a specific NSG on every subnet" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -nsgId <NSG Id> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli
az policy definition create --name 'enforce-nsg-on-subnet' --display-name 'NSG X on every subnet' --description 'This policy enforces a specific NSG on every subnet' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-nsg-on-subnet"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例