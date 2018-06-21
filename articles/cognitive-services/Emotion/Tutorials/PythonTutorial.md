---
title: 情感 API Python 教程 | Microsoft Docs
description: 使用 Jupyter Notebook 了解如何将认知服务情感 API 与 Python 配合使用。 使用流行库可视化结果。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 05/23/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 14c0708993bd0b770618969ee4d579e48614efdb
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407568"
---
# <a name="emotion-api-using-python-tutorial"></a>使用 Python 的情感 API 教程

为了让你轻松开始使用情感 API，下面链接的 Jupyter Notebook 演示了如何在 Python 中使用 API，以及如何使用某些流行库可视化结果。 

[链接到 GitHub 中的 Notebook](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>使用 Jupyter Notebook

若要以交互方式使用 Notebook，需要克隆并在 Jupyter 中运行 Notebook。 若要了解如何开始使用交互式 Jupyter Notebook，请遵照 http://jupyter.readthedocs.org/en/latest/install.html 上的说明。 

若要使用此 Notebook，需要情感 API 的订阅密钥。 可以转到 [Azure 门户](https://portal.azure.cn)，并使用情感 API 创建一个认知服务来获取订阅密钥。 

```
Python Example 

#Variables

_url = 'https://api.cognitive.azure.cn/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```

