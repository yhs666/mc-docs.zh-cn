---
title: Azure Policy 示例 - 审核 DB 级别的威胁检测设置
description: 此示例策略对那些未设置成指定状态的 SQL 数据库安全警报策略进行审核。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 10/30/2017
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: mvc
ms.openlocfilehash: aa5ad1f95b7f0aa1c044f1c84d5bdf3ed944ece9
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409173"
---
# <a name="audit-db-level-threat-detection-setting"></a>审核 DB 级别的威胁检测设置

如果 SQL 数据库安全警报策略未设置成指定状态，则此策略将对其进行审核。 指定一个值，该值指示威胁检测是启用还是禁用状态。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "name": "audit-sql-db-threat-detection",
    "properties": {
        "displayName": "Audit DB level threat detection setting",
        "description": "Audit threat detection setting for SQL databases",
        "parameters": {
            "setting": {
                "type": "String",
                "allowedValues": [
                    "enabled",
                    "disabled"
                ],
                "metadata": {
                    "displayName": "Threat Detection Setting"
                }
            }
        },
        "policyRule": {
            "if": {
                "field": "type",
                "equals": "Microsoft.SQL/servers/databases"
            },
            "then": {
                "effect": "auditIfNotExists",
                "details": {
                    "type": "Microsoft.SQL/servers/databases/securityAlertPolicies",
                    "name": "default",
                    "existenceCondition": {
                        "field": "Microsoft.Sql/securityAlertPolicies.state",
                        "equals": "[parameters('setting')]"
                    }
                }
            }
        }
    }
}
```
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FSQL%2Faudit-sql-db-threat-detection%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "audit-sql-db-threat-detection" -DisplayName "Audit DB level threat detection setting" -description "Audit threat detection setting for SQL databases" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -setting <Threat Detection Setting> -PolicyDefinition $definition
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
az policy definition create --name 'audit-sql-db-threat-detection' --display-name 'Audit DB level threat detection setting' --description 'Audit threat detection setting for SQL databases' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-sql-db-threat-detection"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例