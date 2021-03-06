---
title: Swagger 文档 - 语音服务
titleSuffix: Azure Cognitive Services
description: Swagger 文档可用于自动生成适用于多种编程语言的 SDK。 Swagger 支持服务中的所有操作
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
origin.date: 07/05/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.openlocfilehash: 2d158f13f05f6cd78713aab4f036c316f5005d62
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389271"
---
# <a name="swagger-documentation"></a>Swagger 文档

语音服务提供了一个 Swagger 规范，用于与少量 REST API 交互，这些 REST API 用于导入数据、创建模型、测试模型准确性、创建自定义终结点、排列批量听录以及管理订阅。 可使用这些 API 以编程方式完成通过自定义语音门户提供的大部分操作。

> [!NOTE]
> 支持将语音转文本操作作为 REST API 使用，而这些 API 记录在 Swagger 规范中。

## <a name="generating-code-from-the-swagger-specification"></a>从 Swagger 规范生成代码

[Swagger 规范](https://chinaeast2.cris.azure.cn/swagger/ui/index)包含可快速测试各种路径的选项。 但有时需要为所有路径生成代码，从而创建可基于未来的解决方案的单个调用库。 让我们看看生成 Python 库的过程。

你需要将 Swagger 设置为与语音服务订阅相同的区域。 可在 Azure 门户中的语音服务资源下确认区域。 有关受支持的区域的完整列表，请参阅[区域](regions.md)。

1. 转到 https://editor.swagger.io
2. 单击“文件”，然后单击“导入”  
3. 输入 swagger URL，包括语音服务订阅的区域`https://<your-region>.cris.azure.cn/docs/v2.0/swagger`
4.  单击“生成客户端”，然后选择 Python
5. 保存客户端库

可使用 [GitHub 上的语音服务示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)生成的 Python 库。

## <a name="reference-docs"></a>参考文档

* [REST (Swagger)：批量听录和自定义](https://chinaeast2.cris.azure.cn/swagger/ui/index)
* [REST API：语音转文本](rest-speech-to-text.md)

## <a name="next-steps"></a>后续步骤

* [GitHub 上的语音服务示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)。
* [获取语音服务订阅密钥以进行试用](get-started.md)
