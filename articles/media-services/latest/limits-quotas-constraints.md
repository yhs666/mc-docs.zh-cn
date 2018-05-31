---
title: Azure 媒体服务 v3 中的配额和限制 | Azure
description: 本主题介绍 Azure 媒体服务 v3 中的配额和限制
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
origin.date: 03/19/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: e09d78a7acbe8e6020a26fe6420aadb78be081b5
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475177"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Azure 媒体服务 v3 中的配额和限制

本主题介绍 Azure 媒体服务 v3 中的配额和限制。

| 资源 | 默认限制 | 
| --- | --- | 
| 每个 Azure 媒体服务帐户的资产数 | 1,000,000|
| 每个作业的 JobInputs | 100 |
| 每个作业的 JobOutputs | 30（固定） |
| 文件大小| 在某些情况下，支持在媒体服务中处理的最大文件大小存在限制。 <sup>(1)</sup> |
| 每个媒体服务帐户的作业数 | 50,000<sup>(2)</sup> |

| 单个订阅中的媒体服务帐户数 | 25（固定）| | StreamingPolicies | 1,000,000<sup>(3)</sup> |

| 存储帐户 | 1,000<sup>(4)</sup>（固定）| | 每个媒体服务帐户中处于运行状态的流式处理终结点数 | 2 | | 每个媒体服务帐户的转换数 | 20 | | 一次与资产关联的唯一 StreamingLocator 数 | 20<sup>(5)</sup> |
  
<sup>1</sup> 在 Azure Blob 存储中，单个 Blob 目前支持的最大大小为 5 TB。 但是，Azure 媒体服务会根据服务使用的 VM 大小应用其他限制。 如果源文件大于 260 GB，作业可能会失败。

<sup>2</sup> 这个数字包括已排队的、已完成的、活动的和已取消的作业。 不包括已删除的作业。 

即使记录总数低于最大配额，也会自动删除帐户中所有超过 90 天的作业记录。 

<sup>3</sup> 不同媒体服务策略的策略数限制为 1,000,000 个 StreamingPolicy 条目（例如，StreamingLocator 策略或 ContentKeyAuthorizationPolicy 的情况就是如此）。 

>[!NOTE]
> 如果经常使用相同的天数/访问权限等，则应使用相同的策略 ID。 

<sup>4</sup> 存储帐户必须来自同一 Azure 订阅。

<sup>5</sup> StreamingLocator 不用于管理按用户的访问控制。 要为不同用户提供不同的访问权限，请使用数字权限管理 (DRM) 解决方案。


