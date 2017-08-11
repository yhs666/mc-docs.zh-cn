---
title: "通过 Azure 门户使用 Media Encoder Standard 对资产进行编码 | Azure"
description: "本教程逐步演示如何通过 Azure 门户使用 Media Encoder Standard 对资产进行编码。"
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/24/2016
ms.date: 08/07/2017
ms.author: v-haiqya
ms.openlocfilehash: 34cf9356762a2f963df855a7eb066ecb2b5fb74d
ms.sourcegitcommit: dc2d05f1b67f4988ef28a0931e6e38712f4492af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>通过 Azure 门户使用 Media Encoder Standard 对资产进行编码
> [!NOTE]
> 要完成本教程，需要一个 Azure 帐户。 有关详细信息，请参阅 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)。 
> 
> 

使用 Azure 媒体服务时最常见的方案之一是将自适应比特率流传送至客户端。 媒体服务支持以下自适应比特率流式处理技术：HTTP Live Streaming (HLS)、平滑流式处理和 MPEG DASH。 若要针对自适应比特率流式处理准备视频，需要将源视频编码为多比特率文件。 应使用 **Media Encoder Standard** 编码器来对视频编码。  

此外，媒体服务提供的动态打包可让用户以下述流式处理格式传送多比特率 MP4，而无需重新打包成这些流式处理格式：MPEG DASH、HLS 和平滑流式处理。 通过动态打包，你只需要存储及支付一种存储格式的文件，媒体服务会根据客户端的要求创建并提供适当的响应。

若要利用动态打包，需要将源文件编码成一组多比特率 MP4 文件（本部分稍后将演示编码步骤）。

若要缩放媒体处理，请参阅[此](media-services-portal-scale-media-processing.md)主题。

## <a name="encode-with-the-azure-portal"></a>使用 Azure 门户编码
本部分介绍使用 Media Encoder Standard 为内容编码时可以执行的步骤。

1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 Azure 媒体服务帐户。
2. 在“设置”窗口中，选择“资产”。  
3. 在“资产”窗口中，选择想要编码的资产。
4. 按“编码”按钮。
5. 在“对资产进行编码”窗口中，选择“Media Encoder Standard”处理器和预设。 例如，如果知道输入视频的分辨率为 1920x1080 像素，则可使用“H264 多比特率 1080p”预设。 有关预设的详细信息，请参阅[此](media-services-mes-presets-overview.md)文 - 务必选择最适合输入视频的预设。 如果视频的分辨率低 (640x360)，则不应使用默认的“H264 多比特率 1080p”预设。

   为了方便管理，系统允许编辑输出资产的名称，以及作业的名称。

   ![对资产进行编码](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. 按“创建”。

## <a name="next-step"></a>后续步骤
可通过 Azure 门户监视编码作业的进度，如[此](media-services-portal-check-job-progress.md)文中所述。  

<!--Update_Description: new file-->