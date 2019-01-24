---
title: SDK 和示例 - 内容审查器、Python、Java、Node.js 和 .NET
titlesuffix: Azure Cognitive Services
description: 获取内容审查器 SDK 和示例
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
origin.date: 02/27/2018
ms.date: 01/22/2019
ms.author: v-junlch
ms.openlocfilehash: 08c9ca75970a2cfe40502e0db977d9ca7958b3cf
ms.sourcegitcommit: f248afb1039011d34579baed2980f0632061f5b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54858103"
---
# <a name="content-moderator-sdks-and-samples"></a>内容审查器 SDK 和示例

## <a name="sdks-for-python-java-nodejs-and-net"></a>用于 Python、Java、Node.js 和 .NET 的 SDK

- [用于 Python 的内容审查器 SDK](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [用于 Java 的内容审查器 SDK](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [用于 Node.js 的内容审查器 SDK](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [用于 .NET 的内容审查器 SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>.NET SDK 示例

下面列出了使用用于 .NET 的 Azure 内容审查器 SDK 生成的代码示例的链接。

- **帮助程序库**：请参阅[快速入门](content-moderator-helper-quickstart-dotnet.md)。

### <a name="moderation"></a>审查 
- **图像审查**：[评估图像中是否有成人和挑逗性、文本和人脸](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs)。 请参阅[快速入门](image-moderation-quickstart-dotnet.md)。
- **自定义图像**：[使用自定义图像列表进行审查](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs)。 请参阅[快速入门](image-lists-quickstart-dotnet.md)。

> [!NOTE]
> 最多只能使用 5 个图像列表，每个列表中的图像数不得超过 10,000 张。
>

- **文本审查**：[筛查文本中是否有猥亵内容和个人身份信息 (PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs)。 请参阅[快速入门](text-moderation-quickstart-dotnet.md)。
- **自定义术语**：[使用自定义术语列表进行审查](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs)。 请参阅[快速入门](term-lists-quickstart-dotnet.md)。

> [!NOTE]
> 最多只能使用 5 个术语列表，每个列表中的术语数不得超过 10,000 个。
>

- **视频审查**：[扫描视频中是否有成人内容和挑逗性内容，并获取结果](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs)。 请参阅[快速入门](video-moderation-api.md)。

请参阅 [GitHub 上内容审查器 .NET 示例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator)中的所有 .NET 示例。

## <a name="rest-api-samples-in-c"></a>以 C# 编写的 REST API 示例

- [图像审查](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [文本审查](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [视频审查](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [图像评审](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [图像作业](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

有关这些示例的演练，请观看[点播网络研讨会](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)。



## <a name="on-demand-webinars"></a>点播网络研讨会
- [使用内容审查器进行大规模计算机辅助内容审查](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)

