---
title: 示例：调用分析图像 API - 计算机视觉
titlesuffix: Azure Cognitive Services
description: 了解如何通过使用 Azure 认知服务中的 REST 调用计算机视觉 API。
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: sample
origin.date: 01/20/2017
ms.date: 01/07/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: 7bd8ab772b19d8938122f0d54f76d7dfb69ae70e
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083811"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>示例：如何调用计算机视觉 API

本指南演示如何使用 REST 调用计算机视觉 API。 这些示例是使用计算机视觉 API 客户端库以 C# 编写的，也是作为 HTTP POST/GET 调用编写的。 我们将重点介绍：

-   如何获取 "Tags"、"Description" 和 "Categories"。
-   如何获取“特定领域”的信息（名人）。

### <a name="Prerequisites">先决条件</a> 
本地存储图像的图像 URL 或路径。
  - 支持的输入方法：原始图像二进制，采用应用程序/八位字节流或图像 URL 的形式
  - 支持的图像格式：JPEG、PNG、GIF 和 BMP
  - 图像文件大小：小于 4MB
  - 图像维度：大于 50 x 50 像素
  
下面的示例演示了以下功能：

1. 分析图像并返回标记数组和说明。
2. 使用特定领域的模型（具体说来就是“名人”模型）分析图像，并在返回的 JSON 中获取相应的结果。

功能细分为：

  - **选项一：** 范围内分析 - 仅分析给定模型
  - **选项二：** 强化分析 - 经过分析可提供具有 [86 个类别分类](../Category-Taxonomy.md)的更多详细信息
  
### <a name="Step1">步骤 1：授权 API 调用</a> 
每次调用计算机视觉 API 都需要提供订阅密钥。 需通过查询字符串参数传递此密钥，或者在请求标头中指定此密钥。 

若要获取订阅密钥，请参阅[如何获取订阅密钥](../Vision-API-How-to-Topics/HowToSubscribe.md
)。

**1.** 若要通过查询字符串传递订阅密钥，请参阅下面的计算机视觉 API 示例：

```https://api.cognitive.azure.cn/vision/v1.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** 也可以在 HTTP 请求标头中指定订阅密钥的传递方式：

```ocp-apim-subscription-key: <Your subscription key>```

**3.** 使用客户端库时，订阅密钥通过 VisionServiceClient 的构造函数传入：

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">步骤 2：将图像上传到计算机视觉 API 服务并取回标记、说明和名人</a>
若要执行计算机视觉 API 调用，基本方式是直接上传图像。 为此，可将包含 application/octet-stream 内容类型的“POST”请求连同从图像中读取的数据一起发送。 至于 "Tags" 和 "Description"，此上传方法对于所有计算机视觉 API 调用都是相同的。 唯一的区别是用户指定的查询参数。 

下面介绍如何获取给定图像的 "Tags" 和 "Description"：

**选项一：** 获取“标记”列表和一个“说明”
```
POST https://api.cognitive.azure.cn/vision/v1.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```
```
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```
**选项二：** 只获取 "Tags" 的列表，或者只获取 "Description" 的列表：

###### <a name="tags-only"></a>仅标记：
```
POST https://api.cognitive.azure.cn/vision/v1.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>仅说明：
```
POST https://api.cognitive.azure.cn/vision/v1.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>下面介绍如何获取特定领域的分析（在我们的示例中为名人分析）。

**选项一：** 范围内分析 - 仅分析给定模型
```
POST https://api.cognitive.azure.cn/vision/v1.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
对于此选项来说，所有其他参数参数 {visualFeatures, details} 均无效。 若要查看所有支持的模型，请使用： 
```
GET https://api.cognitive.azure.cn/vision/v1.0/models 
var models = await visionClient.ListModelsAsync();
```
**选项二：** 强化分析 - 经过分析可提供具有 [86 个类别分类](../Category-Taxonomy.md)的更多详细信息

如果应用程序用户除了获取一个或多个特定领域模型中的详细信息，还需要获取泛型图像分析信息，则可使用带模型查询参数的扩展型 v1 API。
```
POST https://api.cognitive.azure.cn/vision/v1.0/analyze?details=celebrities
```
调用此方法时，将先调用“86 类”分类器。 如果某个类别与已知/匹配模型的类别相符，则会进行第二轮分类器调用。 例如，如果 "details=all"，或者 "details" 包括 ‘celebrities’，则会在调用“86 类”分类器后调用名人模型，结果就会包括人员类别。 与“选项一”相比，这会增加对名人感兴趣的用户的延迟。

在这种情况下，所有 v1 查询参数都会表现得一样。  如果未指定 visualFeatures=categories，则会隐式启用它。

### <a name="Step3">步骤 3：检索并了解 analyze&visualFeatures=Tags, Description 的 JSON 输出</a>

下面是一个示例：
```
  {
    “tags”: [
    {
    "name": "outdoor",
        "score": 0.976
    },
    {
    "name": "bird",
        "score": 0.95
    }
            ],
    “description”: 
    {
    "tags": [
    "outdoor",
    "bird"
            ],
    "captions": [
    {
    "text”: “partridge in a pear tree”,
            “confidence”: 0.96
    }
                ]
    }
  }
```
字段   | 类型  | 内容
------|------|------|
标记    | object    | 标记数组的顶级对象
tags[].Name | 字符串    | 标记分类器中的关键字
tags[].Score    | number    | 置信度，介于 0 和 1 之间。
说明  | object   | 说明的顶级对象。
description.tags[] |    字符串  | 标记列表。  如果因置信度不够而无法生成标题，则调用方能够获得的唯一信息可能就是标记。
description.captions[].text | 字符串    | 描述图像的短语。
description.captions[].confidence   | number    | 短语的置信度。

### <a name="Step4">步骤 4：检索并了解特定于域的模型的 JSON 输出</a>

**选项一：** 范围内分析 - 仅分析给定模型

输出将会是标记数组，示例如下：
```
  { 
    "result": [ 
      { 
    "name": "golden retriever", 
    "score": 0.98
    },
    { 
    "name": "Labrador retriever", 
    "score": 0.78
    }
              ]
  }
```

**选项二：** 强化分析 - 经过分析可提供具有 86 个类别分类的更多详细信息

对于使用“选项二(强化分析)”的特定领域模型，会扩展类别返回类型。 示例如下：
```
  {
    "requestId": "87e44580-925a-49c8-b661-d1c54d1b83b5",
    "metadata":     {
      "width": 640,
      "height": 430,
      "format": "Jpeg"
                    },
    "result": {
      "celebrities": 
      [
        {
        "name": "Richard Nixon",
        "faceRectangle": {
          "left": 107,
          "top": 98,
          "width": 165,
          "height": 165
                         },
        "confidence": 0.9999827
        }
      ]
  }
```

类别字段是一个列表，其中包含一个或多个 [86 类](../Category-Taxonomy.md)（按原始分类）。 另请注意，以下划线结尾的类别会匹配该类别及其子类别（例如，名人模型的 people_ 和 people_group）。

字段   | 类型  | 内容
------|------|------|
Categories | object | 顶级对象
categories[].name    | 字符串   | “86 类”分类中的名称
categories[].score  | number    | 置信度，介于 0 和 1 之间
categories[].detail  | 对象？      | 可选详细信息对象

请注意，如果多个类别匹配（例如，当 model=celebrities 时，“86 类”分类器返回了一个针对 people_ 和 people_young 的置信度），则会将详细信息附加到最广泛级别的匹配（即该示例中的 people_）。

### <a name="Errors">错误响应</a>
这些内容与 vision.analyze 相同，但增加了 NotSupportedModel 错误 (HTTP 400)，该错误可能会在使用“选项一”和“选项二”的情况下返回。 对于“选项二(强化分析)”，如果无法识别详细指定的任何模型，则 API 会返回 NotSupportedModel，即使其中的一个或多个是有效的。  用户可以调用 listModels，了解哪些模型受支持。

### <a name="Summary">摘要</a>

计算机视觉 API 的基本功能如下：如何上传图像并在返回的结果中检索有价值的元数据。

若要使用 REST API，请转到[计算机视觉 API 参考](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)。
 

<!-- Update_Description: update metedata properties -->