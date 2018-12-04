---
title: Azure Policy 示例 - 允许用于存储帐户和虚拟机的 SKU
description: 此示例策略要求存储帐户和虚拟机使用已批准的 SKU。
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 10/30/2017
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: mvc
ms.openlocfilehash: f533028c3a947b3b6f73a4856bd15eea088a11d5
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52650877"
---
# <a name="allowed-skus-for-storage-accounts-and-virtual-machines"></a>允许用于存储帐户和虚拟机的 SKU

此策略需要存储帐户和虚拟机使用已批准的 SKU。 使用内置策略以确保使用已批准的 SKU。 指定已批准的虚拟机 SKU 数组和已批准的存储帐户 SKU 数组。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
```json
{
   "properties": {
      "displayName": "Allowed SKUs for Storage Accounts and Virtual Machines",
      "description": "This policy allows you to specify what skus are allowed for storage accounts and virtual machines",
      "parameters": {
         "LISTOFALLOWEDSKUS_1": {
            "type": "Array",
            "metadata": {
               "displayName": "VM SKUs",
               "strongType": "vmSKUs"
            }
         },
         "LISTOFALLOWEDSKUS_2": {
            "type": "Array",
            "metadata": {
               "displayName": "Storage Account SKUs",
               "strongType": "storageSkus"
            }
         }
      },
      "policyDefinitions": [
         {
            "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/cccc23c7-8427-4f53-ad12-b6a63eb452b3",
            "parameters": {
               "listOfAllowedSKUs": {
                  "value": "[parameters('LISTOFALLOWEDSKUS_1')]"
               }
            }
         },
         {
            "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1",
            "parameters": {
               "listOfAllowedSKUs": {
                  "value": "[parameters('LISTOFALLOWEDSKUS_2')]"
               }
            }
         }
      ],
      "metadata": {
         "category": "Cost Control"
      }
   }
}
```
可使用 [Azure 门户](#deploy-with-the-portal)或将其与 [PowerShell](#deploy-with-powershell) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FSQL%2Fsql-db-skus%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/skus-for-multiple-types/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/skus-for-multiple-types/azurepolicyset.parameters.json"

$policyset= New-AzureRmPolicySetDefinition -Name "skus-for-multiple-types" -DisplayName "Allowed SKUs for Storage Accounts and Virtual Machines" -Description "This policy allows you to speficy what skus are allowed for storage accounts and virtual machines" -PolicyDefinition $policydefinitions -Parameter $policysetparameters 
 
New-AzureRmPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentName> -Scope <scope>  -LISTOFALLOWEDSKUS_1 <VM SKUs> -LISTOFALLOWEDSKUS_2 <Storage Account SKUs>
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令删除策略分配和定义。

```powershell
Remove-AzureRmPolicyAssignment -Name <assignmentName>
Remove-AzureRmPolicySetDefinitions -Name "skus-for-multiple-types"
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```cli
az policy set-definition create --name "skus-for-multiple-types" --display-name "Allowed SKUs for Storage Accounts and Virtual Machines" --description "This policy allows you to speficy what skus are allowed for storage accounts and virtual machines" --definitions "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/skus-for-multiple-types/azurepolicyset.definitions.json" --params "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/skus-for-multiple-types/azurepolicyset.parameters.json"

az policy assignment create --name <assignmentName> --scope <scope> --policy-set-definition "skus-for-multiple-types" --params "{ 'LISTOFALLOWEDSKUS_1': { 'value': <VM SKU Array> }, 'LISTOFALLOWEDSKUS_2': { 'value': <Storage Account SKU Array> } }"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令删除策略分配和定义。

```cli
az policy assignment delete --name <assignmentName>
az policy set-definition delete --name "skus-for-multiple-types"
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例