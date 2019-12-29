---
title: 示例 - 强制实施标记及其值
description: 此示例策略定义要求在每个 Azure 资源的参数中定义标记和值。
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 01/23/2019
ms.date: 10/12/2019
ms.author: v-tawe
ms.openlocfilehash: bb026ca331e1a80a703ee0604e350d54ae61ffed
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335774"
---
# <a name="sample---enforce-tag-and-its-value"></a>示例 - 强制实施标记及其值

此策略需要一个指定的标记名称和值。 指定要强制实施的标记名称和值。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
   "properties": {
      "displayName": "Enforce tag and its value",
      "policyType": "BuiltIn",
      "description": "Enforces a required tag and its value.",
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
            "not": {
               "field": "[concat('tags[', parameters('tagName'), ']')]",
               "equals": "[parameters('tagValue')]"
            }
         },
         "then": {
            "effect": "deny"
         }
      }
   },
   "id": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
   "type": "Microsoft.Authorization/policyDefinitions",
   "name": "1e30110a-5ceb-460c-a204-c1c3969c6d62"
}
```
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![将策略示例部署到 Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2Fbuilt-in-policy%2Fenforce-tag-value%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = New-AzPolicyDefinition -Name "enforce-tag-value" -DisplayName "Enforce tag and its value" -description "Enforces a required tag and its value." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -tagName <tagName> -tagValue <tagValue> -PolicyDefinition $definition
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
az policy definition create --name 'enforce-tag-value' --display-name 'Enforce tag and its value' --description 'Enforces a required tag and its value.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-tag-value"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例
