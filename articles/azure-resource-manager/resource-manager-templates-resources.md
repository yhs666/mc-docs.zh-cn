---
title: Azure 资源管理器模板资源 | Azure
description: 介绍了使用声明性 JSON 语法的 Azure 资源管理器模板的 resources 节。
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 12/13/2017
ms.date: 04/30/2018
ms.author: v-yeche
ms.openlocfilehash: 0cdeccb1b34fb15aeaf4fc4e6b60c83b0ad109e6
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
ms.locfileid: "32121612"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Azure 资源管理器模板的 Resources 节

在 resources 节，可以定义部署或更新的资源。 此节可能比较复杂，因为用户必须了解要部署哪些类型才能提供正确的值。

## <a name="available-properties"></a>可用属性

使用以下结构定义资源：

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| 元素名称 | 必须 | 说明 |
|:--- |:--- |:--- |
| 条件 | 否 | 指示是否部署资源的布尔值。 |
| apiVersion |是 |用于创建资源的 REST API 版本。 |
| type |是 |资源的类型。 此值是资源提供程序的命名空间和资源类型（例如 **Microsoft.Storage/storageAccounts**）的组合。 |
| name |是 |资源的名称。 该名称必须遵循 RFC3986 中定义的 URI 构成部分限制。 此外，向第三方公开资源名称的 Azure 服务将验证名称，以确保它不是尝试窃取另一个身份。 |
| location |多种多样 |提供的资源支持的地理位置。 可以选择任何可用位置，但通常选取靠近用户的位置。 通常，在同一区域放置彼此交互的资源也很有用。 大多数资源类型需要一个位置，但某些类型 （如角色分配）不需要位置。 |
| 标记 |否 |与资源关联的标记。 应用可以在订阅中对资源进行逻辑组织的标记。 |
| 注释 |否 |用于描述模板中资源的注释 |
| 复制 |否 |如果需要多个实例，则为要创建的资源数。 默认模式为并行。 如果你不希望同时部署所有资源，请指定串行模式。 有关详细信息，请参阅[在 Azure Resource Manager 中创建多个资源实例](resource-group-create-multiple.md)。 |
| dependsOn |否 |部署此资源之前必须部署的资源。 Resource Manager 评估资源之间的依赖关系，并根据正确顺序进行部署。 如果资源不相互依赖，则可并行部署资源。 该值可以是资源名称或资源唯一标识符的逗号分隔列表。 在此模板中仅部署列出的资源。 此模板中未定义的资源必须已存在。 避免添加不必要的依赖项，因为这些依赖项可能会降低部署速度并创建循环依赖项。 有关设置依赖项的指导，请参阅[在 Azure Resource Manager 模板中定义依赖项](resource-group-define-dependencies.md)。 |
| properties |否 |特定于资源的配置设置。 properties 的值与创建资源时，在 REST API 操作（PUT 方法）的请求正文中提供的值相同。 还可以指定 copy 数组来创建属性的多个实例。 |
| sku | 否 | 某些资源接受定义了要部署的 SKU 的值。 例如，可以为存储帐户指定冗余类型。 |
| kind | 否 | 某些资源接受定义了你部署的资源类型的值。 例如，可以指定要创建的 Cosmos DB 的类型。 |
| 计划 | 否 | 某些资源接受定义了要部署的计划的值。 例如，可以为虚拟机指定 Marketplace 映像。 | 
| 资源 |否 |依赖于所定义的资源的子资源。 只能提供父资源的架构允许的资源类型。 子资源的完全限定类型包含父资源类型，例如 **Microsoft.Web/sites/extensions**。 不隐式表示对父资源的依赖。 必须显式定义该依赖关系。 |

## <a name="resource-specific-values"></a>特定于资源的值

每种资源类型的 **apiVersion**、**type** 和 **properties** 均不同。 **sku**、**kind** 和 **plan** 元素可用于某些资源类型，但非全部。 若要确定这些属性的值，请参阅[模板参考](https://docs.microsoft.com/zh-cn/azure/templates/)。

## <a name="resource-names"></a>资源名称
通常，会在 Resource Manager 中使用三种类型的资源名称：

* 必须唯一的资源名称。
* 不一定要唯一的资源名称，不过，选择提供的名称应有助于识别资源。
* 通用的资源名称。

### <a name="unique-resource-names"></a>唯一的资源名称
对于具有数据访问终结点的任何资源类型，必须提供唯一的资源名称。 需要唯一名称的一些常见资源类型包括：

* Azure 存储<sup>1</sup> 
* Azure 应用服务的 Web 应用功能
* SQL Server
* Azure Key Vault
* Azure Redis 缓存
* Azure Batch
* Azure 流量管理器
* Azure 搜索
* Azure HDInsight

<sup>1</sup> 存储帐户名必须使用小写字母，包含 24 个或更少的字符，并且不包含任何连字符。

设置名称时，可以手动创建一个唯一的名称，也可以使用 [uniqueString()](resource-group-template-functions-string.md#uniquestring) 函数生成一个名称。 可能还需要在 **uniqueString** 结果中添加一个前缀或后缀。 修改唯一的名称可以更方便地通过名称识别资源类型。 例如，可以使用以下变量生成存储帐户的唯一名称：

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>用于标识的资源名称
某些资源类型可能需要命名，但它们的名称不一定非要唯一。 对于这些资源类型，可以提供一个用于标识资源上下文和资源类型的名称。

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>通用资源名称
对于主要通过其他资源访问的资源类型，可以在模板中使用硬编码的通用名称。 例如，可为 SQL Server 上的防火墙规则设置一个标准的通用名称：

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>位置
部署模板时，必须提供每个资源的位置。 不同位置支持的资源类型不一样。 若要查看适用于特定资源类型的订阅的位置列表，请使用 Azure PowerShell 或 Azure CLI。 

以下示例使用 PowerShell 获取 `Microsoft.Web\sites` 资源类型的位置：

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

以下示例使用 Azure CLI 2.0获取 `Microsoft.Web\sites` 资源类型的位置：

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

确定资源支持的位置后，请在模板中设置该位置。 设置此值的最简单方法是在支持资源类型的位置中创建资源组，并将每个位置设置为 `[resourceGroup().location]`。 可以将模板重新部署到不同位置中的资源组，而不更改模板或参数中的任何值。 

以下示例显示了部署到资源组所在位置的存储帐户：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

如果需要在模板中对位置进行硬编码，请提供其中一个支持的区域名称。 以下示例显示了始终部署到中国北部的存储帐户：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "China North",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="tags"></a>标记
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>将标记添加到模板

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>子资源

在某些资源类型内，还可以定义子资源数组。 子资源是只存在于另一资源的上下文内的资源。 例如，SQL 数据库不能在没有 SQL Server 的情况下存在，因此此数据库是此 Server 的子资源。 可以在此 Server 的定义内定义此数据库。

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

在嵌套的情况下，类型设置为 `databases`，但完整资源类型为 `Microsoft.Sql/servers/databases`。 不提供 `Microsoft.Sql/servers/` 是因为它取自父资源类型。 子资源名称设置为 `exampledatabase` ，但完整名称包括父名称。 不提供 `exampleserver` 是因为它取自父资源。

子资源类型的格式为： `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

子资源名称的格式为： `{parent-resource-name}/{child-resource-name}`

但是，无需在此 Server 内定义此数据库。 可以定义顶级子资源。 使用此方法的前提是：父资源未在同一模板中部署，或者需要使用 `copy` 创建多个子资源。 使用此方法时，必须提供完整的资源类型，并将父资源名称包括在子资源名称中。

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

构造对资源的完全限定引用时，组合类型和名称段的顺序并不只是将这两者串联。  而是，在命名空间后面，使用*类型/名称*对的序列（从最不具体到最具体）：

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

例如：

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` 正确，`Microsoft.Compute/virtualMachines/extensions/myVM/myExt` 不正确

## <a name="recommendations"></a>建议
使用资源时，以下信息可以提供帮助：

* 为了帮助其他参与者理解该资源的用途，请为模板中的每个资源指定**注释**：

   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* 如果在模板中使用*公共终结点*（例如 Azure Blob 存储公共终结点），请*不要*将命名空间硬编码。 使用 **reference** 函数可动态检索命名空间。 可以使用此方法将模板部署到不同的公共命名空间环境，而无需在模板中手动更改终结点。 在模板中将 API 版本设置为用于存储帐户的同一版本：

   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

   如果在创建的同一模板中部署存储帐户，则引用资源时不需要指定提供程序命名空间。 以下示例显示简化的语法：

   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

   如果在模板中包含配置为使用公共命名空间的其他值，请更改这些值以反映相同的 **reference** 函数。 例如，可以设置虚拟机诊断配置文件的 **storageUri** 属性：

   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```

   还可以引用不同资源组中的现有存储帐户：

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* 仅当应用程序有需要时，才将公共 IP 地址分配到虚拟机。 若要连接到虚拟机 (VM) 进行调试或管理，请使用出站 NAT 规则、虚拟网络网关或 jumpbox。

     有关连接到虚拟机的详细信息，请参阅：

<!-- Not Available on [Run VMs for an N-tier architecture in Azure](../guidance/guidance-compute-n-tier-vm.md) -->
   * [在 Azure Resource Manager 中设置对 VM 的 WinRM 访问](../virtual-machines/windows/winrm.md)
   * [使用 Azure 门户实现对 VM 的外部访问](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [使用 PowerShell 实现对 VM 的外部访问](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [使用 Azure CLI 实现对 Linux VM 的外部访问](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* 公共 IP 地址的 **domainNameLabel** 属性必须唯一。 **domainNameLabel** 值的长度必须为 3 到 63 个字符，并遵循正则表达式 `^[a-z][a-z0-9-]{1,61}[a-z0-9]$` 指定的规则。 由于 **uniqueString** 函数生成长度为 13 个字符的字符串，因此 **dnsPrefixString** 参数限制为不超过 50 个字符：

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* 将密码添加到自定义脚本扩展时，请在 **protectedSettings** 属性中使用 **commandToExecute** 属性：

   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```

   > [!NOTE]
   > 为了确保机密内容作为参数传递给 VM 和扩展时经过加密，请使用相关扩展的 **protectedSettings** 属性。
   > 
   > 

## <a name="next-steps"></a>后续步骤
* 若要查看许多不同类型的解决方案的完整模型，请参阅 [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates/)（Azure 快速入门模板）。
* 有关用户可以使用的来自模板中的函数的详细信息，请参阅 [Azure Resource Manager Template Functions](resource-group-template-functions.md)（Azure Resource Manager 模板函数）。
* 要在部署期间合并多个模板，请参阅[将已链接的模板与 Azure 资源管理器配合使用](resource-group-linked-templates.md)。
* 可能需要使用不同资源组中的资源。 使用跨多个资源组共享的存储帐户或虚拟网络时，此方案很常见。 有关详细信息，请参阅 [resourceId 函数](resource-group-template-functions-resource.md#resourceid)。
<!--Not Available on [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md) -->

<!-- Update_Description: wording update, update link -->