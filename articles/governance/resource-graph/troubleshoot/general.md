---
title: 排查常见错误
description: 了解如何排查使用 Azure Resource Graph 查询 Azure 资源时出现的问题
author: DCtheGeek
ms.author: v-yiso
origin.date: 07/24/2019
ms.date: 09/16/2019
ms.topic: troubleshooting
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 7bbb94cc09ec957183a79798d2005c6be577da1d
ms.sourcegitcommit: dd0ff08835dd3f8db3cc55301815ad69ff472b13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70737425"
---
# <a name="troubleshoot-errors-using-azure-resource-graph"></a>排查使用 Azure Resource Graph 时出现的错误

在使用 Azure Resource Graph 查询 Azure 资源时，你可能会遇到错误。 本文描述可能会发生的各种错误及其解决方法。

## <a name="finding-error-details"></a>查找错误详细信息

大多数错误的原因是使用 Azure Resource Graph 运行查询时出现了问题。 如果某个查询失败，SDK 将提供有关失败的查询的详细信息。 此信息会指出存在的问题，以便可以修复问题并使后续查询成功。

## <a name="general-errors"></a>常规错误

### <a name="toomanysubscription"></a>场景：订阅过多

#### <a name="issue"></a>问题

有权访问 1000 个以上的订阅（包括具有 Azure Lighthouse 的跨租户订阅）的客户无法通过单次调用 Azure Resource Graph 来提取所有订阅中的数据。

#### <a name="cause"></a>原因

Azure CLI 和 PowerShell 只会将前 1000 个订阅转发到 Azure Resource Graph。 Azure Resource Graph 的 REST API 接受对最大数目的订阅执行查询。

#### <a name="resolution"></a>解决方法

对一部分订阅批处理查询请求，以保持在 1000 个订阅的限制范围内。 解决方法是在 PowerShell 中使用 **Subscription** 参数。

```azurepowershell
# Replace this query with your own
$query = 'project type'

# Fetch the full array of subscription IDs
$subscriptions = Get-AzSubscription
$subscriptionIds = $subscriptions.Id

# Create a counter, set the batch size, and prepare a variable for the results
$counter = [PSCustomObject] @{ Value = 0 }
$batchSize = 1000
$response = @()

# Group the subscriptions into batches
$subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

# Run the query for each batch
foreach ($batch in $subscriptionsBatch){ $response += Search-AzGraph -Query $query -Subscription $batch.Group }

# View the completed results of the query on all subscriptions
$response
```

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

- 通过 [Azure 论坛](https://azure.microsoft.com/support/forums/)获取 Azure 专家的解答。
- 如需更多帮助，可以提交 Azure 支持事件。 转到 [Azure 支持站点](https://support.azure.cn/zh-cn/support/contact)。