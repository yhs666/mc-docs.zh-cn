---
title: "Azure 媒体服务动态打包概述 | Azure"
description: "本主题概述动态打包。"
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/25/2017
ms.date: 03/10/2017
ms.author: v-johch
ms.openlocfilehash: 952a8aff5a1a56dc88f1fdbaf80ea421dc02f8d6
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="dynamic-packaging"></a>动态打包
## <a name="overview"></a>概述
Microsoft Azure 媒体服务可用于向多种客户端技术（例如，iOS、XBOX、Silverlight、Windows 8）传送多种媒体源文件格式、媒体流格式和内容保护格式。 这些客户端可识别不同的协议，例如，iOS 需要 HTTP Live Streaming (HLS) V4 格式，Silverlight 和 Xbox 需要平滑流式处理。 如果你有一组自适应比特率（多比特率）MP4（ISO 基媒体 14496-12）文件或平滑流式处理文件要提供给了解 MPEG DASH、HLS 或平滑流式处理的客户端，则应利用媒体服务动态打包。

使用动态打包，你只需要创建一个包含一组自适应比特率 MP4 文件或自适应比特率平滑流文件的资产。 然后，点播流服务器会确保你以选定的协议按清单或分段请求中的指定格式接收流。 因此，你只需以单一存储格式存储文件并为其付费，然后媒体服务服务就会基于客户端的请求构建并提供相应响应。

下图显示传统编码和静态打包工作流。

![静态编码](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

下图显示了动态打包工作流。

![动态编码](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>常见方案

1. 上传一个输入文件（称为夹层文件）。 例如，H.264、MP4 或 WMV（有关受支持格式的列表，请参阅[Media Encoder Standard 支持的格式](./media-services-media-encoder-standard-formats.md)）。

1. 将夹层文件编码为 H.264 MP4 自适应比特率集。

1. 通过创建点播定位符来发布包含自适应比特率 MP4 集的资产。

1. 生成用于访问和流式传输内容的流 URL。

## <a name="preparing-assets-for-dynamic-streaming"></a>准备用于动态流式传输的资产
若要准备用于动态流式传输的资产，可以使用两个选项：

1. [上传主文件](./media-services-dotnet-upload-files.md)。
2. [使用 Media Encoder Standard 编码器生成 H.264 MP4 自适应比特率集](./media-services-dotnet-encode-with-media-encoder-standard.md)。
3. [流式传输内容](./media-services-deliver-content-overview.md)。

## <a id="unsupported_formats"></a>动态打包不支持的格式
动态打包不支持以下源文件格式。

* Dolby Digital MP4 文件。
* Dolby Digital 平滑流文件。