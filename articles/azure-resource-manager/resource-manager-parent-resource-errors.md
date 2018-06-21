---
title: Azure 父资源错误 | Azure
description: 说明如何在使用父资源时解决错误。
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
origin.date: 10/23/2017
ms.date: 10/23/2017
ms.author: v-yeche
ms.openlocfilehash: 480e25dac2a6f49aca6d8b3a181a509472e28362
ms.sourcegitcommit: 6ef36b2aa8da8a7f249b31fb15a0fb4cc49b2a1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2017
ms.locfileid: "23475217"
---
# <a name="resolve-errors-for-parent-resources"></a>解决父资源的错误

本文介绍部署依赖于父资源的资源时可能遇到的错误。

## <a name="symptom"></a>症状

在部署的资源是另一个资源的子级时，可能会收到以下错误：

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>原因

如果一个资源是另一个资源的子级，则在创建子资源之前，父资源必须存在。 子资源的名称包括父名称。 例如，SQL 数据库可能定义为：

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

但是，如果未在服务器上指定依赖关系，数据库部署可能会在部署服务器之前开始。

## <a name="solution"></a>解决方案

为更正此错误，请添加依赖关系。

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

有关详细信息，请参阅[定义 Azure 资源管理器模板中部署资源的顺序](resource-group-define-dependencies.md)。


<!--Update_Description: new articles on parent resource errors-->