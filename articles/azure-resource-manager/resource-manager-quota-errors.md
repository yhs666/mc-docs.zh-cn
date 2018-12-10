---
title: Azure 配额错误 | Azure
description: 说明如何解决资源配额错误。
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
origin.date: 03/09/2018
ms.date: 03/26/2018
ms.author: v-yeche
ms.openlocfilehash: 64877c52eab7d5fbceea817ac3a8a16ed37d05dc
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52649809"
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

按每个资源组、订阅、帐户和其他作用域应用配额。 例如，订阅可能配置为限制某个区域的核心数目。 如果尝试部署超过允许核心数目的虚拟机，则会收到指出超过配额的错误消息。
有关完整的配额信息，请参阅 [Azure 订阅和服务限制、配额与约束](../azure-subscription-service-limits.md)。

## <a name="troubleshooting"></a>故障排除

### <a name="azure-cli"></a>Azure CLI

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

### <a name="powershell"></a>PowerShell

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

## <a name="solution"></a>解决方案

若要请求增加配额，请转到门户并提出支持问题。 在支持问题中，为你想要在其中进行部署的区域请求增加配额。

> [!NOTE]
> 请记住，对于资源组，配额针对每个单独的区域，而不是针对整个订阅。 如果需要在中国北部部署 30 个核心，则必须在中国北部寻求 30 个 Resource Manager 核心。 如果需要在有权访问的任何区域内部署总共 30 个核心，则应在所有区域内请求总共 30 个 Resource Manager 核心。
>
>

1. 选择 **订阅**。

   ![订阅](./media/resource-manager-quota-errors/subscriptions.png)

2. 选择需要增加配额的订阅。

   ![选择订阅](./media/resource-manager-quota-errors/select-subscription.png)

3. 选择“使用情况 + 配额”

   ![选择使用情况和配额](./media/resource-manager-quota-errors/select-usage-quotas.png)

4. 在右上角选择“请求增加”。

   ![请求增加](./media/resource-manager-quota-errors/request-increase.png)

5. 填写你需要增加的配额类型的表单。
<!-- Not Available on  ![Fill in form](./media/resource-manager-quota-errors/forms.png) -->

<!-- Update_Description: update meta properties, wording update -->