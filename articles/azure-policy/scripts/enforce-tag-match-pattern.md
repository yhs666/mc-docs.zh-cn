---
title: Azure 策略 json 示例 - 强制实施标记匹配模式 | Azure
description: 此 json 示例策略需要资源符合标记值的匹配模式。
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
origin.date: 11/13/2017
ms.date: 07/09/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: cd2420ec2da80c95eb9e33309174d5e1e8773e49
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405278"
---
# <a name="enforce-tag-match-pattern-for-tag-values"></a>强制实施标记值的标记匹配模式

需要标记值符合匹配模式。 在策略规则中指定允许的模式。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "properties": {
        "displayName": "Use match condition on tag value.",
        "description": "Enforce a text pattern on tag value.",
        "mode": "all",
        "policyRule": {
            "if": {
                "not": {
                    "field": "tags.date",
                    "match": "##-???-####"
                }
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

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FTextPatterns%2Fenforce-tag-match-pattern%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "enforce-tag-match-pattern" -DisplayName "Ensure that a tag value matches a text pattern." -description "Ensure that a tag value matches a text pattern." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
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
az policy definition create --name 'enforce-tag-match-pattern' --display-name 'Ensure that a tag value matches a text pattern.' --description 'Ensure that a tag value matches a text pattern.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-tag-match-pattern/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-tag-match-pattern"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 有关更多示例，请参阅 [Azure 策略示例](../json-samples.md)。