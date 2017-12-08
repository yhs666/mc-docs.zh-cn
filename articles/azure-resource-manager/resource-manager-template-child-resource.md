---
title: "在 Azure 模板中定义子资源 | Azure"
description: "演示如何在 Azure Resource Manager 模板中设置子资源的资源类型和名称"
services: azure-resource-manager
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/01/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: 0a3c0a4f33d3839c569d61c7e4653df724d3e888
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>在 Resource Manager 模板中设置子资源的名称和类型
创建模板时，需频繁地包括与父资源相关的子资源。 例如，模板可能包括 SQL Server 和数据库。 SQL Server 是父资源，数据库是子资源。 

子资源类型的格式为： `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

子资源名称的格式为： `{parent-resource-name}/{child-resource-name}`

但是，在模板中指定资源类型和名称的方式并不相同，具体取决于该资源是嵌套在父资源中，还是独立存在于顶级目录中。 本主题演示如何使用这两种方法。

构造对资源的完全限定引用时，组合类型和名称段的顺序并不只是将这两者串联。  而是，在命名空间后面，使用*类型/名称*对的序列（从最不具体到最具体）：

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

例如：

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` 正确，`Microsoft.Compute/virtualMachines/extensions/myVM/myExt` 不正确

## <a name="nested-child-resource"></a>嵌套式子资源
若要定义子资源，最简单的方式是将其嵌套在父资源中。 以下示例演示了嵌套在 SQL Server 中的 SQL 数据库。

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

对于子资源，类型设置为 `databases`，但其完整资源类型是 `Microsoft.Sql/servers/databases`。 不提供 `Microsoft.Sql/servers/` 是因为它取自父资源类型。 子资源名称设置为 `exampledatabase` ，但完整名称包括父名称。 不提供 `exampleserver` 是因为它取自父资源。

## <a name="top-level-child-resource"></a>顶级子资源
可以定义顶级子资源。 使用此方法的前提是：父资源未在同一模板中部署，或者你需要使用 `copy` 创建多个子资源。 使用此方法时，必须提供完整的资源类型，并将父资源名称包括在子资源名称中。

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

数据库是服务器的子资源，即使二者是在模板的同一级别定义的。

## <a name="next-steps"></a>后续步骤
* 有关如何创建模板的建议，请参阅 [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md)（创建 Azure Resource Manager 模板的最佳做法）。
* 如需创建多个子资源的示例，请参阅[在 Azure Resource Manager 模板中部署资源的多个实例](resource-group-create-multiple.md)。