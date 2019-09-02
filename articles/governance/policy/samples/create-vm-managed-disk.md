---
title: 示例 - 审核未使用托管磁盘的 VM
description: 如果未使用托管磁盘创建虚拟机，则此 json 示例定义会进行审核。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origindate: 10/30/2017
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: 29e802dbfe7a6232304b79141c24f3435743c52f
ms.sourcegitcommit: 1e5ca29cde225ce7bc8ff55275d82382bf957413
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "56903226"
---
# <a name="sample---audit-when-vm-does-not-use-managed-disk"></a>示例 - 在 VM 未使用托管磁盘时审核

如果创建了未使用托管磁盘的虚拟机，则进行审核。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
  "properties": {
    "displayName":"Create VM using Managed Disk",
    "description":"Create VM using Managed Disk",
    "policyRule": {
      "if": {
        "anyOf": [
          {
            "allOf": [
              {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines"
              },
              {
                "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
                "exists": true
              }
            ]
          },
          {
            "allOf": [
              {
                "field": "type",
                "equals": "Microsoft.Compute/VirtualMachineScaleSets"
              },
              {
                "anyOf": [
                  {
                    "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                    "exists": true
                  },
                  {
                    "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                    "exists": true
                  }
                ]
              }
            ]
          }
        ]
      },
      "then": {
        "effect": "audit"
      }
    }
  }
}
```
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Faudit-non-managed-disk-vm%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = New-AzPolicyDefinition -Name "audit-non-managed-disk-vm" -DisplayName "Create VM using Managed Disk" -description "Create VM using Managed Disk" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/audit-non-managed-disk-vm/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/audit-non-managed-disk-vm/azurepolicy.parameters.json' -Mode All
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
az policy definition create --name 'audit-non-managed-disk-vm' --display-name 'Create VM using Managed Disk' --description 'Create VM using Managed Disk' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/audit-non-managed-disk-vm/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/audit-non-managed-disk-vm/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-non-managed-disk-vm"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例