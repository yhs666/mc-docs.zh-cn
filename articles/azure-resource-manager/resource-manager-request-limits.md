---
title: 请求限制
description: 介绍在达到订阅限制后如何对 Azure Resource Manager 请求进行限制。
ms.topic: conceptual
origin.date: 10/26/2019
ms.date: 11/25/2019
ms.custom: seodec18
ms.openlocfilehash: ddc8413b70ac1b7d976f06bcada6167305f96005
ms.sourcegitcommit: cf73284534772acbe7a0b985a86a0202bfcc109e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884883"
---
# <a name="throttling-resource-manager-requests"></a>限制 Resource Manager 请求数

本文介绍 Azure 资源管理器如何限制请求。 其中介绍了如何在达到限制之前跟踪剩余的请求数，以及在达到限制时如何采取应对措施。

限制在两个级别发生。 Azure 资源管理器限制对订阅和租户的请求。 如果请求低于订阅和租户的限制，资源管理器会将该请求路由到资源提供程序。 资源提供程序应用针对该订阅和租户的操作专门指定的限制。

<!--Not Available on  The following image shows how throttling is applied as a request goes from the user to Azure Resource Manager and the resource provider.-->
<!--Not Available on ![Request throttling](./media/resource-manager-request-limits/request-throttling.svg)-->

## <a name="subscription-and-tenant-limits"></a>订阅和租户限制

每个订阅级别和租户级别的操作都会受到限制。 订阅请求是需要传递订阅 ID 的请求，例如在订阅中检索资源组。 租户请求不包括订阅 ID，例如，检索有效的 Azure 位置。

下表显示了每小时的默认限制。

| 作用域 | 操作 | 限制 |
| ----- | ---------- | ------- |
| 订阅 | 读取 | 12000 |
| 订阅 | 删除 | 15000 |
| 订阅 | 写入 | 1200 |
| 租户 | 读取 | 12000 |
| 租户 | 写入 | 1200 |

这些限制的范围是发出请求的安全主体（用户或应用程序）和订阅 ID 或租户 ID。 如果请求来自多个安全主体，则对订阅或租户的限制大于每小时 12,000 个和每小时 1,200 个。

这些限制适用于每个 Azure 资源管理器实例。 每个 Azure 区域中有多个实例，Azure 资源管理器将部署到所有 Azure 区域。  因此，实践中的限制要高于这些限制。 来自用户的请求通常由不同的 Azure 资源管理器实例进行处理。

## <a name="resource-provider-limits"></a>资源提供程序限制

资源提供程序应用自身的限制。 由于资源管理器按主体 ID 和资源管理器实例进行限制，因此资源提供程序收到的请求数可能会超过上一部分所述的默认限制。

本部分讨论某些广泛使用的资源提供程序的限制。

### <a name="storage-throttling"></a>存储限制

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="network-throttling"></a>网络限制

Microsoft.Network 资源提供程序应用以下限制：

| 操作 | 限制 |
| --------- | ----- |
| 写入/删除 (PUT) | 每 5 分钟 1000 次 |
| 读取 (GET) | 每 5 分钟 10000 次 |

### <a name="compute-throttling"></a>计算限制

有关计算操作的限制的信息，请参阅[排查 API 限制错误 - 计算](../virtual-machines/troubleshooting/troubleshooting-throttling-errors.md)。

若要检查虚拟机规模集中的虚拟机实例，请使用[虚拟机规模集操作](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms)。 例如，使用[虚拟机规模集 VM - 列出](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list)和相应的参数来检查虚拟机实例的电源状态。 此 API 可减少请求数。

### <a name="azure-resource-graph-throttling"></a>Azure Resource Graph 限制

Azure Resource Graph 会限制对其操作发出的请求数。 本文所述的用于确定剩余请求数以及在达到限制时如何采取应对措施的步骤同样适用于 Resource Graph。 但是，Resource Graph 设置了自身的限制和重置速率。 有关详细信息，请参阅 [Azure Resource Graph 中的限制](../governance/resource-graph/overview.md#throttling)。

## <a name="request-increase"></a>请求增加

有时可以提高限制。 若要确定是否可以提高你的方案限制，请创建支持请求。 我们将详细评估你的调用模式。

## <a name="error-code"></a>错误代码

达到限制时，会收到 HTTP 状态代码“429 请求过多”  。 响应包含 **Retry-After** 值，该值指定在发送下一个请求之前应用程序应该等待（或休眠）的秒数。 如果在尚未达到重试时间值的情况下发送请求，该请求不会得到处理，并会返回一个新的重试时间值。

等待指定的时间后，还可以关闭再重新打开与 Azure 的连接。 通过重置连接，可以连接到 Azure 资源管理器的其他实例。

如果使用的是 Azure SDK，该 SDK 可能会采用自动重试配置。 有关详细信息，请参阅 [Azure 服务的重试指南](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)。

某些资源提供程序返回 429 来报告暂时性问题。 该问题可能是一种过载状态，它不是由你的请求直接造成的。 或者，它可能表示目标资源或依赖资源的状态相关的暂时性错误。 例如，当目标资源被另一个操作锁定时，网络资源提供程序将返回 429 和 **RetryableErrorDueToAnotherOperation** 错误代码。 若要确定错误是由限制还是暂时性的状况造成，请查看响应中的错误详细信息。

## <a name="remaining-requests"></a>剩余请求数

可以通过检查响应标头确定剩余请求数。 读取请求在标头中返回一个值，表示剩余读取请求的数目。 写入请求包含的值表示剩余写入请求的数目。 下表说明了各种响应标头，用户可以检查其中是否存在这些值：

| 响应标头 | 说明 |
| --- | --- |
| x-ms-ratelimit-remaining-subscription-reads |划归到订阅的剩余读取数。 执行读取操作时返回此值。 |
| x-ms-ratelimit-remaining-subscription-writes |划归到订阅的剩余写入数。 执行写入操作时返回此值。 |
| x-ms-ratelimit-remaining-tenant-reads |租户范围的剩余读取数 |
| x-ms-ratelimit-remaining-tenant-writes |租户范围的剩余写入数 |
| x-ms-ratelimit-remaining-subscription-resource-requests |划归到订阅的剩余资源类型请求数。<br /><br />仅当服务重写了默认限制时，才返回此标头值。 Resource Manager 将累加此值而不是订阅读取/写入数。 |
| x-ms-ratelimit-remaining-subscription-resource-entities-read |划归到订阅的剩余资源类型集合请求数。<br /><br />仅当服务重写了默认限制时，才返回此标头值。 此值提供剩余集合请求数（列出资源）。 |
| x-ms-ratelimit-remaining-tenant-resource-requests |划归到租户的剩余资源类型请求数。<br /><br />仅当服务重写了默认限制时，才为租户级别的请求添加此标头。 Resource Manager 将累加此值而不是租户读取/写入数。 |
| x-ms-ratelimit-remaining-tenant-resource-entities-read |划归到租户的剩余资源类型集合请求数。<br /><br />仅当服务重写了默认限制时，才为租户级别的请求添加此标头。 |

资源提供程序还可以返回包含剩余请求数相关信息的响应标头。 有关“计算”资源提供程序返回的响应标头的信息，请参阅[调用速率参考性响应标头](../virtual-machines/troubleshooting/troubleshooting-throttling-errors.md#call-rate-informational-response-headers)。

## <a name="retrieving-the-header-values"></a>检索标头值

检索代码或脚本中的这些标头值与检索任何标头值没有什么不同。 

例如，在 **C#** 中，可使用以下代码从名为 **response** 的 **HttpWebResponse** 对象中检索标头值：

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

在 **PowerShell**中，可以通过 Invoke-WebRequest 操作检索标头值。

```powershell
$r = Invoke-WebRequest -Uri https://management.chinacloudapi.cn/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

有关完整的 PowerShell 示例，请参阅[查看订阅的资源管理器限制](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI)。

若要查看剩余请求数以进行调试，可在 **PowerShell** cmdlet 中提供 **-Debug** 参数。

```powershell
Get-AzResourceGroup -Debug
```

这会返回许多值，包括以下响应值：

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 11999
```

若要获取写入限制，请使用写入操作： 

```powershell
New-AzResourceGroup -Name myresourcegroup -Location chinanorth -Debug
```

这会返回许多值，包括以下值：

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

在 **Azure CLI**中，可以使用更详细的选项检索标头值。

```azurecli
az group list --verbose --debug
```

这会返回许多值，包括以下值：

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '11998'
```

若要获取写入限制，请使用写入操作： 

```azurecli
az group create -n myresourcegroup --location chinanorth --verbose --debug
```

这会返回许多值，包括以下值：

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="next-steps"></a>后续步骤

* 有关完整的 PowerShell 示例，请参阅[查看订阅的资源管理器限制](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI)。
* 有关限制和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额和约束](../azure-subscription-service-limits.md)。
* 若要了解如何处理异步 REST 请求，请参阅[跟踪异步 Azure 操作](resource-manager-async-operations.md)。

<!-- Update_Description: update meta properties, wording update, update link -->