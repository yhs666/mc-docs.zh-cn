---
title: 人脸 API JavaScript 快速入门 | Microsoft Docs
description: 获取信息和代码示例，帮助自己快速开始使用认知服务中的人脸 API 和 JavaScript。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 06/21/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 04dea0958364a78a894a58ac35d6dc34c3329f16
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407592"
---
# <a name="face-api-javascript-quick-starts"></a>人脸 API JavaScript 快速入门

本文提供信息和代码示例来帮助读者快速开始使用人脸 API 和 JavaScript 来完成以下任务： 
- [检测图像中的人脸](#Detect) 
- [识别图像中的人脸](#Identify)

在[此处](../../Computer-vision/Vision-API-How-to-Topics/HowToSubscribe.md)详细了解如何获取免费订阅密钥

## 使用人脸 API 通过 JavaScript 检测图像中的人脸 <a name="Detect"> </a>

使用[“人脸 - 检测”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)可以检测图像中的人脸，并返回人脸属性，包括：
- 人脸 ID：在多种人脸 API 方案使用的唯一 ID。 
- 人脸矩形：左侧坐标、顶部坐标、宽度和高度，指示人脸在图像中的位置。
- 地标：27 点人脸地标数组，指向人脸组成部分的重要位置。
- 面部属性包括年龄、性别、笑容强度、头部姿势和面部毛发。 

#### <a name="face-detect-javascript-example-request"></a>人脸检测 JavaScript 示例请求

若要运行示例，请执行以下步骤：

1. 复制以下内容并将其保存到某个文件，例如 `detectFaces.html`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 将此文件拖放到浏览器中。
1. 单击 `Analyze faces` 按钮。

```html 
<!DOCTYPE html>
<html>
<head>
    <title>Detect Faces Sample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>
<body>

<script type="text/javascript">
    function processImage() {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        var subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

        var uriBase = "https://api.cognitive.azure.cn/face/v1.0/detect";

        // Request parameters.
        var params = {
            "returnFaceId": "true",
            "returnFaceLandmarks": "false",
            "returnFaceAttributes": "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise",
        };

        // Display the image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;

        // Perform the REST API call.
        $.ajax({
            url: uriBase + "?" + $.param(params),
            
            // Request headers.
            beforeSend: function(xhrObj){
                xhrObj.setRequestHeader("Content-Type","application/json");
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            },
            
            type: "POST",
            
            // Request body.
            data: '{"url": ' + '"' + sourceImageUrl + '"}',
        })
        
        .done(function(data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
        
        .fail(function(jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
            alert(errorString);
        });
    };
</script>

<h1>Detect Faces:</h1>
Enter the URL to an image that includes a face or faces, then click the <strong>Analyze face</strong> button.
<br><br>
Image to analyze: <input type="text" name="inputImage" id="inputImage" value="https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg" />
<button onclick="processImage()">Analyze face</button>
<br><br>
<div id="wrapper" style="width:1020px; display:table;">
    <div id="jsonOutput" style="width:600px; display:table-cell;">
        Response:
        <br><br>
        <textarea id="responseTextArea" class="UIInput" style="width:580px; height:400px;"></textarea>
    </div>
    <div id="imageDiv" style="width:420px; display:table-cell;">
        Source image:
        <br><br>
        <img id="sourceImage" width="400" />
    </div>
</div>
</body>
</html>
```

#### <a name="face-detect-response"></a>人脸检测响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例： 

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
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
      }
    }
  }
]
```

## 使用人脸 API 通过 JavaScript 识别图像中的人脸 <a name="Identify"> </a>

复制以下内容并将其保存到某个文件，例如 `test.html`。 更改 `url` 以使用订阅密钥的获取位置，将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥，并添加请求正文。 若要运行示例，请将此文件拖放到浏览器中。

使用[“人脸 - 识别”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)可以根据检测到的人脸和人员数据库（定义为人员组，需提前创建，并可在不同的时间进行编辑）识别人员

#### <a name="face-identify-javascript-example-request"></a>人脸识别 JavaScript 示例请求

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>
<body>

<script type="text/javascript">
    $(function() {
        var params = {
            // Request parameters
        };
      
        $.ajax({
            url: "https://api.cognitive.azure.cn/face/v1.0/identify?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","13hc77781f7e4b19b5fcdd72a8df7156");
            },
            type: "POST",
            // Request body
            data: "{body}",
        })
        .done(function(data) {
            alert("success");
        })
        .fail(function() {
            alert("error");
        });
    });
</script>
</body>
</html>

```

#### <a name="face-identify-response"></a>人脸识别响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例： 

```json
[
    {
        "faceId":"c5c24a82-6845-4031-9d5d-978df9175426",
        "candidates":[
            {
                "personId":"25985303-c537-4467-b41d-bdb45cd95ca1",
                "confidence":0.92
            }
        ]
    },
    {
        "faceId":"65d083d4-9447-47d1-af30-b626144bf0fb",
        "candidates":[
            {
                "personId":"2ae4935b-9659-44c3-977f-61fac20d0538",
                "confidence":0.89
            }
        ]
    }
]
```

