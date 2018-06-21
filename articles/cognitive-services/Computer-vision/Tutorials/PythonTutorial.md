---
title: 计算机视觉 API Python 教程 | Microsoft Docs
description: 了解如何在 Microsoft 认知服务中通过 Jupyter Notebook 将计算机视觉 API 与 Python 配合使用。 使用流行库可视化结果。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: computer-vision
ms.topic: article
origin.date: 02/25/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 6854f3b815715b570f6d834fcc5e4f825b84693c
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407581"
---
# <a name="computer-vision-api-python-tutorial"></a>计算机视觉 API Python 教程

本教程演示如何在 Python 中使用计算机视觉 API，以及如何使用某些流行库来可视化结果。 使用 Jupyter 运行教程。 若要了解如何开始使用交互式 Jupyter Notebook，请参阅 [Jupyter 文档](http://jupyter.readthedocs.io/en/latest/index.html)。 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>在 Jupyter 中打开教程 Notebook 

1. 导航到 [GitHub 中的教程 Notebook](https://github.com/Microsoft/Cognitive-Vision-Python)。 
2. 单击绿色按钮克隆或下载教程。 
3. 打开命令提示符并转到文件夹 _Cognitive-Vision-Python-master\Jupyter Notebook_。 
4. 在命令提示符下运行命令 **jupyter notebook**。 这会启动 Jupyter。
5. 在 Jupyter 窗口中，单击“Computer Vision API Example.ipynb”打开教程 Notebook 

### <a name="running-the-tutorial"></a>运行教程

若要使用此 Notebook，需要计算机视觉 API 的订阅密钥。 访问 [Azure 门户](https://portal.azure.cn/)完成注册。 

```python
# Variables

_url = 'https://api.cognitive.azure.cn/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```

