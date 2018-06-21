---
title: 人脸 API Python 快速入门 | Microsoft Docs
description: 获取信息和代码示例，帮助自己快速开始使用认知服务中的人脸 API 和 Python。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 06/21/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: e7b959ff832128c57208cc70ab38ec8220237f01
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407594"
---
# <a name="face-api-python-quick-starts"></a>人脸 API Python 快速入门
本文提供信息和代码示例来帮助读者快速开始使用人脸 API 和 Python 来完成以下任务： 
- [检测图像中的人脸](#Detect) 
- [创建人员组](#Create)

在[此处](../../Computer-vision/Vision-API-How-to-Topics/HowToSubscribe.md)详细了解如何获取免费订阅密钥

## 使用人脸 API 通过 Python 检测图像中的人脸 <a name="Detect"> </a>
使用[“人脸 - 检测”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)可以检测图像中的人脸，并返回人脸属性，包括：
- 人脸 ID：在多种人脸 API 方案使用的唯一 ID。 
- 人脸矩形：左侧坐标、顶部坐标、宽度和高度，指示人脸在图像中的位置。
- 地标：27 点人脸地标数组，指向人脸组成部分的重要位置。
- 面部属性包括年龄、性别、笑容强度、头部姿势和面部毛发。 

#### <a name="face-detect-python-example-request"></a>人脸检测 Python 示例请求

1. 复制 Python 版本的相应部分，并将其保存到某个文件，例如 `detect_faces.py`。
1. 将 `subscriptionKey` 值替换为有效的订阅密钥。
1. 更改 `uri_base` 值以使用订阅密钥的获取位置。
1. 运行示例。

```python
import httplib, urllib, base64, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'api.cognitive.azure.cn'

# Request headers.
headers = {
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

# Request parameters.
params = urllib.urlencode({
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'false',
    'returnFaceAttributes': 'age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise',
})

# The URL of a JPEG image to analyze.
body = "{'url':'https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg'}"

try:
    # Execute the REST API call and get the response.
    conn = httplib.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/face/v1.0/detect?%s" % params, body, headers)
    response = conn.getresponse()
    data = response.read()

    # 'data' contains the JSON data. The following formats the JSON data for display.
    parsed = json.loads(data)
    print ("Response:")
    print (json.dumps(parsed, sort_keys=True, indent=2))
    conn.close()

except Exception as e:
    print("[Errno {0}] {1}".format(e.errno, e.strerror))

####################################

########### Python 3.6 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, requests, json

###############################################
#### Update or verify the following values. ###
###############################################

# Replace the subscription_key string value with your valid subscription key.
subscription_key = '13hc77781f7e4b19b5fcdd72a8df7156'

uri_base = 'https://api.cognitive.azure.cn'

# Request headers.
headers = {
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': subscription_key,
}

# Request parameters.
params = {
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'false',
    'returnFaceAttributes': 'age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise',
}

# Body. The URL of a JPEG image to analyze.
body = {'url': 'https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg'}

try:
    # Execute the REST API call and get the response.
    response = requests.request('POST', uri_base + '/face/v1.0/detect', json=body, data=None, headers=headers, params=params)

    print ('Response:')
    parsed = json.loads(response.text)
    print (json.dumps(parsed, sort_keys=True, indent=2))

except Exception as e:
    print('Error:')
    print(e)

####################################    

```
#### <a name="face-detect-response"></a>人脸检测响应

成功响应将以 JSON 格式返回。 下面是成功响应的示例： 

```json
Response:
[
  {
    "faceAttributes": {
      "accessories": [],
      "age": 22.9,
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "emotion": {
        "anger": 0.0,
        "contempt": 0.0,
        "disgust": 0.0,
        "fear": 0.0,
        "happiness": 0.0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "facialHair": {
        "beard": 0.0,
        "moustache": 0.0,
        "sideburns": 0.0
      },
      "gender": "female",
      "glasses": "NoGlasses",
      "hair": {
        "bald": 0.0,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1.0
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
        ],
        "invisible": false
      },
      "headPose": {
        "pitch": 0.0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0.0
      },
      "occlusion": {
        "eyeOccluded": false,
        "foreheadOccluded": false,
        "mouthOccluded": false
      },
      "smile": 0.0
    },
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "height": 162,
      "left": 177,
      "top": 131,
      "width": 162
    }
  }
]
```
## 使用人脸 API 通过 Python 创建人员组 <a name="Create"> </a>
使用[“人员组 - 创建人员组”方法](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)可以创建包含指定 personGroupId、名称和用户提供的 userData 的人员组。 人员组是“人脸 - 识别”API 的最重要参数之一。 “识别”API 会在指定的人员组中搜索人员的平面。 

#### <a name="person-group---create-a-person-group-example"></a>“人员组 - 创建人员组”示例

复制 Python 版本的相应部分，并将其保存到某个文件，例如 `test.py`。 将“Ocp-Apim-Subscription-Key”值替换为有效的订阅密钥，并更改 REST URL 以使用订阅密钥的获取位置。

```python
########### Python 2.7 #############
import httplib, urllib, base64

headers = {
    # Request headers.
    'Content-Type': 'application/json',

    # NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key': '13hc77781f7e4b19b5fcdd72a8df7156',
}

# Replace 'examplegroupid' with an ID you haven't used for creating a group before.
# The valid characters for the ID include numbers, English letters in lower case, '-' and '_'. 
# The maximum length of the ID is 64.
personGroupId = 'examplegroupid'

# The userData field is optional. The size limit for it is 16KB.
body = "{ 'name':'group1', 'userData':'user-provided data attached to the person group' }"

try:
    conn = httplib.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("POST", "/face/v1.0/persongroups/%s" % personGroupId, body, headers)
    response = conn.getresponse()

    # 'OK' indicates success. 'Conflict' means a group with this ID already exists.
    # If you get 'Conflict', change the value of personGroupId above and try again.
    # If you get 'Access Denied', verify the validity of the subscription key above and try again.
    print(response.reason)

    conn.close()
except Exception as e:
    print("[Errno {0}] {1}".format(e.errno, e.strerror))
####################################

########### Python 3.2 #############
import http.client, urllib.request, urllib.parse, urllib.error, base64, sys

headers = {
    # Request headers.
    'Content-Type': 'application/json',

    # NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key': '13hc77781f7e4b19b5fcdd72a8df7156',
}

# Replace 'examplegroupid' with an ID you haven't used for creating a group before.
# The valid characters for the ID include numbers, English letters in lower case, '-' and '_'. 
# The maximum length of the ID is 64.
personGroupId = 'examplegroupid'

# The userData field is optional. The size limit for it is 16KB.
body = "{ 'name':'group1', 'userData':'user-provided data attached to the person group' }"

try:
    conn = http.client.HTTPSConnection('api.cognitive.azure.cn')
    conn.request("PUT", "/face/v1.0/persongroups/%s" % personGroupId, body, headers)
    response = conn.getresponse()

    # 'OK' indicates success. 'Conflict' means a group with this ID already exists.
    # If you get 'Conflict', change the value of personGroupId above and try again.
    # If you get 'Access Denied', verify the validity of the subscription key above and try again.
    print(response.reason)

    conn.close()
except Exception as e:
    print(e.args)
####################################

