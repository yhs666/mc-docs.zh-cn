---
title: Azure 策略 json 示例 - 允许的负载均衡器 SKU | Azure
description: 此 json 示例策略需要负载均衡器使用已批准的 SKU。
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
origin.date: 10/30/2017
ms.date: 07/09/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: a184790110b9f0231b9622fe65a1e33d6a640c6f
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405302"
---
# <a name="allowed-load-balancer-skus"></a>允许的负载均衡器 SKU

此策略需要负载均衡器使用已批准的 SKU。 指定允许的 SKU 的数组。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "properties": {
        "displayName": "Allowed Load Balancer SKUs",
        "description": "This policy enables you to specify a set of load balancer SKUs that your organization can deploy.",
        "parameters": {
            "listOfAllowedSKUs": {
                "type": "Array",
                "metadata": {
                    "description": "The list of SKUs that can be specified for load balancer.",
                    "displayName": "Allowed SKUs"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Network/loadbalancers"
                    },
                    {
                        "not": {
                            "field": "Microsoft.Network/loadbalancers/sku.name",
                            "in": "[parameters('listOfAllowedSKUs')]"
                        }
                    }
                ]
            },
            "then": {
                "effect": "Deny"
            }
        }
    }
}
```
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fload-balancer-skus%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "load-balancer-skus" -DisplayName "Allowed Load Balancer SKUs" -description "This policy enables you to specify a set of load balancer SKUs that your organization can deploy." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -listOfAllowedSKUs <Allowed SKUs> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```cli
az policy definition create --name 'load-balancer-skus' --display-name 'Allowed Load Balancer SKUs' --description 'This policy enables you to specify a set of load balancer SKUs that your organization can deploy.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "load-balancer-skus"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 有关更多示例，请参阅 [Azure 策略示例](../json-samples.md)。