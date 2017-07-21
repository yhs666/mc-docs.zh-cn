---
title: "使用现有的播放器播放内容 - Azure | Azure"
description: "本主题列出了你可以用来播放内容的现有播放器。"
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/23/2017
ms.date: 03/10/2017
ms.author: v-johch
ms.openlocfilehash: 46e6b29c3a2de4b37d08066d98c33790168209ed
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
#<a name="playing-your-content-with-existing-players"></a>使用现有播放器播放内容

Azure 媒体服务支持多种常用的流式处理格式，如平滑流、HTTP 实时流和 MPEG-Dash。 本主题列出了可用于测试流的现有播放器。

### <a name="the-azure-portal-media-services-content-player"></a>Azure 门户媒体服务内容播放器

**Azure** 门户提供了可用于测试视频的内容播放器。

单击所需视频，然后单击门户底部的“播放”按钮。

请注意以下事项：

* **媒体服务内容播放器** 从默认的流式处理终结点播放。 如果要从非默认流式处理终结点播放，请使用其他播放器。 例如 [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)。

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
使用 [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)按以下列任意格式播放内容（清除或受保护）：

- 平滑流
- MPEG DASH
- HLS
- 渐进式 MP4

###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>带令牌的 AES 加密

[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight 播放器

####<a name="monitoring"></a>监视

[http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>带令牌的 PlayReady

[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>DASH 播放器

[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>其他

若要测试 HLS URL，还可以使用：

-  或
-  。

## <a name="developing-video-players"></a>开发视频播放器
有关如何开发自己的播放器的信息，请参阅[开发视频播放器](./media-services-develop-video-players.md)

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png