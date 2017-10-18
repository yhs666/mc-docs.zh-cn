---
title: "情感 API PHP 快速入门 | Microsoft Docs"
description: "获取信息和代码示例，帮助自己快速开始使用认知服务中的情感 API 和 PHP。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 05/23/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 040abf7a8d126c42f585108a136909e7c045eb50
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="emotion-api-php-quick-start"></a>情感 API PHP 快速入门
本文提供信息和代码示例，帮助读者快速开始使用 PHP 和[情感 API 识别方法](https://dev.cognitive.azure.cn/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)来识别图像中一个或多个人员表达的情感。 

## <a name="prerequisite"></a>先决条件
- 从 [Azure 门户](https://portal.azure.cn)获取订阅密钥

## <a name="recognize-emotions-php-example-request"></a>识别情感 PHP 示例请求

更改 REST URL 以使用订阅密钥的获取位置，将正文更改为想要测试的图像的 URL，并将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥。

```php
<?php
// This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

$request = new Http_Request2('https://api.cognitive.azure.cn/emotion/v1.0/recognize');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',

    // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '13hc77781f7e4b19b5fcdd72a8df7156',
);

$request->setHeader($headers);

$parameters = array(
    // Request parameters
);

$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body
$request->setBody('{"url": "http://www.example.com/images/image.jpg"}');

try
{
    $response = $request->send();
    echo $response->getBody();
}
catch (HttpException $ex)
{
    echo $ex;
}

?>
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


