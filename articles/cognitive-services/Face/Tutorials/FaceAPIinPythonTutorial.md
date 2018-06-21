---
title: 人脸 API Python 教程 | Microsoft Docs
description: 了解如何在认知服务中使用人脸 API 和 Python SDK 检测图像中的人脸。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 02/24/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: bf093cc53064d8d8c2ec67ebf4aa2f0b825f0091
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407587"
---
# <a name="getting-started-with-face-api-in-python-tutorial"></a>Python 中的人脸 API 入门教程

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

KEY = 'subscription key'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
result = CF.face.detect(img_url)
print result
```

下面是示例结果。 它是检测到的人脸的 `list`。 列表中的每个项是一个 `dict` 实例，其中，`faceId` 是检测到的人脸的唯一 ID，`faceRectangle` 描述检测到的人脸所在的位置。 人脸 ID 在 24 小时后失效。

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name='further'></a>进一步探索

为了帮助读者进一步探索人脸 API，本教程提供了 GUI 示例。 若要运行该示例，请先安装 [wxPython](https://wxpython.org/)，然后运行以下命令。

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

