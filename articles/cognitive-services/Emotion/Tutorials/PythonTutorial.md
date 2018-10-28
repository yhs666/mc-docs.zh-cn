---
title: 教程：识别图像中人脸的情感 - 情感 API、Python
titlesuffix: Azure Cognitive Services
description: 使用 Jupyter Notebook 了解如何通过 Python 使用情感 API。 使用流行库可视化结果。
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
origin.date: 05/23/2017
ms.date: 10/25/2018
ms.author: v-junlch
ROBOTS: NOINDEX
ms.openlocfilehash: 560f7d228928d0fdceba999db5087d9515f394d2
ms.sourcegitcommit: 44ce337717bb948f5ac08217a156935f663c0f46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50034661"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>教程：通过 Jupyter Notebook 和 Python 使用情感 API。

> [!IMPORTANT]
> 情感 API 将于 2019 年 2 月 15 日弃用。 情感识别功能现在已作为[人脸 API](/cognitive-services/face/) 的一部分正式发布。 

为了让你轻松开始使用情感 API，下面链接的 Jupyter Notebook 演示了如何在 Python 中使用 API，以及如何使用某些流行库可视化结果。

[链接到 GitHub 中的 Notebook](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>使用 Jupyter Notebook

若要以交互方式使用 Notebook，需要克隆并在 Jupyter 中运行 Notebook。 若要了解如何开始使用交互式 Jupyter 笔记本，请遵循 http://jupyter.readthedocs.org/en/latest/install.html 上的说明。

若要使用此 Notebook，需要情感 API 的订阅密钥。 可以转到 [Azure 门户](https://portal.azure.cn)，并使用情感 API 创建一个认知服务来获取订阅密钥。 

```
Python Example

#Variables

_url = 'https://api.cognitive.azure.cn/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```

