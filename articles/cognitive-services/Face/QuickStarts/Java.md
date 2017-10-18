---
title: "人脸 API Java 快速入门 | Microsoft Docs"
description: "获取信息和代码示例，帮助自己快速开始使用认知服务中的人脸 API 和 Java。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 06/21/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 550df55cff68dc2c87b73544c10ee2d7c9bc73e9
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="face-api-java-quick-starts"></a>人脸 API Java 快速入门
本文提供信息和代码示例来帮助读者快速开始使用人脸 API 和 Java 来完成以下任务： 
- [检测图像中的人脸](#Detect) 
- [创建人员组](#Create)

## <a name="prerequisites"></a>先决条件
- 在[此处](https://github.com/Microsoft/Cognitive-face-android)获取 Microsoft 人脸 API Android SDK
- 在[此处](../../Computer-vision/Vision-API-How-to-Topics/HowToSubscribe.md)详细了解如何获取免费订阅密钥

## 使用人脸 API 通过 Java 检测图像中的人脸 <a name="Detect"> </a>
使用[“人脸 - 检测”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)可以检测图像中的人脸，并返回人脸属性，包括：
- 人脸 ID：在多种人脸 API 方案使用的唯一 ID。 
- 人脸矩形：左侧坐标、顶部坐标、宽度和高度，指示人脸在图像中的位置。
- 地标：27 点人脸地标数组，指向人脸组成部分的重要位置。
- 面部属性包括年龄、性别、笑容强度、头部姿势和面部毛发。 

#### <a name="face-detect-java-example-request"></a>人脸检测 Java 示例请求

若要运行示例，请执行以下步骤：

1. 创建新的命令行应用。
1. 将 Main 类替换为以下代码（保留所有 `package` 语句）。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 根据需要更改 `uriBase` 值，以使用订阅密钥的获取位置。
1. 从 Maven 存储库将这些全局库下载到项目中的 `lib` 目录：
   - `org.apache.httpcomponents:httpclient:4.2.4`
   - `org.json:json:20170516`
1. 运行“Main”。

```java
// This sample uses the Apache HTTP client library(org.apache.httpcomponents:httpclient:4.2.4)
// and the org.json library (org.json:json:20170516).

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

    public static final String uriBase = "https://api.cognitive.azure.cn/face/v1.0/detect";

    public static void main(String[] args)
    {
        HttpClient httpclient = new DefaultHttpClient();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

#### <a name="face-detect-response"></a>人脸检测响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例： 

```json
REST Response:

[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]

Process finished with exit code 0
```

## 使用人脸 API 通过 Java 创建人员组 <a name="Create"> </a>

使用[“人员组 - 创建人员组”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)可以创建包含指定 personGroupId、名称和用户提供的 userData 的人员组。 人员组是“人脸 - 识别”API 的最重要参数之一。 “识别”API 会在指定的人员组中搜索人员的平面。

#### <a name="person-group---create-a-person-group-example"></a>“人员组 - 创建人员组”示例

更改 REST URL 以使用订阅密钥的获取位置，并将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥。

```java
// // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPut;
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
            // The valid characters for the ID below include numbers, English letters in lower case, '-', and '_'.
            // The maximum length of the personGroupId is 64.
            String personGroupId = "example-group-00";

            URIBuilder builder = new URIBuilder("https://api.cognitive.azure.cn/face/v1.0/persongroups/" +
                                                personGroupId);

            URI uri = builder.build();
            HttpPut request = new HttpPut(uri);

            // Request headers. Replace the example key with your valid subscription key.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

            // Request body. The name field is the display name you want for the group (must be under 128 characters).
            // The size limit for what you want to include in the userData field is 16KB.
            String body = "{ \"name\":\"My Group\",\"userData\":\"User-provided data attached to the person group.\" }";

            StringEntity reqEntity = new StringEntity(body);
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

