---
title: Java for Android 情感 API 快速入门 | Microsoft Docs
description: 获取信息和代码示例，帮助自己快速开始使用认知服务中的情感 API 和 Java for Android。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 05/23/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: bc8b3be5b2ef04c2742061a95a4a97c6be78443d
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407564"
---
# <a name="emotion-api-java-for-android-quick-start"></a>Java for Android 情感 API 快速入门
本文提供信息和代码示例来帮助读者快速开始使用情感 API Android 客户端库中的[情感识别方法](https://dev.cognitive.azure.cn/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)。 示例演示如何使用 Java 识别人员表达的情感。 

## <a name="prerequisites"></a>先决条件
- 在[此处](https://github.com/Microsoft/Cognitive-emotion-android)获取 Java for Android 情感 API SDK
- 从 [Azure 门户](https://portal.azure.cn)获取订阅密钥

## <a name="recognize-emotions-java-for-android-example-request"></a>识别情感 Java for Android 示例请求

```java
// // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;

public class Main
{
    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            URIBuilder uriBuilder = new URIBuilder("https://api.cognitive.azure.cn/emotion/v1.0/recognize");

            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers. Replace the example key below with your valid subscription key.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

            // Request body. Replace the example URL below with the URL of the image you want to analyze.
            StringEntity reqEntity = new StringEntity("{ \"url\": \"http://example.com/images/test.jpg\" }");
            request.setEntity(reqEntity);

            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                System.out.println(EntityUtils.toString(entity));
            }
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
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

