---
title: 教程：检测和定格图像中的人脸 - 人脸 API、Python
titleSuffix: Azure Cognitive Services
description: 了解如何使用人脸 API 和 Python SDK 检测图像中的人脸。
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
origin.date: 03/01/2018
ms.date: 10/23/2018
ms.author: v-junlch
ms.openlocfilehash: 721c6ce3dffa514b1b9672225d750096056b230d
ms.sourcegitcommit: 44ce337717bb948f5ac08217a156935f663c0f46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50034645"
---
# <a name="tutorial-detect-and-frame-faces-with-the-face-api-and-python"></a>教程：使用人脸 API 和 Python 检测和定格人脸 

本教程介绍如何通过 Python SDK 调用人脸 API 来检测图像中的人脸。

## <a name="prerequisites"></a>先决条件

若要使用本教程，需要做好以下准备：

- 安装 Python 2.7+ 或 Python 3.5+。
- 安装 pip。
- 按如下所述安装人脸 API的 Python SDK：

```bash
pip install cognitive_face
```

- 在 Azure 门户中获取 Microsoft 认知服务的订阅密钥。 在本教程中，可以使用主密钥或辅助密钥。 （请注意，若要使用任何人脸 API，必须拥有有效的订阅密钥。）

## <a name="sdk-example"></a>检测图像中的人脸

```python
import cognitive_face as CF

KEY = '<Subscription Key>'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

BASE_URL = 'https://api.cognitive.azure.cn/face/v1.0/'  
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

下面是示例结果。 它是检测到的人脸的 `list`。 列表中的每项都是一个 `dict` 实例，其中 `faceId` 是检测到的人脸的唯一 ID，`faceRectangle` 说明检测到的人脸的位置。 人脸 ID 的有效期为 24 小时。

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>在人脸周围绘制矩形

使用通过上一命令接收到的 json 坐标，可在图像上绘制矩形，以可视方式呈现每张人脸。 需具备 `pip install Pillow` 才能使用 `PIL` 成像模块。  在该文件的顶部，添加以下代码：

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

然后，在脚本的 `print(faces)` 后面包括以下代码：

```python
#Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))

#Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

#For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

#Display the image in the users default image browser.
img.show()
```

## <a name='further'></a>深入了解

为了帮助读者进一步探索人脸 API，本教程提供了 GUI 示例。 若要运行该示例，请先安装 [wxPython](https://wxpython.org/pages/downloads/)，然后运行以下命令。

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a>摘要

本教程介绍了通过调用 Python SDK 使用人脸 API 的基本过程。 有关 API 的详细信息，请参阅操作说明和 [API 参考](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)。

## <a name="related"></a>相关主题

- [CSharp 中的人脸 API 入门](FaceAPIinCSharpTutorial.md)
- [Java for Android 中的人脸 API 入门](FaceAPIinJavaForAndroidTutorial.md)

