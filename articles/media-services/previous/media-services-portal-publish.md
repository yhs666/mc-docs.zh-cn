---
title: 在 Azure 门户中发布内容 | Microsoft Docs
description: 本教程逐步演示如何在 Azure 门户中发布内容。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/07/2017
ms.date: 01/07/2019
ms.author: v-jay
ms.openlocfilehash: d3d5375965d372bc80a3aafc42b6e40f6b69dc4d
ms.sourcegitcommit: 4f91d9bc4c607cf254479a6e5c726849caa95ad8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996153"
---
# <a name="publish-content-in-the-azure-portal"></a>在 Azure 门户中发布内容
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>概述
> [!NOTE]
> 若要完成本教程，需要一个 Azure 帐户。 有关详细信息，请参阅 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)。 
> 
> 

若要为用户提供一个可用来流式传输内容或下载内容的 URL，首先必须通过创建定位符来发布资产。 通过定位符，可访问资产文件。 Azure 媒体服务支持两种类型的定位符： 

* **流式处理 (OnDemandOrigin) 定位符**。 流式处理定位符用于自适应流式处理。 自适应流式处理的示例包括 Apple HTTP Live Streaming (HLS)、Microsoft 平滑流式处理，以及经 HTTP 的动态自适应流式处理（DASH，也称 MPEG-DASH）。 要创建流式处理定位符，资产必须包括一个 .ism 文件。 例如， http://amstest.streaming.mediaservices.chinacloudapi.cn/61b3da1d-96c7-489e-bd21-c5f8a7494b03/scott.ism/manifest。
* **渐进式（共享访问签名）定位符**。 渐进式定位符用于通过渐进式下载来传输视频。

要生成 HLS 流 URL，请将 (format=m3u8-aapl) 附加到 URL：

    {streaming endpoint name-media services account name}/{locator ID}/{file name}.ism/Manifest(format=m3u8-aapl)

要生成流式处理 URL 来播放平滑流资产，请使用以下 URL 格式：

    {streaming endpoint name-media services account name}/{locator ID}/{file name}.ism/Manifest

要生成 MPEG DASH 流式处理 URL，请将 (format=mpd-time-csf) 附加到 URL：

    {streaming endpoint name-media services account name}/{locator ID}/{file name}.ism/Manifest(format=mpd-time-csf)

共享访问签名 URL 格式如下：

    {blob container name}/{asset name}/{file name}/{shared access signature}

有关详细信息，请参阅[传送内容概述](media-services-deliver-content-overview.md)。

> [!NOTE]
> 2015 年 3 月之前在 Azure 门户中创建的定位符的过期日期为两年后。  
> 
> 

可以使用 [REST API](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) 或 [.NET API](https://go.microsoft.com/fwlink/?LinkID=533259) 来更新定位符的过期日期。 

> [!NOTE]
> 更新共享访问签名定位符的过期日期时，URL 会发生变化。

### <a name="to-use-the-portal-to-publish-an-asset"></a>使用门户发布资产
1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 Azure 媒体服务帐户。
2. 选择“设置” > “资产”。 选择想要发布的资产。
3. 选择“发布”按钮。
4. 选择定位符类型。
5. 选择“设置” （应用程序对象和服务主体对象）。
   
    ![发布视频](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL 会添加到“已发布的 URL” 列表。

## <a name="play-content-in-the-portal"></a>在门户中播放内容
可以在 Azure 门户中的内容播放器上测试视频。

选择视频，然后选择“播放”按钮。

![在 Azure 门户中播放视频](./media/media-services-portal-vod-get-started/media-services-play.png)

请注意以下事项：

* 确保视频已发布。
* Azure 门户媒体播放器从默认的流式处理终结点播放。 若要从非默认流式处理终结点播放，请选择并复制该 URL，然后将其粘贴到其他播放器中。 例如，可以在 [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) 上测试视频。
* 用于进行流式处理的流式处理终结点必须处于运行状态。  
<!--Update_Description: wording update-->
