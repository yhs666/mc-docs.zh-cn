---
title: 情感 API C# 快速入门 | Microsoft Docs
description: 获取信息和代码示例，帮助自己快速开始使用认知服务中的情感 API 和 C#。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 05/23/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 3a117e1070ecdd9a38fb6d00d154d704f7fe0991
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407569"
---
# <a name="emotion-api-c-quick-start"></a>情感 API C# 快速入门
本文提供信息和代码示例，帮助读者快速开始使用[情感 API 识别方法](https://dev.cognitive.azure.cn/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)与 C# 来识别图像中一个或多个人员表达的情感。 

## <a name="prerequisites"></a>先决条件
- 在[此处](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)获取 Microsoft 认知情感 API Windows SDK
- 从 [Azure 门户](https://portal.azure.cn)获取订阅密钥。

## <a name="emotion-recognition-c-example-request"></a>情感识别 C# 示例请求

在 Visual Studio 中创建一个新的控制台解决方案，然后将 Program.cs 替换为以下代码。 更改 `string uri` 以使用订阅密钥的获取区域，并将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥。

```c#
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine();
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

            string uri = "https://api.cognitive.azure.cn/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            //A peak at the JSON response.
            Console.WriteLine(responseContent);
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>识别情感示例响应
成功调用返回人脸条目及其关联情感评分的数组，返回的内容已按人脸矩形大小的降序排序。 空响应表示未检测到任何人脸。 情感条目包含以下字段：
- faceRectangle - 人脸在图像中的矩形位置。
- scores - 图像中每张人脸的情感评分。 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]

