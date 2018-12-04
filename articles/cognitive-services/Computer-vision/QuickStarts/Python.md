---
title: 计算机视觉 API Python 快速入门 | Microsoft Docs
description: 获取信息和代码示例，帮助自己快速开始使用 Python 和 Microsoft 认知服务中的计算机视觉 API。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: computer-vision
ms.topic: article
origin.date: 06/12/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 9a3298787c603158af853163b13c789f7888ad50
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647879"
---
# <a name="computer-vision-python-quick-starts"></a>计算机视觉 Python 快速入门

本文提供信息和代码示例来帮助读者快速开始使用计算机视觉 API 和 Python 来完成以下任务：
- [分析图像](#AnalyzeImage)
- [使用特定于域的模型](#DomainSpecificModel)
- [以智能方式生成缩略图](#GetThumbnail)
- [从图像中检测并提取打印的文本](#OCR)
- [从图像中检测并提取手写的文本](#RecognizeText)

若要使用计算机视觉 API，需要一个订阅密钥。 可在[此处](/cognitive-services/Computer-vision/Vision-API-How-to-Topics/HowToSubscribe)获取免费订阅密钥。

## 使用计算机视觉 API 通过 Python 分析图像 <a name="AnalyzeImage"> </a>

使用[“分析图像”方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)，可以基于图像内容提取视觉特征。 可以上传图像或指定图像 URL 并选择要返回的特征，包括：
- 与图像内容相关的标记的详细列表。
- 完整句子中图像内容的说明。
- 图像包含的任何人脸的坐标、性别和年龄。
- ImageType（剪贴画或线条绘图）。
- 主色、主题色或图像是否为黑白。
- 此[分类](../Category-Taxonomy.md)中定义的类别。
- 图像是否包含成人或性暗示内容？

### <a name="analyze-an-image-python-example-request"></a>分析图像 Python 示例请求

复制 Python 版本的相应部分，并将其保存到某个文件，例如 `analyze.py`。 将 `subscription_key` 值替换为有效的订阅密钥，并更改 `uri_base` 以使用订阅密钥的获取位置。 然后执行脚本。

```python
########### Python 2.7 #############
import httplib, urllib, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.urlencode({
    # Request parameters. All of them are optional.
    'visualFeatures': 'Categories,Description,Color',
    'language': 'en',
})

# The URL of a JPEG image to analyze.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/1/12/Broadway_and_Times_Square_by_night.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = httplib.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/vision/v1.0/analyze?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################

########### Python 3.6 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.parse.urlencode({
    # Request parameters. All of them are optional.
    'visualFeatures': 'Categories,Description,Color',
    'language': 'en',
})

# Replace the three dots below with the URL of a JPEG image of a celebrity.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/1/12/Broadway_and_Times_Square_by_night.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = http.client.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/vision/v1.0/analyze?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################
```

### <a name="analyze-an-image-response"></a>分析图像响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例：

```json
Response:
{
  "categories": [
    {
      "name": "outdoor_street",
      "score": 0.625
    }
  ],
  "color": {
    "accentColor": "B74314",
    "dominantColorBackground": "Brown",
    "dominantColorForeground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "isBWImg": false
  },
  "description": {
    "captions": [
      {
        "confidence": 0.8241403656347864,
        "text": "a group of people on a city street filled with traffic at night"
      }
    ],
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
  "metadata": {
    "format": "Jpeg",
    "height": 2436,
    "width": 1826
  },
  "requestId": "92525ac4-fda0-47c3-876c-6457851fdb08"
}
```

## 使用特定于域的模型 <a name="DomainSpecificModel"> </a>

特定于域的模型是经过训练，可在图像中识别一组特定对象的模型。 当前可用的两个特定于域的模型为 celebrities 和 landmarks。 以下示例识别图像中的地标。

### <a name="landmark-python-example-request"></a>地标 Python 示例请求

复制 Python 版本的相应部分，并将其保存到某个文件，例如 `landmark.py`。 将 `subscription_key` 值替换为有效的订阅密钥，并更改 `uri_base` 以使用订阅密钥的获取位置。 然后执行脚本。

```python
########### Python 2.7 #############
import httplib, urllib, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.urlencode({
    # Request parameters. Use 'model': 'celebrities' to use the Celebrities model.
    'model': 'landmarks',
})

# The URL of a JPEG image containing a landmark.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = httplib.HTTPSConnection(uri_base)

    # Change "landmarks" to "celebrities" in the url to use the Celebrities model.
    conn.request("POST", "/vision/v1.0/models/landmarks/analyze?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################

########### Python 3.6 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.parse.urlencode({
    # Request parameters. Use "model": "celebrities" to use the Celebrity model.
    'model': 'landmarks',
})

# The URL of a JEPG image containing text.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = http.client.HTTPSConnection(uri_base)
    conn.request("POST", "/vision/v1.0/models/landmarks/analyze?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################
```

### <a name="landmark-example-response"></a>地标示例响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例：  

```json
{
  "metadata": {
    "format": "Jpeg",
    "height": 4132,
    "width": 2096
  },
  "requestId": "d08a914a-0fbb-4695-9a2e-c93791865436",
  "result": {
    "landmarks": [
      {
        "confidence": 0.9998178,
        "name": "Space Needle"
      }
    ]
  }
}
```

## 使用计算机视觉 API 通过 Python 获取缩略图 <a name="GetThumbnail"> </a>

使用[“获取缩略图”方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fb)可以根据图像的兴趣区域 (ROI) 将该图像裁剪为所需的高度和宽度。 为缩略图设置的纵横比可以不同于输入图像的纵横比。

### <a name="get-a-thumbnail-python-example-request"></a>获取缩略图 Python 示例请求

复制 Python 版本的相应部分，并将其保存到某个文件，例如 `thumbnail.py`。 将 `subscription_key` 值替换为有效的订阅密钥，并更改 `uri_base` 以使用订阅密钥的获取位置。 然后执行脚本。

```python
########### Python 2.7 #############
import httplib, urllib, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.urlencode({
    # Request parameters. The smartCropping flag is optional.
    'width': '150',
    'height': '100',
    'smartCropping': 'true',
})

# The URL of a JPEG image to use to create a thumbnail image.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = httplib.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/vision/v1.0/generateThumbnail?%s" % params, body, headers)
    response = conn.getresponse()
    
    # Check for success.
    if response.status == 200:
        # Success. Use 'response.read()' to return the image data. 
        # Display the response headers.
        print ('Success.')
        print ('Response headers:')
        headers = response.getheaders()
        for field, value in headers:
            print ('  ' + field + ': ' + value)
    else:
        # Error. 'data' contains the JSON error data. Display the error data.
        data = response.read()
        parsed = json.loads(data)
        print ('Error:')
        print (json.dumps(parsed, sort_keys=True, indent=2))
    
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################

########### Python 3.6 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.parse.urlencode({
    # Request parameters. The smartCropping flag is optional.
    'width': '150',
    'height': '100',
    'smartCropping': 'true',
})

# Replace the three dots below with the URL of the JPEG image for which you want a thumbnail.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = http.client.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/vision/v1.0/generateThumbnail?%s" % params, body, headers)
    response = conn.getresponse()
    
    # Check for success.
    if response.status == 200:
        # Success. Use 'response.read()' to return the image data. 
        # Display the response headers.
        print ('Success.')
        print ('Response headers:')
        headers = response.getheaders()
        for field, value in headers:
            print ('  ' + field + ': ' + value)
    else:
        # Error. 'data' contains the JSON error data. Display the error data.
        data = response.read()
        parsed = json.loads(data)
        print ('Error:')
        print (json.dumps(parsed, sort_keys=True, indent=2))
    
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################
```

### <a name="get-a-thumbnail-response"></a>获取缩略图响应

成功响应包含缩略图二进制文件。 如果请求失败，则响应包含错误代码和消息，以帮助确定问题所在。 下面是成功响应的示例：

```text
Success.
Response headers:
  Cache-Control: no-cache
  Pragma: no-cache
  Content-Length: 4025
  Content-Type: image/jpeg
  Expires: -1
  X-AspNet-Version: 4.0.30319
  X-Powered-By: ASP.NET
  apim-request-id: e2533391-44a9-4265-a681-91b07aaab6dd
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
  x-content-type-options: nosniff
  Date: Thu, 08 Jun 2017 22:51:16 GMT
```

## 使用计算机视觉 API 通过 Python 执行光学字符识别 (OCR) <a name="OCR"> </a>

使用[光学字符识别 (OCR) 方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc)可以检测图像中的文本，并将识别到的字符提取到机器可用的字符流中。

### <a name="ocr-python-example-request"></a>OCR Python 示例请求

复制 Python 版本的相应部分，并将其保存到某个文件，例如 `ocr.py`。 将 `subscription_key` 值替换为有效的订阅密钥，并更改 `uri_base` 以使用订阅密钥的获取位置。 然后执行脚本。

```python
########### Python 2.7 #############
import httplib, urllib, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.urlencode({
    # Request parameters. The language setting "unk" means automatically detect the language.
    'language': 'unk',
    'detectOrientation ': 'true',
})

# The URL of a JPEG image containing text.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png'}"

try:
    # Execute the REST API call and get the response.
    conn = httplib.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/vision/v1.0/ocr?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################

########### Python 3.6 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

params = urllib.parse.urlencode({
    # Request parameters. The language setting "unk" means automatically detect the language.
    'language': 'unk',
    'detectOrientation ': 'true',
})

# The URL of a JPEG image containing text.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png'}"

try:
    # Execute the REST API call and get the response.
    conn = http.client.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/vision/v1.0/ocr?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################
```

### <a name="ocr-example-response"></a>OCR 示例响应

如果成功，OCR 结果包括图像中的文本。 此外，还包括区域范围框、线条和单词。 下面是成功响应的示例：

```json
Response:
{
  "language": "en",
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
  ],
  "textAngle": 0.0
}
```

## 使用计算机视觉 API 通过 Python 执行文本识别 <a name="RecognizeText"> </a>

### <a name="handwriting-recognition-python-example"></a>手写文本识别 Python 示例

复制 Python 版本的相应部分，并将其保存到某个文件，例如 `handwriting.py`。 将 `subscription_key` 值替换为有效的订阅密钥，并更改 `uri_base` 以使用订阅密钥的获取位置。 然后执行脚本。

```python
########### Python 2.7 #############
import httplib, urllib, base64, time, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

headers = {
    # Request headers.
    # Another valid content type is "application/octet-stream".
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

# The URL of a JPEG image containing handwritten text.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg'}"

# For printed text, set "handwriting" to false.
params = urllib.urlencode({'handwriting' : 'true'})

try:
    # This operation requrires two REST API calls. One to submit the image for processing,
    # the other to retrieve the text found in the image. 
    #
    # This executes the first REST API call and gets the response.
    conn = httplib.HTTPSConnection(uri_base)
    conn.request("POST", "/vision/v1.0/RecognizeText?%s" % params, body, headers)
    response = conn.getresponse()
    
    # Success is indicated by a status of 202.
    if response.status != 202:
        # Display JSON data and exit if the first REST API call was not successful.
        parsed = json.loads(response.read())
        print ("Error:")
        print (json.dumps(parsed, sort_keys=True, indent=2))
        conn.close()
        exit()

    # The 'Operation-Location' in the response contains the URI to retrieve the recognized text.
    operationLocation = response.getheader('Operation-Location')
    parsedLocation = operationLocation.split(uri_base)
    answerURL = parsedLocation[1]

    # NOTE: The response may not be immediately available. Handwriting recognition is an
    # async operation that can take a variable amount of time depending on the length
    # of the text you want to recognize. You may need to wait or retry this GET operation.

    print('\nHandwritten text submitted. Waiting 10 seconds to retrieve the recognized text.\n')
    time.sleep(10)
    
    # Execute the second REST API call and get the response.
    conn = httplib.HTTPSConnection(uri_base)
    conn.request("GET", answerURL, '', headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print('Error:')
    print(e)

####################################

########### Python 3.6 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, requests, time, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'https://api.cognitive.azure.cn'

requestHeaders = {
    # Request headers.
    # Another valid content type is "application/octet-stream".
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

# The URL of a JPEG image containing handwritten text.
body = {'url' : 'https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg'}

# For printed text, set "handwriting" to false.
params = {'handwriting' : 'true'}

try:
    # This operation requrires two REST API calls. One to submit the image for processing,
    # the other to retrieve the text found in the image. 
    #
    # This executes the first REST API call and gets the response.
    response = requests.request('POST', uri_base + '/vision/v1.0/RecognizeText', json=body, data=None, headers=requestHeaders, params=params)
    
    # Success is indicated by a status of 202.
    if response.status_code != 202:
        # if the first REST API call was not successful, display JSON data and exit.
        parsed = json.loads(response.text)
        print ("Error:")
        print (json.dumps(parsed, sort_keys=True, indent=2))
        exit()

    # The 'Operation-Location' in the response contains the URI to retrieve the recognized text.
    operationLocation = response.headers['Operation-Location']

    # Note: The response may not be immediately available. Handwriting recognition is an
    # async operation that can take a variable amount of time depending on the length
    # of the text you want to recognize. You may need to wait or retry this GET operation.

    print('\nHandwritten text submitted. Waiting 10 seconds to retrieve the recognized text.\n')
    time.sleep(10)
    
    # Execute the second REST API call and get the response.
    response = requests.request('GET', operationLocation, json=None, data=None, headers=requestHeaders, params=None)

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(response.text)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))

except Exception as e:
    print('Error:')
    print(e)

####################################
```

成功响应将以 JSON 格式返回。 下面是成功响应的示例：

```json
Response:
{
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
  },
  "status": "Succeeded"
}
```

