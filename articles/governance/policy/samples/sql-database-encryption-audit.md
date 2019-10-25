---
title: 示例 - 审核 SQL 数据库的透明数据加密
description: 如果 SQL 数据库未启用透明数据加密，则此示例策略定义会进行审核。
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 01/23/2019
ms.date: 10/12/2019
ms.author: v-tawe
ms.openlocfilehash: fd603f0b7aaabffac908fd411a48075ef1b28abe
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526576"
---
# <a name="sample---audit-sql-database-encryption"></a>示例 - 审核 SQL 数据库加密

如果 SQL 数据库未启用透明数据加密，则此内置策略会进行审核。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板

```json
{
   "properties": {
      "displayName": "Audit transparent data encryption status",
      "description": "Audit transparent data encryption status for SQL databases",
      "mode": "Indexed",
      "parameters": {
         "effect": {
            "type": "string",
            "defaultValue": "AuditIfNotExists",
            "allowedValues": [
               "AuditIfNotExists",
               "Disabled"
            ],
            "metadata": {
               "displayName": "Effect",
               "description": "Enable or disable the execution of the policy"
            }
         }
      },
      "policyRule": {
         "if": {
            "allOf": [
               {
                  "field": "type",
                  "equals": "Microsoft.Sql/servers/databases"
               },
               {
                  "field": "name",
                  "notEquals": "master"
               }
            ]
         },
         "then": {
            "effect": "[parameters('effect')]",
            "details": {
               "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
               "name": "current",
               "existenceCondition": {
                  "allOf": [
                     {
                        "field": "Microsoft.Sql/transparentDataEncryption.status",
                        "equals": "enabled"
                     }
                  ]
               }
            }
         }
      }
   }
}
```

可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。 若要获取内置策略，请使用 ID `17k78e20-9358-41c9-923c-fb736d382a12`。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

分配策略时，请从可用的内置定义中选择“审核透明数据加密状态”  。

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/17k78e20-9358-41c9-923c-fb736d382a12

New-AzPolicyAssignment -name "SQL TDE Audit" -PolicyDefinition $definition -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令删除策略分配。

```powershell
Remove-AzPolicyAssignment -Name "SQL TDE Audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli
az policy assignment create --scope <scope> --name "SQL TDE Audit" --policy 17k78e20-9358-41c9-923c-fb736d382a12
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令删除策略分配。

```azurecli
az policy assignment delete --name "SQL TDE Audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例