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
ms.date: 06/25/2018
ms.author: v-nany
ms.openlocfilehash: 79abf0c1573e5cf366599723c711c0eccfd305ef
ms.sourcegitcommit: d6ff9675cc2288f5d7971ef003422d62ff02a102
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2018
ms.locfileid: "36748410"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Azure 媒体服务 v3 中的配额和限制

本文介绍 Azure 媒体服务 v3 中的配额和限制。

| 资源 | 默认限制 | 
| --- | --- | 
| 每个 Azure 媒体服务帐户的资产数 | 1,000,000|
| 每个作业的 JobInputs | 50 |
| 每个作业的 JobOutputs | 20（固定） |
| 文件大小| 在某些情况下，支持在媒体服务中处理的最大文件大小存在限制。 <sup>(1)</sup> |
| 每个媒体服务帐户的作业数 | 1,000,000 <sup>(2)</sup>|
| 列出转换数|对响应进行分页，每页 1000 个转换|
| 列出作业数|对响应进行分页，每页 500 个作业|
| 每个媒体服务帐户的 LiveEvents |5|
| 单个订阅中的媒体服务帐户数 | 25（固定） |
| StreamingPolicies | 1,000,000<sup>(3)</sup> |
| 每个 LiveEvent 处于运行状态的 LiveOutputs |3|
| 每个 LiveEvent 处于已停止状态的 LiveOutputs |50|
| 存储帐户 | 100<sup>(4)</sup>（固定） |
| 每个媒体服务帐户处于运行状态的流式处理终结点数|2|
| 每个媒体服务帐户的转换数 | 100 |
| 转换中的 TransformOutput 数| 20 个|
| 一次与一个资产关联的唯一 StreamingLocators | 20<sup>(5)</sup> |

<sup>1</sup> 在 Azure Blob 存储中，单个 Blob 目前支持的最大大小为 5 TB。 但是，Azure 媒体服务会根据服务使用的 VM 大小应用其他限制。 如果源文件大于 260 GB，作业可能会失败。

<sup>2</sup> 这个数字包括已排队的、已完成的、活动的和已取消的作业。 不包括已删除的作业。 

即使记录总数低于最大配额，也会自动删除帐户中所有超过 90 天的作业记录。 

<sup>3</sup> 使用自定义的 [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) 时，应为媒体服务帐户设计有限的一组此类策略，并在需要同样的加密选项和协议时重新将这些策略用于 StreamingLocators。 不应为每个 StreamingLocator 创建新的 StreamingPolicy。

<sup>4</sup> 存储帐户必须来自同一 Azure 订阅。

<sup>5</sup> StreamingLocator 不用于管理按用户的访问控制。 要为不同用户提供不同的访问权限，请使用数字权限管理 (DRM) 解决方案。


