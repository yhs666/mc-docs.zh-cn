---
title: Azure 点播媒体编码器概述 | Microsoft Docs
description: 本主题简要介绍 Azure 点播媒体编码器。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/25/2019
ms.date: 08/26/2019
ms.author: v-jay
ms.openlocfilehash: f237271b7a812a601ce4d7aa75d0254b0bcf99df
ms.sourcegitcommit: 3aff96c317600eec69c4bf3b8853e9d4e44210b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2019
ms.locfileid: "69670939"
---
# <a name="overview-of-azure-on-demand-media-encoders"></a>Azure 点播媒体编码器概述 

## <a name="encoding-overview"></a>编码概述
Azure 媒体服务提供了多个用于在云中对媒体进行编码的选项。

开始使用媒体服务时，了解编解码器与文件格式之间的区别很重要。
编解码器是实现压缩/解压缩算法的软件，而文件格式是用于保存压缩视频的容器。

媒体服务所提供的动态打包，允许以媒体服务支持的流格式（MPEG DASH、HLS、平滑流式处理）传送自适应比特率 MP4 或平滑流式处理编码内容，而无须重新打包成这些流格式。

创建 AMS 帐户后，会将一个处于“已停止”状态的**默认**流式处理终结点添加到帐户。   若要开始对内容进行流式处理并利用动态打包和动态加密功能，必须确保要从其流式获取内容的流式处理终结点处于“正在运行”状态。

> [!Note]
> 每当流式处理终结点处于“正在运行”  状态时，就会对该终结点进行计费。

媒体服务支持在本文中介绍的以下按需编码器：

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)

本文简要概述了按需媒体编码器，并提供了指向介绍更多详细信息的文章的链接。 本主题还提供对编码器的比较。

默认情况下每个媒体服务帐户同时只能有一个活动的编码任务。 可以预留编码单元，使用它们可以同时运行多个编码任务，购买的每个编码预留单位对应一个任务。 有关信息，请参阅[缩放编码单位](media-services-scale-media-processing-overview.md)。

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-to-use"></a>如何使用
[如何使用 Media Encoder Standard 进行编码](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>格式
[格式和编解码器](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>预设
Media Encoder Standard 使用 [此处](media-services-mes-presets-overview.md)所述的编码器预设之一进行配置。

### <a name="input-and-output-metadata"></a>输入和输出元数据
[此处](media-services-input-metadata-schema.md)说明了编码器输入元数据。

[此处](media-services-output-metadata-schema.md)说明了编码器输出元数据。

### <a name="generate-thumbnails"></a>生成缩略图
有关信息，请参阅[如何使用 Media Encoder Standard 生成缩略图](media-services-advanced-encoding-with-mes.md#thumbnails)。

### <a name="trim-videos-clipping"></a>修剪视频（裁剪）
有关信息，请参阅[如何使用 Media Encoder Standard 剪裁视频](media-services-advanced-encoding-with-mes.md#trim_video)。

### <a name="create-overlays"></a>创建覆盖层
有关信息，请参阅[如何使用 Media Encoder Standard 创建覆盖层](media-services-advanced-encoding-with-mes.md#overlay)。

### <a name="see-also"></a>另请参阅
[媒体服务博客](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

### <a name="known-issues"></a>已知问题
如果输入视频不包含隐藏式字幕，输出资产仍包含一个空的 TTML 文件。

## <a name="related-articles"></a>相关文章
* [通过自定义 Media Encoder Standard 预设执行高级编码任务](media-services-custom-mes-presets-with-dotnet.md)
* [配额和限制](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://www.azure.cn/pricing/details/media-services/