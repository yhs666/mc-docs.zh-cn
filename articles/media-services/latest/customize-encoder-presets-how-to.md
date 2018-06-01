---
title: 使用 Azure 媒体服务对自定义转换进行编码 | Azure
description: 本主题演示如何使用 Azure 媒体服务对自定义转换进行编码。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
origin.date: 05/17/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: e31f257a6fff189a69484fd751e93149b0a9f518
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475121"
---
# <a name="customize-encoder-presets"></a>自定义编码器预设

使用 Azure 媒体服务进行编码时，可以使用[流文件](stream-files-tutorial-with-api.md)教程中所示的内置 EncoderNamedPreset 或使用自定义预设。 本文中介绍的示例演示了如何创建自定义编解码器和层输出设置。

> [!Note]
>  所有比特率都以位/秒为单位。  

## <a name="download-the-sample"></a>下载示例

使用以下命令将包含完整 .NET Core 示例的 GitHub 存储库克隆到计算机：  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
可以在[此文件](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs)中找到本主题中讨论的代码。

## <a name="create-a-transform-with-a-custom-preset"></a>使用自定义预设创建转换 

创建新转换实例时，需要指定希望生成的输出内容[](https://docs.microsoft.com/rest/api/media/transforms)。 所需参数是 TransformOutput 对象，如以下代码所示。 每个 TransformOutput 包含一个预设。 预设介绍了视频和/或音频处理操作的分步说明，这些操作将用于生成所需的 TransformOutput。 以下 **TransformOutput** 创建自定义编解码器和层输出设置。

在创建时[转换](https://docs.microsoft.com/rest/api/media/transforms)，首先应检查是否其中一个已存在使用**获取**方法，如下面的代码中所示。  在媒体服务 v3 中，如果实体不存在（对名称进行不区分大小写检查），实体上的 **Get** 方法将返回 **null**。

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

