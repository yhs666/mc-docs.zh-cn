---
title: "Azure 服务总线常见问题 (FAQ) | Azure"
description: "回答了一些关于 Azure 服务总线的常见问题。"
services: service-bus
documentationCenter: na
authors: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 05/02/2017
ms.author: v-yiso
ms.date: 07/17/2017
ms.openlocfilehash: 4880331ae859061a030e43423569b4a953c051fd
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# 服务总线常见问题解答
<a id="service-bus-faq" class="xliff"></a>
本文回答了一些关于 Microsoft Azure 服务总线的常见问题。 你还可以访问 [Azure 支持 FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) 了解一般的 Azure 定价和支持信息。

## 关于 Azure 服务总线的一般问题
<a id="general-questions-about-azure-service-bus" class="xliff"></a>
### 什么是 Azure 服务总线？
<a id="what-is-azure-service-bus" class="xliff"></a>
[Azure 服务总线](service-bus-messaging-overview.md) 是一个异步消息传送云平台，可让你在分离的系统之间发送数据。 Microsoft 以服务的形式提供此功能，这表示你不需要托管任何自有硬件就能使用它。

### 什么是服务总线命名空间？
<a id="what-is-a-service-bus-namespace" class="xliff"></a>

[命名空间](./service-bus-create-namespace-portal.md)提供了用于对应用程序中的服务总线资源进行寻址的范围容器。 必须创建命名空间才能使用服务总线，而且这也是入门的第一步。

### 什么是 Azure 服务总线队列？
<a id="what-is-an-azure-service-bus-queue" class="xliff"></a>

[服务总线队列](./service-bus-queues-topics-subscriptions.md)是用于存储消息的实体。 当你有多个应用程序，或者多个需要彼此通信的分布式应用程序部分时，队列特别有用。 队列和发行中心的相似之处在于，两者都会接收多个产品（消息），再从该处送出。

### 什么是 Azure 服务总线主题和订阅？
<a id="what-are-azure-service-bus-topics-and-subscriptions" class="xliff"></a>
主题可被视为队列，使用多个订阅时，它将成为更丰富的消息传送模型；实质上是一种一对多的通信工具。 此发布/订阅模型（或 pub/sub）启用了一个应用程序，该应用程序将消息发送到具有多个订阅的主题中，进而使多个应用程序接收到该消息。

### 什么是分区实体？
<a id="what-is-a-partitioned-entity" class="xliff"></a>

传统的队列或主题由单个消息中转站进行处理并存储在一个消息存储中。 [分区队列或主题](./service-bus-partitioning.md)由多个消息中转站处理，并存储在多个消息传送存储中。 这意味着分区的队列或主题的总吞吐量不再受到单个消息中转站或消息存储的性能所限制。 此外，消息传送存储的临时中断不会导致分区的队列或主题不可用。

请注意，使用分区实体时不保证排序。 如果某个分区不可用，你仍可从其他分区发送和接收消息。

## 最佳实践
<a id="best-practices" class="xliff"></a>
### Azure 服务总线的最佳实践有哪些？
<a id="what-are-some-azure-service-bus-best-practices" class="xliff"></a>
* [使用服务总线改进性能的最佳实践][Best practices for performance improvements using Service Bus] - 本文说明如何在交换消息时优化性能。

### 创建实体前的须知事项有哪些？
<a id="what-should-i-know-before-creating-entities" class="xliff"></a>
队列和主题的以下属性是固定不变的。 预配实体时请考虑到这一点，因为若要修改属性，就必须创建新的替代实体。

-   大小

-   分区

-   会话

-   重复检测

-   快速实体

## <a name="service-bus-pricing"></a> 定价
本部分回答了一些关于服务总线定价结构的常见问题。

[服务总线定价和计费](./service-bus-pricing-billing.md)一文介绍了服务总线中的计费计量，有关服务总线定价选项的信息，请参阅[服务总线定价详细信息](https://www.azure.cn/pricing/details/messaging/)。

还可以访问 [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083)（Azure 支持常见问题解答），了解一般的 Azure 定价信息。 

### 服务总线如何收取费用？
<a id="how-do-you-charge-for-service-bus" class="xliff"></a>
有关服务总线定价的完整信息，请参阅 [服务总线定价详细信息][Pricing overview]。 除标示的价格外，你还需为在其中部署应用程序的数据中心之外的相关数据输出支付费用。

### 服务总线的哪些使用情况受数据传输限制？ 哪些不受其限制？
<a id="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not" class="xliff"></a>
在给定 Azure 区域内的任何数据传输和入站数据传输均不收费。 

### 服务总线是否对存储收费？
<a id="does-service-bus-charge-for-storage" class="xliff"></a>

否，服务总线不对存储收费。 但是，对每个队列/主题可以保留的数据最大量设有配额限制。 请参阅下一个常见问题。

## 配额
<a id="quotas" class="xliff"></a>

有关服务总线限制和配额的列表，请参阅[服务总线配额概述][Quotas overview]。

### 服务总线是否有任何使用率配额？
<a id="does-service-bus-have-any-usage-quotas" class="xliff"></a>
默认情况下，对于任何云服务，Microsoft 每月都会设置聚合的使用率配额，此配额基于客户的所有订阅进行计算。 由于我们清楚你可能需要这些限制之外的更多限制，因此，请随时联系客户服务，以便我们了解你的需求并相应地调整这些限制。 对于服务总线，总用量配额是每月 50 亿条消息。

虽然我们保留禁用在给定月份超过使用率配额的客户帐户的权利，但我们仍然会在采取任何措施前发送电子邮件通知并会多次尝试与客户联系。 超过这些配额的客户仍将负责超出配额的费用。

至于 Azure 上的其他服务，服务总线会强制使用一组特定配额，以确保资源的公平使用。 可以在[服务总线配额概述][Quotas overview]中找到有关这些配额的更多详细信息。

## 故障排除
<a id="troubleshooting" class="xliff"></a>
### Azure 服务总线 API 生成了哪些异常？建议采取什么操作？
<a id="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions" class="xliff"></a>
有关可能的服务总线异常的列表，请参阅[异常概述][Exceptions overview]。

### 什么是共享访问签名？哪些语言支持生成签名？
<a id="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature" class="xliff"></a>
共享访问签名是基于 SHA–256 安全哈希或 URI 的身份验证机制。 有关如何在 Node、PHP、Java 和 C\# 中生成自有签名的信息，请参阅[共享访问签名][Shared Access Signatures]一文。

## 订阅和命名空间管理
<a id="subscription-and-namespace-management" class="xliff"></a>
### 如何将命名空间迁移到另一个 Azure 订阅？
<a id="how-do-i-migrate-a-namespace-to-another-azure-subscription" class="xliff"></a>

可以使用 [Azure 门户](https://portal.azure.cn)或 PowerShell 命令，将命名空间从一个 Azure 订阅移到另一个 Azure 订阅。 若要执行此操作，命名空间必须已处于活动状态。 执行这些命令的用户必须是源订阅和目标订阅的管理员。

#### 门户
<a id="portal" class="xliff"></a>

若要使用 Azure 门户将服务总线命名空间迁移到其他订阅，可按照[此处](../azure-resource-manager/resource-group-move-resources.md#use-portal)的说明操作。 

#### PowerShell
<a id="powershell" class="xliff"></a>

下面的 PowerShell 命令序列将命名空间从一个 Azure 订阅迁移到另一个 Azure 订阅。 若要执行此操作，命名空间必须已经处于活动状态，而且运行 PowerShell 命令的用户必须同时是源订阅和目标订阅的管理员。

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## 后续步骤
<a id="next-steps" class="xliff"></a>
若要了解有关服务总线的详细信息，请参阅以下主题。

- [服务总线概述](./service-bus-messaging-overview.md)
- [Azure 服务总线体系结构概述](./service-bus-fundamentals-hybrid-solutions.md)
- [服务总线队列入门](./service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: ./service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: ./service-bus-outages-disasters.md
[Pricing overview]: https://www.azure.cn/pricing/details/messaging/
[Quotas overview]: ./service-bus-quotas.md
[Exceptions overview]: ./service-bus-messaging-exceptions.md
[Shared Access Signatures]: ./service-bus-sas.md
