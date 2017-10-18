---
title: "情感 API Ruby 快速入门 | Microsoft Docs"
description: "获取信息和代码示例，帮助自己快速开始使用认知服务中的情感 API 和 Ruby。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 05/23/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: f5fc2c6398e6571727e8e97353f9b97e31f1a075
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="emotion-api-ruby-quick-start"></a>情感 API Ruby 快速入门
本文提供信息和代码示例，帮助读者快速开始使用[情感 API 识别方法](https://dev.cognitive.azure.cn/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)与 Ruby 来识别图像中一个或多个人员表达的情感。

## <a name="prerequisite"></a>先决条件
- 从 [Azure 门户](https://portal.azure.cn)获取订阅密钥

## <a name="recognize-emotions-ruby-example-request"></a>识别情感 Ruby 示例请求

更改 REST URL 以使用订阅密钥的获取位置，将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥，并将照片的 URL 添加到 `body` 变量。

```ruby
require 'net/http'

uri = URI('https://api.cognitive.azure.cn/emotion/v1.0/recognize')
uri.query = URI.encode_www_form({
})

request = Net::HTTP::Post.new(uri.request_uri)
# Request headers
request['Content-Type'] = 'application/json'
# NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '13hc77781f7e4b19b5fcdd72a8df7156'
# Request body
request.body = "{\"url\":\"http://example.com/1.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts response.body

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

