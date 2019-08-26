---
title: include 文件
description: include 文件
services: billing
author: rothja
ms.service: billing
ms.topic: include
origin.date: 05/18/2018
ms.date: 09/26/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 7cb7d0e12b2b0fdde7ebc38a291bfdd521132636
ms.sourcegitcommit: b418463868dac6b3c82b292f70d4a17bc5e01e95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2019
ms.locfileid: "69578674"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[订阅](https://www.azure.cn/pricing)的 vCPU 数量 <sup>1</sup> |20 |10,000 |
| 每个订阅的[共同管理员数](/billing/billing-add-change-azure-subscription-administrator) |200 |200 |
| 每个订阅在每个区域中的[存储帐户数](../articles/storage/common/storage-quickstart-create-account.md)<sup>2</sup> |200 |250 |
| 每个订阅的[云服务数](../articles/cloud-services/cloud-services-choose-me.md) |20 |200 |
| 每个订阅的[本地网络数](http://msdn.microsoft.com/library/jj157100.aspx) |10 |500 |
| 每个订阅的 SQL 数据库服务器数 |6 |200 |
| 每个订阅的 DNS 服务器 |9 |100 |
| 每个订阅的保留的 IP |20 |100 |
| 每个订阅的托管服务证书数 |199 |199 |
| 每个订阅的[地缘组数](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) |256 |256 |


<sup>1</sup>特小实例作为一个 vCPU 至 vCPU 限制计数，即使使用了部分 CPU 核心。

<sup>2</sup>此存储帐户限制包括标准和高级存储帐户。 如果在某特定区域中需要的存储帐户多于 200 个，请通过 [Azure 支持](https://www.azure.cn/support/faq/)提出请求。 Azure 存储团队将评审业务案例，对于特定区域最多可以批准 250 个存储帐户。 

<!-- ms.date: 09/26/2018 -->
