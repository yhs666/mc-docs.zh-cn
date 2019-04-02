---
title: 教程：执行图像操作 - JavaScript
titlesuffix: Azure Cognitive Services
description: 介绍一款使用 Azure 认知服务计算机视觉 API 的基本 JavaScript 应用。 执行 OCR，创建缩略图，并处理图像中的视觉特征。
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
origin.date: 09/19/2017
ms.date: 03/27/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: 5beb8e91e632335462fa25b8d2e492919886c02f
ms.sourcegitcommit: c5599eb7dfe9fd5fe725b82a861c97605635a73f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505483"
---
# <a name="tutorial-computer-vision-api-javascript"></a>教程：计算机视觉 API JavaScript

本教程展示 Azure 认知服务计算机视觉 REST API 的功能。

探讨一个使用计算机视觉 REST API 执行光学字符识别 (OCR)、创建智能裁剪的缩略图，以及在图像中检测、标记和描述视觉特征（包括人脸）并对其分类的 JavaScript 应用程序。 此示例允许提交图像 URL 来分析或处理图像。 可以使用此开源示例作为模板，生成自己的 JavaScript 应用，以便使用计算机视觉 REST API。

JavaScript 形式的应用程序已编写好，但尚无计算机视觉功能。 在本教程中，请添加特定于计算机视觉 REST API 的代码，以便完成应用程序的功能。

## <a name="prerequisites"></a>先决条件

### <a name="platform-requirements"></a>平台要求

本教程的内容已使用简单的文本编辑器开发。

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>订阅计算机视觉 API 并获取订阅密钥 

创建示例之前，必须在 [Azure 门户](https://portal.azure.cn)中创建带计算机视觉 API 的认知服务， 然后即可获取订阅密钥。 主密钥和辅助密钥均适用于本教程。 

## <a name="acquire-the-incomplete-tutorial-project"></a>获取不完整的教程项目

### <a name="download-the-tutorial-project"></a>下载教程项目

克隆 [Cognitive Services JavaScript Computer Vision Tutorial](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial)（认知服务 JavaScript 计算机视觉教程），或下载 .zip 文件并将其解压缩到空目录。

若要使用已完成的的教程（添加了所有教程代码），可以使用 Completed 文件夹中的文件。

## <a name="add-the-tutorial-code-to-the-project"></a>向项目添加教程代码

JavaScript 应用程序使用六个 .html 文件进行设置，每个文件对应于一个功能。 每个文件演示的计算机视觉功能（分析、OCR 等）各不相同。 教程的六个部分并不互相依赖，因此可以将教程代码添加到一个文件、所有六个文件或者部分文件。 可以按任意顺序将教程代码添加到文件中。

让我们开始吧。

### <a name="analyze-an-image"></a>分析图像

计算机视觉的分析功能可扫描图像中超过 2,000 个可识别的对象、生物、风景和动作。 分析完成以后，分析功能会返回一个 JSON 对象，用描述性标记、颜色分析、标题等对图像进行描述。

若要完成教程应用程序的分析功能，请执行以下步骤：

#### <a name="add-the-event-handler-code-for-the-form-button"></a>为表单按钮添加事件处理程序代码

在文本编辑器中打开 analyze.html 文件，找到文件底部的 analyzeButtonClick 函数。

analyzeButtonClick 事件处理程序函数会清除窗体，显示在 URL 中指定的图像，然后调用 AnalyzeImage 函数进行图像分析。

将以下代码复制并粘贴到 analyzeButtonClick 函数中。

```javascript
function analyzeButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    AnalyzeImage(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>添加用于 REST API 调用的包装器

AnalyzeImage 函数包装进行图像分析的 REST API 调用。 成功返回后，指定的文本区域将显示格式化的 JSON 分析结果，且指定的范围中将显示题注。

复制 AnalyzeImage 函数代码并将其粘贴到 analyzeButtonClick 函数下方。

```javascript
/* Analyze the image at the specified URL by using Microsoft Cognitive Services Analyze Image API.
 * @param {string} sourceImageUrl - The URL to the image to analyze.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function AnalyzeImage(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "visualFeatures": "Categories,Description,Color",
        "details": "",
        "language": "en",
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseAnalyze +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.description && data.description.captions) {
            var caption = data.description.captions[0];
            
            if (caption.text && caption.confidence) {
                captionSpan.text("Caption: " + caption.text +
                    " (confidence: " + caption.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>运行应用程序

保存 analyze.html 文件，然后在 Web 浏览器中将其打开。 在“订阅密钥”字段中填写订阅密钥，并验证确保“订阅区域中”使用的区域正确无误。 输入图像 URL 进行分析，然后单击“分析图像”按钮以分析图像并查看结果。

### <a name="recognize-a-landmark"></a>识别地标

计算机视觉的地标功能可以分析图像中是否存在自然的和人工的地标，例如山脉或著名建筑。 分析完以后，地标功能会返回一个 JSON 对象，其中标识了图像中发现的地标。

若要完成教程应用程序的地标功能，请执行以下步骤：

#### <a name="add-the-event-handler-code-for-the-form-button"></a>为表单按钮添加事件处理程序代码

在文本编辑器中打开 landmark.html 文件，找到文件底部的 landmarkButtonClick 函数。

landmarkButtonClick 事件处理程序函数会清除窗体，显示在 URL 中指定的图像，然后调用 IdentifyLandmarks 函数进行图像分析。

将以下代码复制并粘贴到 landmarkButtonClick 函数中。

```javascript
function landmarkButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyLandmarks(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>添加用于 REST API 调用的包装器

IdentifyLandmarks 函数包装进行图像分析的 REST API 调用。 成功返回以后，会在指定的文本区域显示格式化的 JSON 分析，并会按指定的时间间隔显示标题。

复制 IdentifyLandmarks 函数代码并将其粘贴到 landmarkButtonClick 函数下方。

```javascript
/* Identify landmarks in the image at the specified URL by using Microsoft Cognitive Services 
 * Landmarks API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for landmarks.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyLandmarks(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "landmarks"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseLandmark +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.landmarks) {
            var landmark = data.result.landmarks[0];
            
            if (landmark.name && landmark.confidence) {
                captionSpan.text("Landmark: " + landmark.name +
                    " (confidence: " + landmark.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>运行应用程序

保存 landmark.html 文件，然后在 Web 浏览器中将其打开。 在“订阅密钥”字段中填写订阅密钥，并验证确保“订阅区域中”使用的区域正确无误。 输入要分析的图像的 URL，然后单击“分析图像”按钮对图像进行分析并查看结果。

### <a name="recognize-celebrities"></a>识别名人

计算机视觉的名人功能分析图像中是否存在名人。 分析完以后，名人功能会返回一个 JSON 对象，其中标识了图像中发现的名人。

若要完成教程应用程序的名人功能，请执行以下步骤：

#### <a name="add-the-event-handler-code-for-the-form-button"></a>为表单按钮添加事件处理程序代码

在文本编辑器中打开 celebrities.html 文件，找到文件底部的 celebritiesButtonClick 函数。

celebritiesButtonClick 事件处理程序函数会清除窗体，显示在 URL 中指定的图像，然后调用 IdentifyCelebrities 函数进行图像分析。

将以下代码复制并粘贴到 celebritiesButtonClick 函数中。

```javascript
function celebritiesButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyCelebrities(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>添加用于 REST API 调用的包装器

```javascript
/* Identify celebrities in the image at the specified URL by using Microsoft Cognitive Services 
 * Celebrities API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for celebrities.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyCelebrities(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "celebrities"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseCelebrities +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.celebrities) {
            var celebrity = data.result.celebrities[0];
            
            if (celebrity.name && celebrity.confidence) {
                captionSpan.text("Celebrity name: " + celebrity.name +
                    " (confidence: " + celebrity.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>运行应用程序

保存 celebrities.html 文件，然后在 Web 浏览器中将其打开。 在“订阅密钥”字段中填写订阅密钥，并验证确保“订阅区域中”使用的区域正确无误。 输入图像 URL 进行分析，然后单击“分析”按钮以分析图像并查看结果。

### <a name="intelligently-generate-a-thumbnail"></a>智能生成缩略图

计算机视觉的缩略图功能根据图像生成缩略图。 缩略图功能可以使用智能裁剪功能来标识图像中的感兴趣区域，使缩略图聚焦在该区域，目的是生成更美的缩略图。

若要完成教程应用程序的缩略图功能，请执行以下步骤：

#### <a name="add-the-event-handler-code-for-the-form-button"></a>为表单按钮添加事件处理程序代码

在文本编辑器中打开 thumbnail.html 文件，找到文件底部的 thumbnailButtonClick 函数。

thumbnailButtonClick 事件处理程序函数会清除窗体，显示在 URL 中指定的图像，然后通过调用 getThumbnail 函数两次来创建两个缩略图，一个经过智能裁剪，另一个没有智能裁剪。

将以下代码复制并粘贴到 thumbnailButtonClick 函数中。

```javascript
function thumbnailButtonClick() {

    // Clear the display fields.
    document.getElementById("sourceImage").src = "#";
    document.getElementById("thumbnailImageSmartCrop").src = "#";
    document.getElementById("thumbnailImageNonSmartCrop").src = "#";
    document.getElementById("responseTextArea").value = "";
    document.getElementById("captionSpan").text = "";
    
    // Display the image.
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.getElementById("sourceImage").src = sourceImageUrl;
    
    // Get a smart cropped thumbnail.
    getThumbnail (sourceImageUrl, true, document.getElementById("thumbnailImageSmartCrop"), 
        document.getElementById("responseTextArea"));
    
    // Get a non-smart-cropped thumbnail.
    getThumbnail (sourceImageUrl, false, document.getElementById("thumbnailImageNonSmartCrop"),
        document.getElementById("responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>添加用于 REST API 调用的包装器

getThumbnail 函数包装进行图像分析的 REST API 调用。 成功返回后，缩略图会显示在指定的 img 元素中。

复制以下 getThumbnail 函数并将其粘贴到 thumbnailButtonClick 函数下方。

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumbnail image.
 * @param {<textarea> element} responseTextArea - The text area to display the Response Headers returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function getThumbnail (sourceImageUrl, smartCropping, imageElement, responseTextArea) {
    // Create the HTTP Request object.
    var xhr = new XMLHttpRequest();
    
    // Request parameters.
    var params = "width=100&height=150&smartCropping=" + smartCropping.toString();

    // Build the full URI.
    var fullUri = common.uriBasePreRegion + 
                  document.getElementById("subscriptionRegionSelect").value + 
                  common.uriBasePostRegion + 
                  common.uriBaseThumbnail +
                  "?" + 
                  params;
    
    // Identify the request as a POST, with the URI and parameters.
    xhr.open("POST", fullUri);
    
    // Add the request headers.
    xhr.setRequestHeader("Content-Type","application/json");
    xhr.setRequestHeader("Ocp-Apim-Subscription-Key", 
        encodeURIComponent(document.getElementById("subscriptionKeyInput").value));
    
    // Set the response type to "blob" for the thumbnail image data.
    xhr.responseType = "blob";
    
    // Process the result of the REST API call.
    xhr.onreadystatechange = function(e) {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            
            // Thumbnail successfully created.
            if (xhr.status === 200) {
                // Show response headers.
                var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                responseTextArea.value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                
                // Show thumbnail image.
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                imageElement.src = imageUrl;
            } else {
                // Display the error message. The error message is the response body as a JSON string. 
                // The code in this code block extracts the JSON string from the blob response.
                var reader = new FileReader();
                
                // This event fires after the blob has been read.
                reader.addEventListener('loadend', (e) => {
                    responseTextArea.value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                });
                
                // Start reading the blob as text.
                reader.readAsText(xhr.response);
            }
        }
    }
    
    // Execute the REST API call.
    xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
}
```

#### <a name="run-the-application"></a>运行应用程序

保存 thumbnail.html 文件，然后在 Web 浏览器中将其打开。 将订阅密钥置于“订阅密钥”字段中，然后在“订阅区域”中验证是否使用了正确的区域。 输入要分析的图像的 URL，然后单击“生成缩略图”按钮对图像进行分析并查看结果。

### <a name="read-printed-text-ocr"></a>读取打印文本 (OCR)

计算机视觉的光学字符识别 (OCR) 功能分析图像中是否有印刷体文本。 分析完以后，OCR 会返回一个 JSON 对象，其中包含图像中的文本和文本位置。

若要完成教程应用程序的 OCR 功能，请执行以下步骤：

### <a name="ocr-step-1-add-the-event-handler-code-for-the-form-button"></a>OCR 步骤 1：为表单按钮添加事件处理程序代码

在文本编辑器中打开 ocr.html 文件，找到文件底部的 ocrButtonClick 函数。

ocrButtonClick 事件处理程序函数会清除窗体，显示在 URL 中指定的图像，然后调用 ReadOcrImage 函数进行图像分析。

将以下代码复制并粘贴到 ocrButtonClick 函数中。

```javascript
function ocrButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadOcrImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>添加用于 REST API 调用的包装器

ReadOcrImage 函数包装进行图像分析的 REST API 调用。 成功返回以后，会在指定的文本区域显示格式化的 JSON，用于描述文本和文本位置。

复制以下 ReadOcrImage 函数并将其粘贴到 ocrButtonClick 函数下方。

```javascript
/* Recognize and read printed text in an image at the specified URL by using Microsoft Cognitive 
 * Services OCR API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for printed text.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadOcrImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "language": "unk",
        "detectOrientation ": "true",
    };

    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseOcr +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>运行应用程序

保存 ocr.html 文件，然后在 Web 浏览器中将其打开。 在“订阅密钥”字段中填写订阅密钥，并验证确保“订阅区域中”使用的区域正确无误。 输入要读取的文本图像的 URL，然后单击“读取”按钮以分析图像并查看结果。

### <a name="read-handwritten-text-handwriting-recognition"></a>读取手写文本（手写识别）

计算机视觉的手写识别功能分析图像中是否有手写文本。 分析完以后，手写识别功能会返回一个 JSON 对象，其中包含图像中的文本和文本位置。

若要完成教程应用程序的手写识别功能，请执行以下步骤：

#### <a name="add-the-event-handler-code-for-the-form-button"></a>为表单按钮添加事件处理程序代码

在文本编辑器中打开 handwriting.html 文件，找到文件底部的 handwritingButtonClick 函数。

handwritingButtonClick 事件处理程序函数会清除窗体，显示在 URL 中指定的图像，然后调用 HandwritingImage 函数进行图像分析。

将以下代码复制并粘贴到 handwritingButtonClick 函数中。

```javascript
function handwritingButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadHandwrittenImage(sourceImageUrl, $("#responseTextArea"));
}
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>添加用于 REST API 调用的包装器

ReadHandwrittenImage 函数包装两个进行图像分析所需的 REST API 调用。 由于手写识别很耗时，因此使用一个两步过程。 第一个调用提交图像进行分析；第二个调用检索完成处理时检测到的文本。

检索文本以后，会在指定的文本区域显示格式化的 JSON，用于描述文本和文本位置。

复制以下 ReadHandwrittenImage 函数并将其粘贴到 handwritingButtonClick 函数下方。

```javascript
/* Recognize and read text from an image of handwriting at the specified URL by using Microsoft 
 * Cognitive Services Recognize Handwritten Text API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for handwriting.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadHandwrittenImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "handwriting": "true",
    };

    // This operation requires two REST API calls. One to submit the image for processing,
    // the other to retrieve the text found in the image. 
    //
    // Perform the first REST API call to submit the image for processing.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseHandwriting +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data, textStatus, jqXHR) {
        // Show progress.
        responseTextArea.val("Handwritten image submitted.");
        
        // Note: The response may not be immediately available. Handwriting Recognition is an
        // async operation that can take a variable amount of time depending on the length
        // of the text you want to recognize. You may need to wait or retry this GET operation.
        //
        // Try once per second for up to ten seconds to receive the result.
        var tries = 10;
        var waitTime = 100;
        var taskCompleted = false;
        
        var timeoutID = setInterval(function () { 
            // Limit the number of calls.
            if (--tries <= 0) {
                window.clearTimeout(timeoutID);
                responseTextArea.val("The response was not available in the time allowed.");
                return;
            }

            // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
            var operationLocation = jqXHR.getResponseHeader("Operation-Location");
            
            // Perform the second REST API call and get the response.
            $.ajax({
                url: operationLocation,
                
                // Request headers.
                beforeSend: function(jqXHR){
                    jqXHR.setRequestHeader("Content-Type","application/json");
                    jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key",
                        encodeURIComponent($("#subscriptionKeyInput").val()));
                },
                
                type: "GET",
            })
            
            .done(function(data) {
                // If the result is not yet available, return.
                if (data.status && (data.status === "NotStarted" || data.status === "Running")) {
                    return;
                }
                
                // Show formatted JSON on webpage.
                responseTextArea.val(JSON.stringify(data, null, 2));
                
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
            })
            
            .fail(function(jqXHR, textStatus, errorThrown) {
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
                
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                    jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
                alert(errorString);
            });
        }, waitTime);
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

#### <a name="run-the-application"></a>运行应用程序

保存 handwriting.html 文件，然后在 Web 浏览器中将其打开。 在“订阅密钥”字段中填写订阅密钥，并验证确保“订阅区域中”使用的区域正确无误。 输入要读取其文本的图像的 URL，然后单击“读取图像”按钮对图像进行分析并查看结果。

## <a name="next-steps"></a>后续步骤

- [计算机视觉 API C&#35; 教程](CSharpTutorial.md)
- [计算机视觉 API Python 教程](PythonTutorial.md)

<!-- Update_Description: wording update -->
