---
title: "Azure 配额错误 | Azure"
description: "说明如何解决资源配额错误。"
services: azure-resource-manager,azure-portal
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
origin.date: 09/13/2017
ms.date: 10/23/2017
ms.author: v-yeche
ms.openlocfilehash: f3c7baae26b786c66c03b8c93405ca847fa693ef
ms.sourcegitcommit: 9284e560b58d9cbaebe6c2232545f872c01b78d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>解决资源配额错误

本文介绍部署资源时可能遇到的配额错误。

## <a name="symptom"></a>症状

如果部署一个创建超过 Azure 配额的资源的模板，则会收到如下所示的部署错误：

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

或者，可能会看到：

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>原因

按每个资源组、订阅、帐户和其他作用域应用配额。 例如，订阅可能配置为限制某个区域的核心数目。 如果尝试部署超过允许核心数目的虚拟机，将收到指出超过配额的错误消息。

## <a name="solution"></a>解决方案

### <a name="solution-1"></a>解决方案 1

对于 Azure CLI，使用 `az vm list-usage` 命令查找虚拟机配额。

```azurecli
az vm list-usage --location "China East"
```

返回：

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>解决方案 2

对于 PowerShell，使用 **Get-AzureRmVMUsage** 命令查找虚拟机配额。

```powershell
Get-AzureRmVMUsage -Location "China East"
```

返回：

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>解决方案 3

如果需要增加配额限制，请前往门户并提交一份支持问题以增加要部署区域内的配额。

> [!NOTE]
> 请记住，对于资源组，配额针对每个单独的区域，而不是针对整个订阅。 如果需要在中国北部部署 30 个核心，则必须在中国北部寻求 30 个 Resource Manager 核心。 如果需要在有权访问的任何区域内部署总共 30 个核心，则应在所有区域内请求总共 30 个 Resource Manager 核心。
>
>


<!--Update_Description: new articles on resource manager quota errors-->