---
title: 示例 - 允许的 SQL DB SKU
description: 此示例策略定义要求 SQL 数据库使用已批准的 SKU。
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 01/23/2019
ms.date: 10/12/2019
ms.author: v-tawe
ms.openlocfilehash: 3a1b95c86826bb9958bfc33f437668680f9072ec
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526658"
---
# <a name="sample---allowed-sql-db-skus"></a>示例 - 允许的 SQL DB SKU

此策略要求 SQL 数据库使用已批准的 SKU。 指定一个允许的 SKU ID 的数组或允许的 SKU 名称的数组。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
    "properties": {
        "displayName": "Allowed SQL DB SKUs",
        "description": "This policy enables you to specify a set of SQL DB SKUs",
        "parameters": {
            "listOfSKUId": {
                "type": "Array",
                "metadata": {
                    "description": "The list of SKUs that can be specified for SQL Databases.",
                    "displayName": "Allowed SKU IDs"
                }
            },
            "listOfSKUName": {
                "type": "Array",
                "metadata": {
                    "description": "The list of SKUs that can be specified for SQL Databases.",
                    "displayName": "Allowed SKU Names"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.SQL/servers/databases"
                    },
                    {
                        "not": {
                            "anyOf": [
                                {
                                    "field": "Microsoft.SQL/servers/databases/requestedServiceObjectiveId",
                                    "in": "[parameters('listOfSKUId')]"
                                },
                                {
                                    "field": "Microsoft.SQL/servers/databases/requestedServiceObjectiveName",
                                    "in": "[parameters('listOfSKUName')]"
                                }
                            ]
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

[![将策略示例部署到 Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FSQL%2Fsql-db-skus%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]


```powershell
$definition = New-AzPolicyDefinition -Name "sql-db-skus" -DisplayName "Allowed SQL DB SKUs" -description "This policy enables you to specify a set of SQL DB SKUs" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/sql-db-skus/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/sql-db-skus/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -listOfSKUId <Allowed SKU IDs> -listOfSKUName <Allowed SKU Names> -PolicyDefinition $definition
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
az policy definition create --name 'sql-db-skus' --display-name 'Allowed SQL DB SKUs' --description 'This policy enables you to specify a set of SQL DB SKUs' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/sql-db-skus/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/sql-db-skus/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "sql-db-skus"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例