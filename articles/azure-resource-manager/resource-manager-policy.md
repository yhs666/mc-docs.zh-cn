---
title: "Azure 资源策略 | Azure"
description: "介绍如何使用 Azure Resource Manager 来确保在部署过程中设置一致的资源属性。 可在订阅或资源组中应用策略。"
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/15/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: 57053ccd7b83f22d32a2b9267323c6e1eb55bac0
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 资源策略概述
<a id="resource-policy-overview" class="xliff"></a>
使用资源策略可在组织中建立资源约定。 通过定义约定，可以控制成本并更轻松地管理资源。 例如，可以指定仅允许某些类型的虚拟机，或要求所有资源都带有特定的标记。 策略由所有子资源继承。 因此，如果将策略应用到资源组，则其适用于该资源组中的所有资源。

了解策略的两个概念：

* 策略定义 - 描述何时强制执行策略，以及要采取的操作
* 策略分配 - 应用策略定义的范围（订阅或资源组）

<!-- Not Available on resource-manager-policy-create-assign.md-->

Azure 提供一些内置的策略定义，可减少需要定义的策略数目。 如果内置策略定义适用于你的方案，请在分配到范围时使用该定义。

将在创建和更新资源（PUT 和 PATCH 操作）时评估策略。

> [!NOTE]
> 当前，策略不对不支持标记、种类和位置的资源类型进行评估，例如 Microsoft.Resources/deployments 资源类型。 将来会添加此支持。 若要避免向后兼容问题，创作策略时应显式指定类型。 例如，未指定类型的标记策略应用于所有类型。 在此情况下，如果有嵌套资源不支持标记，并且部署资源类型已添加到策略评估中，则模板部署可能会失败。 
> 
> 

## 策略与 RBAC 有什么不同？
<a id="how-is-it-different-from-rbac" class="xliff"></a>
策略和基于角色的访问控制 (RBAC) 之间存在一些主要区别。 RBAC 关注不同范围内的**用户**操作。 例如，将你添加到所需范围的资源组的参与者角色后，你便可以对该资源组做出更改。 策略关注部署期间的 **资源** 属性。 例如，通过策略，你可以控制可预配的资源类型，或限制可以预配资源的位置。 不同于 RBAC，策略是默认的允许和明确拒绝系统。 

若要使用策略，用户必须通过 RBAC 进行身份验证。 具体而言，帐户需要：

* `Microsoft.Authorization/policydefinitions/write` 权限
* `Microsoft.Authorization/policyassignments/write` 权限 

**参与者** 角色不包括这些权限。

## 策略定义结构
<a id="policy-definition-structure" class="xliff"></a>
使用 JSON 创建策略定义。 策略定义包含以下各项的元素：

* 参数
* 显示名称
* description
* 策略规则
  * 逻辑求值
  * 效果

以下示例演示了一个限制资源部署位置的策略：

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## 参数
<a id="parameters" class="xliff"></a>
使用参数可减少策略定义的数量，有助于简化策略管理。 为资源属性定义策略（如限制资源部署的位置），并在定义中包含参数。 然后，通过在分配策略时传递不同的值（例如为订阅指定一组位置），针对不同的方案重复使用该策略定义。

在创建策略定义时声明参数。

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

参数类型可以是字符串，也可以是数组。 Azure 门户等工具使用元数据属性显示用户友好信息。 

在策略规则中，使用下列语法引用参数： 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## 显示名称和说明
<a id="display-name-and-description" class="xliff"></a>

使用**显示名称**和**说明**来标识策略定义，并提供其使用情景。

## 策略规则
<a id="policy-rule" class="xliff"></a>

策略规则包括 **If** and **Then** 块。 在 **If** 块中，定义强制执行策略时指定的一个或多个条件。 可以对这些条件应用逻辑运算符，以精确定义策略的方案。 在 **Then** 块中，定义满足 **If** 条件时产生的效果。

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### 逻辑运算符
<a id="logical-operators" class="xliff"></a>
支持的逻辑运算符为：

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

**not** 语法反转条件的结果。 **allOf** 语法（与逻辑 **And** 操作相似）要求所有条件为 true。 **anyOf** 语法（与逻辑 **Or** 操作相似）要求一个或多个条件为 true。

可以嵌套逻辑运算符。 以下示例显示了嵌套在 allOf 操作中的 not 操作。 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>条件
条件评估 **字段** 是否符合特定的准则。 支持的条件有：

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

在使用 **like** 条件时，可以在值中提供通配符 (*)。

使用 **match** 条件时，请提供 `#` 来表示数字，提供 `?` 来表示字母，提供任何其他字符来表示该实际字符。 有关示例，请参阅[设置命名约定](#set-naming-convention)。

### 字段
<a id="fields" class="xliff"></a>
使用字段构成条件。 字段显示用于描述资源状态的资源请求负载属性。  

支持以下字段：

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* 属性别名 - 若要查看列表，请参阅[别名](#aliases)。

### 效果
<a id="effect" class="xliff"></a>
策略支持三种类型的效果 - `deny`、`audit` 和 `append`。 

* **Deny** 将在审核日志中生成一个事件，并使请求失败
* **Audit** 将在审核日志中生成一个警告事件，但不会使请求失败
* **Append** 会将定义的字段集添加到请求 

对于 **append**，必须提供以下详细信息：

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

值可以是字符串或 JSON 格式对象。 

## 别名
<a id="aliases" class="xliff"></a>

使用属性别名来访问资源类型的特定属性。 

Microsoft.Cache/Redis

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | 设置是否启用非 ssl Redis 服务器端口 (6379)。 |
| Microsoft.Cache/Redis/shardCount | 设置要在高级群集缓存上创建的分片数。  |
| Microsoft.Cache/Redis/sku.capacity | 设置要部署的 Redis 缓存大小。  |
| Microsoft.Cache/Redis/sku.family | 设置要使用的 SKU 系列。 |
| Microsoft.Cache/Redis/sku.name | 设置要部署的 Redis 缓存类型。 |

Microsoft.Cdn/profiles

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | 设置定价层的名称。 |

Microsoft.Compute/disks

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | 设置用于创建虚拟机的平台映像或应用商店映像的产品/服务。 |
| Microsoft.Compute/imagePublisher | 设置用于创建虚拟机的平台映像或应用商店映像的发布服务器。 |
| Microsoft.Compute/imageSku | 设置用于创建虚拟机的平台映像或应用商店映像的 SKU。 |
| Microsoft.Compute/imageVersion | 设置用于创建虚拟机的平台映像或应用商店映像的版本。 |

Microsoft.Compute/virtualMachines

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | 设置用于创建虚拟机的平台映像或应用商店映像的产品/服务。 |
| Microsoft.Compute/imagePublisher | 设置用于创建虚拟机的平台映像或应用商店映像的发布服务器。 |
| Microsoft.Compute/imageSku | 设置用于创建虚拟机的平台映像或应用商店映像的 SKU。 |
| Microsoft.Compute/imageVersion | 设置用于创建虚拟机的平台映像或应用商店映像的版本。 |
| Microsoft.Compute/licenseType | 设置映像或磁盘是否在本地获得许可。 此值仅用于包含 Windows Server 操作系统的映像。  |
| Microsoft.Compute/virtualMachines/imageOffer | 设置用于创建虚拟机的平台映像或应用商店映像的产品/服务。 |
| Microsoft.Compute/virtualMachines/imagePublisher | 设置用于创建虚拟机的平台映像或应用商店映像的发布服务器。 |
| Microsoft.Compute/virtualMachines/imageSku | 设置用于创建虚拟机的平台映像或应用商店映像的 SKU。 |
| Microsoft.Compute/virtualMachines/imageVersion | 设置用于创建虚拟机的平台映像或应用商店映像的版本。 |
| Microsoft.Compute/virtualMachines/osDisk.Uri | 设置 vhd URI。 |
| Microsoft.Compute/virtualMachines/sku.name | 设置虚拟机的大小。 |

Microsoft.Compute/virtualMachines/extensions

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | 设置扩展的发布服务器的名称。 |
| Microsoft.Compute/virtualMachines/extensions/type | 设置扩展的类型。 |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | 设置扩展的版本。 |

Microsoft.Compute/virtualMachineScaleSets

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | 设置用于创建虚拟机的平台映像或应用商店映像的产品/服务。 |
| Microsoft.Compute/imagePublisher | 设置用于创建虚拟机的平台映像或应用商店映像的发布服务器。 |
| Microsoft.Compute/imageSku | 设置用于创建虚拟机的平台映像或应用商店映像的 SKU。 |
| Microsoft.Compute/imageVersion | 设置用于创建虚拟机的平台映像或应用商店映像的版本。 |
| Microsoft.Compute/licenseType | 设置映像或磁盘是否在本地获得许可。 此值仅用于包含 Windows Server 操作系统的映像。 |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | 设置规模集中所有虚拟机的计算机名前缀。 |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | 设置用户映像的 blob URI。 |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | 设置用于存储规模集操作系统磁盘的容器 URL。 |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | 设置规模集中虚拟机的大小。 |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | 设置规模集中虚拟机的层级。 |

Microsoft.Network/applicationGateways

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | 设置网关的大小。 |

Microsoft.Network/virtualNetworkGateways

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | 设置此虚拟网络网关的类型。 |
| Microsoft.Network/virtualNetworkGateways/sku.name | 设置网关 SKU 名称。 |

Microsoft.Sql/servers

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Sql/servers/version | 选择服务器的版本。 |

Microsoft.Sql/databases

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | 设置数据库的版本。 |
| Microsoft.Sql/servers/databases/elasticPoolName | 设置数据库所在弹性池的名称。 |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | 设置数据库的配置服务级别目标 ID。 |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | 设置数据库的配置服务级别目标的名称。  |

Microsoft.Sql/elasticpools

| 别名 | 说明 |
| ----- | ----------- |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | 设置数据库弹性池的总共享 DTU。 |
| servers/elasticpools | Microsoft.Sql/servers/elasticPools/edition | 设置弹性池的版本。 |

Microsoft.Storage/storageAccounts

| 别名 | 说明 |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | 设置用于计费的访问层。 |
| Microsoft.Storage/storageAccounts/accountType | 设置 SKU 名称。 |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | 设置数据存储在 blob 存储服务中时，服务是否对数据进行加密。 |
| Microsoft.Storage/storageAccounts/enableFileEncryption | 设置数据存储在文件存储服务中时，服务是否对数据进行加密。 |
| Microsoft.Storage/storageAccounts/sku.name | 设置 SKU 名称。 |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | 设置为仅允许 https 流量访问存储服务。 |

## 策略示例
<a id="policy-examples" class="xliff"></a>

以下主题包含策略示例：

<!-- Not Available on  resource-manager-policy-tags.md -->
<!-- Not Available on  resource-manager-policy-storage -->
* 有关虚拟机策略的示例，请参阅[将资源策略应用于 Linux VM](../virtual-machines/linux/policy.md?toc=%2fazure-resource-manager%2ftoc.json) 和[将资源策略应用于 Windows WM](../virtual-machines/windows/policy.md?toc=%2fazure-resource-manager%2ftoc.json)

### 允许的资源位置
<a id="allowed-resource-locations" class="xliff"></a>
若要指定允许的位置，请参阅 [策略定义结构](#policy-definition-structure) 部分中的示例。 若要分配此策略定义，请使用带有资源 ID `/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c`的内置策略。

### 不允许的资源位置
<a id="not-allowed-resource-locations" class="xliff"></a>
若要指定不允许的位置，请使用以下策略定义：

```json
{
  "properties": {
    "parameters": {
      "notAllowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that are not allowed when deploying resources",
          "strongType": "location",
          "displayName": "Not allowed locations"
        }
      }
    },
    "displayName": "Not allowed locations",
    "description": "This policy enables you to block locations that your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "field": "location",
        "in": "[parameters('notAllowedLocations')]"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### 允许的资源类型
<a id="allowed-resource-types" class="xliff"></a>
以下示例显示只允许对 Microsoft.Resources、Microsoft.Compute、Microsoft.Storage 和 Microsoft.Network 资源类型进行部署的策略。 将拒绝其他所有资源类型：

```json
{
  "if": {
    "not": {
      "anyOf": [
        {
          "field": "type",
          "like": "Microsoft.Resources/*"
        },
        {
          "field": "type",
          "like": "Microsoft.Compute/*"
        },
        {
          "field": "type",
          "like": "Microsoft.Storage/*"
        },
        {
          "field": "type",
          "like": "Microsoft.Network/*"
        }
      ]
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

### 设置命名约定
<a id="set-naming-convention" class="xliff"></a>
以下示例演示如何使用 **like** 条件支持的通配符。 该条件指明，如果名称符合所述模式 (namePrefix\*nameSuffix)，则拒绝请求：

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

若要指定资源名称与某个模式匹配，请使用 match 条件。 下面的示例要求名称以 `contoso` 开头并包含六个其他字母：

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

若要求日期模式为两个数字、短划线、三个字母、短划线和四个数字，请使用以下代码：

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## 后续步骤
<a id="next-steps" class="xliff"></a>
<!-- Not Available on resource-manager-policy-portal.md /  resource-manager-policy-create-assign.md-->
* 有关企业可如何使用 Resource Manager 有效管理订阅的指南，请参阅 [Azure 企业基架 - 出于合规目的监管订阅](resource-manager-subscription-governance.md)。
* 该策略架构已在 [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json)中发布。