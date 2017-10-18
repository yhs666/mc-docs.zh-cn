---
title: "计算机视觉 API Java 快速入门 | Microsoft Docs"
description: "获取信息和代码示例，帮助自己快速开始使用 Java 和认知服务中的计算机视觉 API。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: computer-vision
ms.topic: article
origin.date: 06/15/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: a28c15a4eee93e1ef71ea4c96d3d68d5e7590928
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="computer-vision-java-quick-starts"></a>计算机视觉 Java 快速入门

本文提供信息和代码示例来帮助读者快速开始使用 Java 和计算机视觉 API 来完成以下任务：
- [分析图像](#AnalyzeImage)
- [使用特定于域的模型](#DomainSpecificModel)
- [以智能方式生成缩略图](#GetThumbnail)
- [从图像中检测并提取打印的文本](#OCR)
- [从图像中检测并提取手写的文本](#RecognizeText)

## <a name="prerequisites"></a>先决条件

- 在[此处](https://github.com/Microsoft/Cognitive-vision-android)获取 Microsoft 计算机视觉 Android SDK。
- 若要使用计算机视觉 API，需要一个订阅密钥。 可在[此处](/cognitive-services/Computer-vision/Vision-API-How-to-Topics/HowToSubscribe)获取免费订阅密钥。

## 使用计算机视觉 API 通过 Java 分析图像 <a name="AnalyzeImage"> </a>

使用[“分析图像”方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)，可以基于图像内容提取视觉特征。 可以上传图像或指定图像 URL 并选择要返回的特征，包括：
- 与图像内容相关的标记的详细列表。
- 完整句子中图像内容的说明。
- 图像包含的任何人脸的坐标、性别和年龄。
- ImageType（剪贴画或线条绘图）。
- 主色、强调色，或者图像是否为黑白色。
- 在此[分类](/cognitive-services/computer-vision/category-taxonomy)中定义的类别。
- 图像是否包含成人或性暗示内容？

### <a name="analyze-an-image-java-example-request"></a>分析图像 Java 示例请求

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
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

    public static final String uriBase = "https://api.cognitive.azure.cn/vision/v1.0/analyze";

    public static void main(String[] args)
    {
        HttpClient httpclient = new DefaultHttpClient();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/1/12/Broadway_and_Times_Square_by_night.jpg\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("REST Response:\n");
                System.out.println(json.toString(2));
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

### <a name="analyze-an-image-response"></a>分析图像响应

成功响应将以 JSON 格式返回。 程序应生成如下所示的输出：

```json
REST Response:

{
  "metadata": {
    "width": 1826,
    "format": "Jpeg",
    "height": 2436
  },
  "color": {
    "dominantColorForeground": "Brown",
    "isBWImg": false,
    "accentColor": "B74314",
    "dominantColorBackground": "Brown",
    "dominantColors": ["Brown"]
  },
  "requestId": "bbffe1a1-4fa3-4a6b-a4d5-a4964c58a811",
  "description": {
    "captions": [{
      "confidence": 0.8241405091548035,
      "text": "a group of people on a city street filled with traffic at night"
    }],
    "tags": [
      "outdoor",
      "building",
      "street",
      "city",
      "busy",
      "people",
      "filled",
      "traffic",
      "many",
      "table",
      "car",
      "group",
      "walking",
      "bunch",
      "crowded",
      "large",
      "night",
      "light",
      "standing",
      "man",
      "tall",
      "umbrella",
      "riding",
      "sign",
      "crowd"
    ]
  },
  "categories": [{
    "score": 0.625,
    "name": "outdoor_street"
  }]
}

Process finished with exit code 0
```

## 使用特定于域的模型 <a name="DomainSpecificModel"> </a>

特定于域的模型是经过训练，可在图像中识别一组特定对象的模型。 当前可用的两个特定于域的模型为 celebrities 和 landmarks。 以下示例识别图像中的地标。

### <a name="landmark-java-example-request"></a>地标 Java 示例请求

若要运行示例，请执行以下步骤：

1. 创建新的命令行应用。
1. 将 Main 类替换为以下代码（保留所有 `package` 语句）。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 根据需要更改 `uriBase` 值，以使用订阅密钥的获取位置。
1. 从 Maven 存储库将这些库下载项目中的 `lib` 目录：
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
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

    // if you want to use the celebrities model, change "landmarks" to "celebrities" here and in
    // uriBuilder.setParameter to use the Celebrities model.
    public static final String uriBase = "https://api.cognitive.azure.cn/vision/v1.0/models/landmarks/analyze";

    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            URIBuilder uriBuilder = new URIBuilder(uriBase);

            // Request parameters.
            // To use the Celebrities model, change "landmarks" to "celebrities" here and in uriBase.
            uriBuilder.setParameter("model", "landmarks");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity = new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg\"}");
            request.setEntity(requestEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("REST Response:\n");
                System.out.println(json.toString(2));
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

### <a name="landmark-example-response"></a>地标示例响应

成功响应将以 JSON 格式返回。 程序应生成如下所示的输出：  

```json
REST Response:

{
  "result": {"landmarks": [{
    "confidence": 0.9998178,
    "name": "Space Needle"
  }]},
  "metadata": {
    "width": 2096,
    "format": "Jpeg",
    "height": 4132
  },
  "requestId": "8551c2b7-fcf9-4932-aff3-87e7f744343f"
}

Process finished with exit code 0
```

## 使用计算机视觉 API 通过 Java 获取缩略图 <a name="GetThumbnail"> </a>

使用[“获取缩略图”方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fb)可以根据图像的兴趣区域 (ROI) 将该图像裁剪为所需的高度和宽度。 为缩略图设置的纵横比可以不同于输入图像的纵横比。

### <a name="get-a-thumbnail-java-example-request"></a>获取缩略图 Java 示例请求

若要运行示例，请执行以下步骤：

1. 创建新的命令行应用。
1. 将 Main 类替换为以下代码（保留所有 `package` 语句）。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 根据需要更改 `uriBase` 值，以使用订阅密钥的获取位置。
1. 从 Maven 存储库将这些库下载项目中的 `lib` 目录：
   - `org.apache.httpcomponents:httpclient:4.2.4`
   - `org.json:json:20170516`
1. 运行“Main”。

```java
// This sample uses the Apache HTTP client library(org.apache.httpcomponents:httpclient:4.2.4)
// and the org.json library (org.json:json:20170516).

import java.awt.*;
import javax.swing.*;
import java.net.URI;
import java.io.InputStream;
import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

    public static final String uriBase = "https://api.cognitive.azure.cn/vision/v1.0/generateThumbnail";


    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            URIBuilder uriBuilder = new URIBuilder(uriBase);

            // Request parameters.
            uriBuilder.setParameter("width", "100");
            uriBuilder.setParameter("height", "150");
            uriBuilder.setParameter("smartCropping", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity = new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg\"}");
            request.setEntity(requestEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            // Check for success.
            if (response.getStatusLine().getStatusCode() == 200)
            {
                // Display the thumbnail.
                System.out.println("\nDisplaying thumbnail.\n");
                displayImage(entity.getContent());
            }
            else
            {
                // Format and display the JSON error message.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
            }
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
        }
    }

    // Displays the given input stream as an image.
    private static void displayImage(InputStream inputStream)
    {
        try {
            BufferedImage bufferedImage = ImageIO.read(inputStream);

            ImageIcon imageIcon = new ImageIcon(bufferedImage);

            JLabel jLabel = new JLabel();
            jLabel.setIcon(imageIcon);

            JFrame jFrame = new JFrame();
            jFrame.setLayout(new FlowLayout());
            jFrame.setSize(100, 150);

            jFrame.add(jLabel);
            jFrame.setVisible(true);
            jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        }
        catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

### <a name="get-a-thumbnail-response"></a>获取缩略图响应

成功响应包含缩略图二进制文件。 如果请求失败，则响应包含错误代码和消息，以帮助确定问题所在。

## 使用计算机视觉 API 通过 Java 执行光学字符识别 (OCR) <a name="OCR"> </a>

使用[光学字符识别 (OCR) 方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc)可以检测图像中的打印文本，并将识别到的字符提取到机器可用的字符流中。

### <a name="ocr-java-example-request"></a>OCR Java 示例请求

若要运行示例，请执行以下步骤：

1. 创建新的命令行应用。
1. 将 Main 类替换为以下代码（保留所有 `package` 语句）。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 根据需要更改 `uriBase` 值，以使用订阅密钥的获取位置。
1. 从 Maven 存储库将这些库下载项目中的 `lib` 目录：
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
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

    // if you want to use the celebrities model, change "landmarks" to "celebrities" here and in
    // uriBuilder.setParameter to use the Celebrities model.
    public static final String uriBase = "https://api.cognitive.azure.cn/vision/v1.0/ocr";


    public static void main(String[] args)
    {
        HttpClient httpClient = new DefaultHttpClient();

        try
        {
            URIBuilder uriBuilder = new URIBuilder(uriBase);

            uriBuilder.setParameter("language", "unk");
            uriBuilder.setParameter("detectOrientation ", "true");

            // Request parameters.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png\"}");
            request.setEntity(requestEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("REST Response:\n");
                System.out.println(json.toString(2));
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

### <a name="ocr-example-response"></a>OCR 示例响应

成功响应将以 JSON 格式返回。 OCR 结果包括检测到的文本、区域范围框、线条和单词。

程序应生成如下所示的输出：

```json
REST Response:

{
  "orientation": "Up",
  "regions": [{
    "boundingBox": "21,16,304,451",
    "lines": [
      {
        "boundingBox": "28,16,288,41",
        "words": [{
          "boundingBox": "28,16,288,41",
          "text": "NOTHING"
        }]
      },
      {
        "boundingBox": "27,66,283,52",
        "words": [{
          "boundingBox": "27,66,283,52",
          "text": "EXISTS"
        }]
      },
      {
        "boundingBox": "27,128,292,49",
        "words": [{
          "boundingBox": "27,128,292,49",
          "text": "EXCEPT"
        }]
      },
      {
        "boundingBox": "24,188,292,54",
        "words": [{
          "boundingBox": "24,188,292,54",
          "text": "ATOMS"
        }]
      },
      {
        "boundingBox": "22,253,297,32",
        "words": [
          {
            "boundingBox": "22,253,105,32",
            "text": "AND"
          },
          {
            "boundingBox": "144,253,175,32",
            "text": "EMPTY"
          }
        ]
      },
      {
        "boundingBox": "21,298,304,60",
        "words": [{
          "boundingBox": "21,298,304,60",
          "text": "SPACE."
        }]
      },
      {
        "boundingBox": "26,387,294,37",
        "words": [
          {
            "boundingBox": "26,387,210,37",
            "text": "Everything"
          },
          {
            "boundingBox": "249,389,71,27",
            "text": "else"
          }
        ]
      },
      {
        "boundingBox": "127,431,198,36",
        "words": [
          {
            "boundingBox": "127,431,31,29",
            "text": "is"
          },
          {
            "boundingBox": "172,431,153,36",
            "text": "opinion."
          }
        ]
      }
    ]
  }],
  "textAngle": 0,
  "language": "en"
}

Process finished with exit code 0
```

## 使用计算机视觉 API 通过 Java 执行文本识别 <a name="RecognizeText"> </a>

### <a name="handwriting-recognition-java-example"></a>手写文本识别 Java 示例

若要运行示例，请执行以下步骤：

1. 创建新的命令行应用。
1. 将 Main 类替换为以下代码（保留所有 `package` 语句）。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 根据需要更改 `uriBase` 值，以使用订阅密钥的获取位置。
1. 从 Maven 存储库将这些库下载项目中的 `lib` 目录：
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
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.apache.http.Header;
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

    // For printed text, set "handwriting" to false.
    public static final String uriBase = "https://api.cognitive.azure.cn/vision/v1.0/recognizeText?handwriting=true";

    public static void main(String[] args)
    {
        HttpClient textClient = new DefaultHttpClient();
        HttpClient resultClient = new DefaultHttpClient();

        try
        {
            // This operation requrires two REST API calls. One to submit the image for processing,
            // the other to retrieve the text found in the image.
            //
            // Begin the REST API call to submit the image for processing.
            URI uri = new URI(uriBase);
            HttpPost textRequest = new HttpPost(uri);

            // Request headers. Another valid content type is "application/octet-stream".
            textRequest.setHeader("Content-Type", "application/json");
            textRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg\"}");
            textRequest.setEntity(requestEntity);

            // Execute the first REST API call to detect the text.
            HttpResponse textResponse = textClient.execute(textRequest);

            // Check for success.
            if (textResponse.getStatusLine().getStatusCode() != 202)
            {
                // Format and display the JSON error message.
                HttpEntity entity = textResponse.getEntity();
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
                return;
            }

            String operationLocation = null;

            // The 'Operation-Location' in the response contains the URI to retrieve the recognized text.
            Header[] responseHeaders = textResponse.getAllHeaders();
            for(Header header : responseHeaders) {
                if(header.getName().equals("Operation-Location"))
                {
                    // This string is the URI where you can get the text recognition operation result.
                    operationLocation = header.getValue();
                    break;
                }
            }

            // NOTE: The response may not be immediately available. Handwriting recognition is an
            // async operation that can take a variable amount of time depending on the length
            // of the text you want to recognize. You may need to wait or retry this operation.

            System.out.println("\nHandwritten text submitted. Waiting 10 seconds to retrieve the recognized text.\n");
            Thread.sleep(10000);

            // Execute the second REST API call and get the response.
            HttpGet resultRequest = new HttpGet(operationLocation);
            resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            HttpResponse resultResponse = resultClient.execute(resultRequest);
            HttpEntity responseEntity = resultResponse.getEntity();

            if (responseEntity != null)
            {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(responseEntity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Text recognition result response: \n");
                System.out.println(json.toString(2));
            }
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
```

### <a name="handwriting-recognition-java-example-response"></a>手写文本识别 Java 示例响应

成功响应将以 JSON 格式返回。 手写文本识别结果包括检测到的文本、区域范围框、线条和单词。

程序应生成如下所示的输出：

```json
Handwritten text submitted. Waiting 10 seconds to retrieve the recognized text.

Text recognition result response: 

{
  "status": "Succeeded",
  "recognitionResult": {"lines": [
    {
      "boundingBox": [
        2,
        84,
        783,
        96,
        782,
        154,
        1,
        148
      ],
      "words": [
        {
          "boundingBox": [
            6,
            86,
            92,
            87,
            71,
            151,
            0,
            150
          ],
          "text": "Pack"
        },
        {
          "boundingBox": [
            86,
            87,
            172,
            88,
            150,
            152,
            64,
            151
          ],
          "text": "my"
        },
        {
          "boundingBox": [
            165,
            88,
            241,
            89,
            219,
            152,
            144,
            152
          ],
          "text": "box"
        },
        {
          "boundingBox": [
            234,
            89,
            343,
            90,
            322,
            154,
            213,
            152
          ],
          "text": "with"
        },
        {
          "boundingBox": [
            347,
            90,
            432,
            91,
            411,
            154,
            325,
            154
          ],
          "text": "five"
        },
        {
          "boundingBox": [
            432,
            91,
            538,
            92,
            516,
            154,
            411,
            154
          ],
          "text": "dozen"
        },
        {
          "boundingBox": [
            554,
            92,
            696,
            94,
            675,
            154,
            533,
            154
          ],
          "text": "liquor"
        },
        {
          "boundingBox": [
            710,
            94,
            800,
            96,
            800,
            154,
            688,
            154
          ],
          "text": "jugs"
        }
      ],
      "text": "Pack my box with five dozen liquor jugs"
    },
    {
      "boundingBox": [
        2,
        52,
        65,
        46,
        69,
        89,
        7,
        95
      ],
      "words": [{
        "boundingBox": [
          0,
          62,
          79,
          39,
          94,
          82,
          0,
          105
        ],
        "text": "dog"
      }],
      "text": "dog"
    },
    {
      "boundingBox": [
        6,
        2,
        771,
        13,
        770,
        75,
        5,
        64
      ],
      "words": [
        {
          "boundingBox": [
            8,
            4,
            92,
            5,
            77,
            71,
            0,
            71
          ],
          "text": "The"
        },
        {
          "boundingBox": [
            89,
            5,
            188,
            5,
            173,
            72,
            74,
            71
          ],
          "text": "quick"
        },
        {
          "boundingBox": [
            188,
            5,
            323,
            6,
            308,
            73,
            173,
            72
          ],
          "text": "brown"
        },
        {
          "boundingBox": [
            316,
            6,
            386,
            6,
            371,
            73,
            302,
            73
          ],
          "text": "fox"
        },
        {
          "boundingBox": [
            396,
            7,
            508,
            7,
            493,
            74,
            381,
            73
          ],
          "text": "jumps"
        },
        {
          "boundingBox": [
            501,
            7,
            604,
            8,
            589,
            75,
            487,
            74
          ],
          "text": "over"
        },
        {
          "boundingBox": [
            600,
            8,
            673,
            8,
            658,
            75,
            586,
            75
          ],
          "text": "the"
        },
        {
          "boundingBox": [
            670,
            8,
            800,
            9,
            787,
            76,
            655,
            75
          ],
          "text": "lazy"
        }
      ],
      "text": "The quick brown fox jumps over the lazy"
    }
  ]}
}

Process finished with exit code 0
```

