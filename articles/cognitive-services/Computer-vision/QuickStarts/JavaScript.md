---
title: "计算机视觉 API JavaScript 快速入门 | Microsoft Docs"
description: "获取信息和代码示例，帮助自己快速开始使用 JavaScript 和认知服务中的计算机视觉 API。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: computer-vision
ms.topic: article
origin.date: 06/15/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 49caa6611d927df8a936e9fff15ae48b15dac105
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="computer-vision-javascript-quick-starts"></a>计算机视觉 JavaScript 快速入门

本文提供信息和代码示例来帮助读者快速开始使用 JavaScript 和计算机视觉 API 来完成以下任务： 
- [分析图像](#AnalyzeImage) 
- [使用特定于域的模型](#DomainSpecificModel)
- [以智能方式生成缩略图](#GetThumbnail)
- [从图像中检测并提取文本](#OCR)

在[此处](../Vision-API-How-to-Topics/HowToSubscribe.md)详细了解如何获取免费订阅密钥

本文中的大多数示例使用 jQuery 1.9.0。 有关使用 JavaScript 但不使用 jQuery 的示例，请参阅示例[以智能方式生成缩略图](#GetThumbnail)。

## 使用计算机视觉 API 通过 JavaScript 分析图像 <a name="AnalyzeImage"> </a>

使用[“分析图像”方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)，可以基于图像内容提取视觉特征。 可以上传图像或指定图像 URL 并选择要返回的特征，包括：
- 与图像内容相关的标记的详细列表。 
- 完整句子中图像内容的说明。 
- 图像包含的任何人脸的坐标、性别和年龄。
- ImageType（剪贴画或线条绘图）
- 主色、强调色，或者图像是否为黑白色。
- 在此[分类](../Category-Taxonomy.md)中定义的类别。 
- 图像是否包含色情或性暗示内容。 

### <a name="analyze-an-image-javascript-example-request"></a>分析图像 JavaScript 示例请求

若要运行示例，请执行以下步骤：

1. 复制以下内容并将其保存到某个文件，例如 `analyze.html`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 将此文件拖放到浏览器中。
1. 单击 `Analyze image` 按钮。

```html
<!DOCTYPE html>
<html>
<head>
    <title>Analyze Sample</title>
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

        var uriBase = "https://api.cognitive.azure.cn/vision/v1.0/analyze";

        // Request parameters.
        var params = {
            "visualFeatures": "Categories,Description,Color",
            "details": "",
            "language": "en",
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
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            alert(errorString);
        });
    };
</script>

<h1>Analyze image:</h1>
Enter the URL to an image of a natural or artificial landmark, then click the <strong>Analyze image</strong> button.
<br><br>
Image to analyze: <input type="text" name="inputImage" id="inputImage" value="http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg" />
<button onclick="processImage()">Analyze image</button>
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

### <a name="analyze-an-image-response"></a>分析图像响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例： 

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "top",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.9165146827843689
      }
    ]
  },
  "requestId": "63d3c630-7f3d-43d7-8a97-143012fc53f4",
  "metadata": {
    "width": 1280,
    "height": 959,
    "format": "Jpeg"
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBWImg": false
  }
}
```

## 使用特定于域的模型 <a name="DomainSpecificModel"> </a>
特定于域的模型是经过训练，可在图像中识别一组特定对象的模型。 当前可用的两个特定于域的模型为 celebrities 和 landmarks。 以下示例识别图像中的地标。

### <a name="landmark-javascript-example-request"></a>地标 JavaScript 示例请求

若要运行示例，请执行以下步骤：

1. 复制以下内容并将其保存到某个文件，例如 `landmark.html`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 将此文件拖放到浏览器中。
1. 单击 `Analyze image` 按钮。

```html
<!DOCTYPE html>
<html>
<head>
    <title>Landmark Sample</title>
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

        // if you want to use the celebrities model, change "landmarks" to "celebrities" here and in
        // uriBuilder.setParameter to use the Celebrities model.
        var uriBase = "https://api.cognitive.azure.cn/vision/v1.0/models/landmarks/analyze";

        // Request parameters.
        var params = {
            "model": "landmarks", // Use "model": "celebrities" to use the Celebrities model.
        };

        // Display the image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;

        // Perform the REST API call.
        $.ajax({
            url: uriBase + "?" + $.param(params),

            // Request headers.
            beforeSend: function(xhrObj) {
                xhrObj.setRequestHeader("Content-Type", "application/json");
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
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            alert(errorString);
        });
    };
</script>

<h1>Landmark image:</h1>
Enter the URL to an image of a natural or artificial landmark, then click the <strong>Analyze image</strong> button.
<br><br>
Landscape image to analyze: <input type="text" name="inputImage" id="inputImage" value="https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg" />
<button onclick="processImage()">Analyze image</button>
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

### <a name="landmark-example-response"></a>地标示例响应
成功响应将以 JSON 格式返回。 下面是成功响应的示例：

```json
{
  "requestId": "fe0d4539-7a21-4fc6-b7af-3bb24beba390",
  "metadata": {
    "width": 2096,
    "height": 4132,
    "format": "Jpeg"
  },
  "result": {
    "landmarks": [
      {
        "name": "Space Needle",
        "confidence": 0.9998178
      }
    ]
  }
}
```

## 使用计算机视觉 API 通过 JavaScript 获取缩略图 <a name="GetThumbnail"> </a>
使用[“获取缩略图”方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fb)可以根据图像的兴趣区域 (ROI) 将该图像裁剪为所需的高度和宽度，即使纵横比不同于输入图像。 

### <a name="get-a-thumbnail-javascript-example-request"></a>获取缩略图 JavaScript 示例请求

若要运行示例，请执行以下步骤：

1. 复制以下内容并将其保存到某个文件，例如 `thumbnail.html`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 将此文件拖放到浏览器中。
1. 单击 `Generate thumbnail` 按钮。

```html
<!DOCTYPE html>
<html>
<head>
    <title>Thumbnail Sample</title>
</head>
<body>

<script type="text/javascript">
    function processImage() {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        var subscriptionKey = "13hc77781f7e4b19b5fcdd72a8df7156";

        var uriBase = "https://api.cognitive.azure.cn/vision/v1.0/generateThumbnail";
        
        // Request parameters.
        var params = "?width=100&height=150&smartCropping=true";
      
        // Display the source image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;

        // Prepare the REST API call:
        
        // Create the HTTP Request object.
        var xhr = new XMLHttpRequest();
        
        // Identify the request as a POST, with the URL and parameters.
        xhr.open("POST", uriBase + params);
        
        // Add the request headers.
        xhr.setRequestHeader("Content-Type","application/json");
        xhr.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
        
        // Set the response type to "blob" for the thumbnail image data.
        xhr.responseType = "blob";
        
        // Process the result of the REST API call.
        xhr.onreadystatechange = function(e) {
            if(xhr.readyState === XMLHttpRequest.DONE) {
                
                // Thumbnail successfully created.
                if (xhr.status === 200) {
                    // Show response headers.
                    var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                    document.getElementById("responseTextArea").value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                    
                    // Show thumbnail image.
                    var urlCreator = window.URL || window.webkitURL;
                    var imageUrl = urlCreator.createObjectURL(this.response);
                    document.querySelector("#thumbnailImage").src = imageUrl;
                } else {
                    // Display the error message. The error message is the response body as a JSON string. 
                    // The code in this code block extracts the JSON string from the blob response.
                    var reader = new FileReader();
                    
                    // This event fires after the blob has been read.
                    reader.addEventListener('loadend', (e) => {
                        document.getElementById("responseTextArea").value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                    });
                    
                    // Start reading the blob as text.
                    reader.readAsText(xhr.response);
                }
            }
        }
        
        // Execute the REST API call.
        xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
    };
</script>

<h1>Generate thumbnail image:</h1>
Enter the URL to an image to use in creating a thumbnail image, then click the <strong>Generate thumbnail</strong> button.
<br><br>
Image for thumnail: <input type="text" name="inputImage" id="inputImage" value="https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg" />
<button onclick="processImage()">Generate thumbnail</button>
<br><br>
<div id="wrapper" style="width:1160px; display:table;">
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
    <div id="thumbnailDiv" style="width:140px; display:table-cell;">
        Thumbnail:
        <br><br>
        <img id="thumbnailImage" />
    </div>
</div>
</body>
</html>
```

### <a name="get-a-thumbnail-response"></a>获取缩略图响应

成功响应包含缩略图二进制文件。 如果请求失败，则响应包含错误代码和消息，以帮助确定问题所在。

## 使用计算机视觉 API 通过 JavaScript 执行光学字符识别 (OCR) <a name="OCR"> </a>

使用[光学字符识别 (OCR) 方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc)可以检测图像中的文本，并将识别到的字符提取到机器可用的字符流中。

### <a name="ocr-javascript-example-request"></a>OCR JavaScript 示例请求

若要运行示例，请执行以下步骤：

1. 复制以下内容并将其保存到某个文件，例如 `ocr.html`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 将此文件拖放到浏览器中。
1. 单击 `Read image` 按钮。

```html
<!DOCTYPE html>
<html>
<head>
    <title>OCR Sample</title>
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

        var uriBase = "https://api.cognitive.azure.cn/vision/v1.0/ocr";

        // Request parameters.
        var params = {
            "language": "unk",
            "detectOrientation ": "true",
        };

        // Display the image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;
        
        // Perform the REST API call.
        $.ajax({
            url: uriBase + "?" + $.param(params),
            
            // Request headers.
            beforeSend: function(jqXHR){
                jqXHR.setRequestHeader("Content-Type","application/json");
                jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
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

<h1>Optical Character Recognition (OCR):</h1>
Enter the URL to an image of printed text, then click the <strong>Read image</strong> button.
<br><br>
Image to read: <input type="text" name="inputImage" id="inputImage" value="https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png" />
<button onclick="processImage()">Read image</button>
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

### <a name="ocr-example-response"></a>OCR 示例响应

成功响应将以 JSON 格式返回。 返回的 OCR 结果包括文本、区域范围框、线条和单词。

下面是成功响应的示例：

```json 
{
  "language": "en",
  "textAngle": 0,
  "orientation": "Up",
  "regions": [
    {
      "boundingBox": "21,16,304,451",
      "lines": [
        {
          "boundingBox": "28,16,288,41",
          "words": [
            {
              "boundingBox": "28,16,288,41",
              "text": "NOTHING"
            }
          ]
        },
        {
          "boundingBox": "27,66,283,52",
          "words": [
            {
              "boundingBox": "27,66,283,52",
              "text": "EXISTS"
            }
          ]
        },
        {
          "boundingBox": "27,128,292,49",
          "words": [
            {
              "boundingBox": "27,128,292,49",
              "text": "EXCEPT"
            }
          ]
        },
        {
          "boundingBox": "24,188,292,54",
          "words": [
            {
              "boundingBox": "24,188,292,54",
              "text": "ATOMS"
            }
          ]
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
          "words": [
            {
              "boundingBox": "21,298,304,60",
              "text": "SPACE."
            }
          ]
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
    }
  ]
}
```

## 使用计算机视觉 API 通过 JavaScript 执行文本识别 <a name="RecognizeText"> </a>

### <a name="handwriting-recognition-java-example"></a>手写文本识别 Java 示例

若要运行示例，请执行以下步骤：

1. 复制以下内容并将其保存到某个文件，例如 `handwriting.html`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uriBase` 值以使用订阅密钥的获取位置。
1. 将此文件拖放到浏览器中。
1. 单击 `Read image` 按钮。

```html
<!DOCTYPE html>
<html>
<head>
    <title>Handwriting Sample</title>
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

        var uriBase = "https://api.cognitive.azure.cn/vision/v1.0/RecognizeText";

        // Request parameters.
        var params = {
            "handwriting": "true",
        };

        // Display the image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;
        
        // This operation requrires two REST API calls. One to submit the image for processing,
        // the other to retrieve the text found in the image. 
        //
        // Perform the first REST API call to submit the image for processing.
        $.ajax({
            url: uriBase + "?" + $.param(params),
            
            // Request headers.
            beforeSend: function(jqXHR){
                jqXHR.setRequestHeader("Content-Type","application/json");
                jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            },
            
            type: "POST",
            
            // Request body.
            data: '{"url": ' + '"' + sourceImageUrl + '"}',
        })
        
        .done(function(data, textStatus, jqXHR) {
            // Show progress.
            $("#responseTextArea").val("Handwritten text submitted. Waiting 10 seconds to retrieve the recognized text.");
            
            // Note: The response may not be immediately available. Handwriting recognition is an
            // async operation that can take a variable amount of time depending on the length
            // of the text you want to recognize. You may need to wait or retry this GET operation.
            //
            // Wait ten seconds before making the second REST API call.
            setTimeout(function () { 
                // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
                var operationLocation = jqXHR.getResponseHeader("Operation-Location");
                
                // Perform the second REST API call and get the response.
                $.ajax({
                    url: operationLocation,
                    
                    // Request headers.
                    beforeSend: function(jqXHR){
                        jqXHR.setRequestHeader("Content-Type","application/json");
                        jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                    },
                    
                    type: "GET",
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
            }, 10000);
        })
        
        .fail(function(jqXHR, textStatus, errorThrown) {
            // Put the JSON description into the text area.
            $("#responseTextArea").val(JSON.stringify(jqXHR, null, 2));
            
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
            alert(errorString);
        });
    };
</script>
<h1>Read handwritten image image:</h1>
Enter the URL to an image of handwritten text, then click the <strong>Read image</strong> button.
<br><br>
Image to read: <input type="text" name="inputImage" id="inputImage" value="https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg" />
<button onclick="processImage()">Read image</button>
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

### <a name="handwriting-example-response"></a>手写文本示例响应

成功响应将以 JSON 格式返回。 返回的手写文本结果包括文本、区域范围框、线条和单词。

下面是成功响应的示例：

```json 
{
  "status": "Succeeded",
  "recognitionResult": {
    "lines": [
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
        "text": "Pack my box with five dozen liquor jugs",
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
        ]
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
        "text": "dog",
        "words": [
          {
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
          }
        ]
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
        "text": "The quick brown fox jumps over the lazy",
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
        ]
      }
    ]
  }
}
```

