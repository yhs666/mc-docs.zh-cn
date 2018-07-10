---
title: Azure 策略 json 示例 - 允许的 Express Route 带宽 | Azure
description: 此 json 示例策略需要 Express Route 使用一组指定的带宽。
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
ms.openlocfilehash: c7a6f9e2e0beb07120574a0dd1b67a2204e7ee17
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405157"
---
# <a name="allowed-express-route-bandwidth"></a>允许的 Express Route 带宽

此策略需要 Express Route 使用一组指定的带宽。 指定可为 Express Route 指定的 SKU 数组。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "properties": {
        "displayName": "Allowed Express Route bandwidth",
        "description": "This policy enables you to specify a set of express route bandwidths that your organization can deploy.",
        "parameters": {
            "listOfBandwidthinMbps": {
                "type": "Array",
                "metadata": {
                    "description": "The list of SKUs that can be specified for express route.",
                    "displayName": "Allowed Bandwidth"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Network/expressRouteCircuits"
                    },
                    {
                        "not": {
                            "field": "Microsoft.Network/expressRouteCircuits/serviceProvider.bandwidthInMbps",
                            "in": "[parameters('listOfBandwidthinMbps')]"
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

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fexpress-route-bandwidthInMbps%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "express-route-bandwidthInMbps" -DisplayName "Allowed Express Route bandwidth" -description "This policy enables you to specify a set of express route bandwidths that your organization can deploy." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-bandwidthInMbps/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-bandwidthInMbps/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -listOfBandwidthinMbps <Allowed Bandwidth> -PolicyDefinition $definition
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
az policy definition create --name 'express-route-bandwidthInMbps' --display-name 'Allowed Express Route bandwidth' --description 'This policy enables you to specify a set of express route bandwidths that your organization can deploy.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-bandwidthInMbps/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-bandwidthInMbps/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "express-route-bandwidthInMbps"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 有关更多示例，请参阅 [Azure 策略示例](../json-samples.md)。