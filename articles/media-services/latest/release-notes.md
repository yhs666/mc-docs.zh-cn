---
title: Azure 媒体服务 v3 发行说明 | Azure
description: 为了让大家随时了解最新的开发成果，本文提供了 Azure 媒体服务 v3 的最新更新。
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
ms.openlocfilehash: ebaeecc495edbc0d60609a6298344df0987a24dc
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52648290"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Azure 媒体服务 v3（预览）发行说明 

为了让大家随时了解最新的开发成果，本文将提供以下方面的信息：

* 最新版本
* 已知问题
* Bug 修复
* 已弃用的功能
* 更改计划

## <a name="may-07-2018"></a>2018 年 5 月 7 日

### <a name="net-sdk"></a>.NET SDK

.Net SDK 具有以下功能：

1. 转换和作业，用于对媒体内容来进行编码或分析。 有关示例，请参阅[流式传输文件](stream-files-tutorial-with-api.md)和[分析](analyze-videos-tutorial-with-api.md)。
2. StreamingLocators，用于发布内容并将其流式传输到最终用户设备
3. StreamingPolicies 和 ContentKeyPolicies，用于在传送内容时配置密钥传递和内容保护 (DRM)。
4. LiveEvents 和 LiveOutputs，用于配置实时传送视频流内容的引入和归档。
5. 资产，用于在 Azure 存储中存储和发布媒体内容。 
6. StreamingEndpoints，用于配置和缩放实时和点播媒体内容的动态打包、加密和流式处理。

### <a name="known-issues"></a>已知问题

已知问题：

通过指向源内容的 HTTPS URL (JobInputHttp) 提交作业时，请确保 HTTP 服务器支持“HEAD”请求。 否则，会拒绝该作业。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [概述](media-services-overview.md)
