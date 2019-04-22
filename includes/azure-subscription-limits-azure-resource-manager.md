---
title: include 文件
description: include 文件
services: billing
author: rockboyfor
ms.service: billing
ms.topic: include
origin.date: 04/02/2019
ms.date: 04/15/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 0d9f37c5bb9588f033a67bf7bb5b34a31ebc4746
ms.sourcegitcommit: 9f7a4bec190376815fa21167d90820b423da87e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "59532450"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[订阅](https://www.azure.cn/pricing)的 VM 数 |每个区域 25,000 个<sup>1</sup>。 |每个区域 25,000 个。 |
| 每个[订阅](https://www.azure.cn/pricing)的 VM 核心总数 |每个区域 20 个<sup>1</sup> | 请联系支持人员。 |
| VM 系列（例如 Dv2 和 F）、每个[订阅](https://www.azure.cn/pricing)的核心数 |每个区域 20 个<sup>1</sup> | 请联系支持人员。 |
| 每个订阅的[协同管理员数](/billing/billing-add-change-azure-subscription-administrator) |不受限制。 |不受限制。 |
| 每个订阅在每个区域中的[存储帐户数](../articles/storage/common/storage-quickstart-create-account.md) |250 |250 |
| 每个订阅的[资源组数](../articles/azure-resource-manager/resource-group-overview.md) |980 |980 |
| 每个订阅的[可用性集数](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) |每个区域 2,000 个。 |每个区域 2,000 个。 |
| Resource Manager API 读取次数 |每小时 15,000 次 |每小时 15,000 次 |
| Resource Manager API 写入次数 |每小时 1,200 次 |每小时 1,200 次 |
| Azure 资源管理器 API 请求大小 |4,194,304 字节。 |4,194,304 字节。 |
| 每个订阅的标记数<sup>2</sup> |不受限制。 |不受限制。 |
| 每个订阅的唯一标记计算<sup>2</sup> | 10,000 | 10,000 |
| 每个订阅的[云服务数](../articles/cloud-services/cloud-services-choose-me.md) |不适用<sup>3</sup> |不适用<sup>3</sup> |
| 每个订阅的[地缘组数](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) |不适用<sup>3</sup> |不适用<sup>3</sup> |

<sup>1</sup>默认限制因套餐类别类型（例如 1 元试用、提前支付）以及系列（例如 Dv2、F、G）而异。

<sup>2</sup>每个订阅可以应用无限数量的标记。 每个资源或资源组的标记数限制为 15。 当标记数少于或等于 10,000 时，资源管理器仅返回订阅中[唯一标记名和值的列表](https://docs.microsoft.com/rest/api/resources/tags)。 即使数目超过 10,000，也仍可按标记查找资源。  

<sup>3</sup>使用 Azure 资源组和资源管理器时不再需要这些功能。

> [!NOTE]
> 虚拟机核心数存在区域总数限制。 区域大小系列（例如 Dv2 和 F）也存在限制。这些限制是单独实施的。 例如，假设某个订阅在中国东部的 VM 核心数限制为 30 个，即 A 系列的核心数限制为 30，D 系列的核心数限制也为 30。 此订阅可以部署 30 个 A1 VM、30 个 D1 VM，或两者的组合，但总共不能超过 30 个核心。 例如，10 个 A1 VM 和 20 个 D1 VM 就是一种组合。  
> <!-- -->
> 
> 

