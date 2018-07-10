---
title: 使用 Azure 策略以编程方式创建策略和查看符合性数据
description: 本文逐步讲解如何以编程方式创建和管理适用于 Azure 策略的策略。
services: azure-policy
author: WenJason
ms.author: v-nany
origin.date: 05/24/2018
ms.date: 07/09/2018
ms.topic: conceptual
ms.service: azure-policy
manager: digimobile
ms.openlocfilehash: c07af12cff7b404acd54cd01b83fd19dc6e0ab5d
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405190"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>以编程方式创建策略和查看符合性数据

本文逐步讲解如何以编程方式创建和管理策略。 此外，还介绍如何查看资源符合性状态和策略。 策略定义对资源强制实施不同的规则和效果。 强制实施可确保资源始终符合企业标准和服务级别协议。

## <a name="prerequisites"></a>先决条件

在开始之前，请确保满足以下先决条件：

1. 安装 [ARMClient](https://github.com/projectkudu/ARMClient)（如果尚未安装）。 该工具可将 HTTP 请求发送到基于 Azure 资源管理器的 API。
2. 将 AzureRM PowerShell 模块更新到最新版本。 有关最新版本的详细信息，请参阅 [Azure PowerShell](https://github.com/Azure/azure-powershell/releases)。
3. 使用 Azure PowerShell 注册策略见解资源提供程序，以确保订阅可使用资源提供程序。 若要注册资源提供程序，必须具有为资源提供程序执行注册操作的权限。 此操作包含在“参与者”和“所有者”角色中。 运行以下命令，注册资源提供程序：

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  有关注册和查看资源提供程序的详细信息，请参阅[资源提供程序和类型](../azure-resource-manager/resource-manager-supported-services.md)。
4. 安装 Azure CLI（如果尚未安装）。 可以通过[在 Windows 上安装 Azure CLI 2.0](/cli/install-azure-cli-windows) 获取最新版本。

## <a name="create-and-assign-a-policy-definition"></a>创建并分配策略定义

更清晰地洞察资源的第一步是针对资源创建并分配策略。 下一步是了解如何以编程方式创建和分配策略。 示例策略使用 PowerShell、Azure CLI 和 HTTP 请求来审核向所有公共网络开放的存储帐户。

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>使用 PowerShell 创建并分配策略定义

1. 使用以下 JSON 代码片段创建名为 AuditStorageAccounts.json 的 JSON 文件。

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

  有关编写策略定义的详细信息，请参阅 [Azure 策略定义结构](policy-definition.md)。
2. 运行以下命令，使用 AuditStorageAccounts.json 文件创建策略定义。

  ```powershell
  New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
  ```

  该命令创建名为 _Audit Storage Accounts Open to Public Networks_ 的策略定义。 有关可用的其他参数的详细信息，请参阅 [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition)。
3. 创建策略定义后，可运行以下命令创建策略分配：

  ```powershell
  $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
  $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
  New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
  ```

  将 _ContosoRG_ 替换为所需资源组的名称。

有关使用 Azure 资源管理器 PowerShell 模块管理资源策略的详细信息，请参阅 [AzureRM.Resources](/powershell/module/azurerm.resources/#policies)。

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>使用 ARMClient 创建并分配策略定义

使用以下过程创建策略定义。

1. 复制以下 JSON 代码片段以创建 JSON 文件。 在下一步骤中将会调用该文件。

  ```json
  "properties": {
      "displayName": "Audit Storage Accounts Open to Public Networks",
      "policyType": "Custom",
      "mode": "Indexed",
      "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
      "parameters": {},
      "policyRule": {
          "if": {
              "allOf": [{
                      "field": "type",
                      "equals": "Microsoft.Storage/storageAccounts"
                  },
                  {
                      "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                      "equals": "Allow"
                  }
              ]
          },
          "then": {
              "effect": "audit"
          }
      }
  }
  ```

2. 使用以下调用之一创建策略定义：

  ```
  # For defining a policy in a subscription
  armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

  Replace the preceding {subscriptionId} with the ID of your subscription .

For more information about the structure of the query, see [Policy Definitions – Create or Update](/rest/api/resources/policydefinitions/createorupdate).

Use the following procedure to create a policy assignment and assign the policy definition at the resource group level.

1. Copy the following JSON snippet to create a JSON policy assignment file. Replace example information in &lt;&gt; symbols with your own values.

  ```json
  {
      "properties": {
          "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
          "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
          "parameters": {},
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
          "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
      }
  }
  ```

2. 使用以下调用创建策略分配：

  ```
  armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
  ```

  请将 &lt;&gt; 符号中的示例信息替换为自己的值。

  有关向 REST API 发出 HTTP 调用的详细信息，请参阅 [Azure REST API 资源](/rest/api/resources/)。

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>使用 Azure CLI 创建并分配策略定义

若要创建策略定义，请使用以下过程：

1. 复制以下 JSON 代码片段以创建 JSON 策略分配文件。

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

2. 运行以下命令创建策略定义：

  ```cli
az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
  ```

3. 使用以下命令创建策略分配。 请将 &lt;&gt; 符号中的示例信息替换为自己的值。

  ```cli
  az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
  ```

可以在 PowerShell 中使用以下命令获取策略定义 ID：

```cli
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

创建的策略定义的策略定义 ID 应如以下示例所示：

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

有关如何使用 Azure CLI 管理资源策略的详细信息，请参阅 [Azure CLI 资源策略](/cli/policy?view=azure-cli-latest)。

## <a name="next-steps"></a>后续步骤

查看以下文章，详细了解本文中所示的命令和查询。

- [Azure REST API 资源](/rest/api/resources/)
- [Azure RM PowerShell 模块](/powershell/module/azurerm.resources/#policies)
- [Azure CLI 策略命令](/cli/policy?view=azure-cli-latest)
- [策略见解资源提供程序 REST API 参考](/rest/api/policy-insights)