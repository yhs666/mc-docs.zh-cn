---
title: 教程：计算机视觉 API Python
titlesuffix: Azure Cognitive Services
description: 通过 Jupyter Notebook 了解如何使用 Python 中的计算机视觉 API。 使用流行库可视化结果。
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
origin.date: 02/25/2017
ms.date: 10/30/2018
ms.author: v-junlch
ms.openlocfilehash: 5442f2d59cfd96a83faa37c5dd81381d103d591e
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50408989"
---
# <a name="tutorial-computer-vision-api-python"></a>教程：计算机视觉 API Python

本教程演示如何在 Python 中使用计算机视觉 API，以及如何使用某些流行库来可视化结果。 使用 Jupyter 运行教程。 若要了解如何开始使用交互式 Jupyter Notebook，请参阅 [Jupyter 文档](http://jupyter.readthedocs.io/en/latest/index.html)。 

## <a name="open-the-tutorial-notebook-in-jupyter"></a>在 Jupyter 中打开教程 Notebook 

1. 导航到 [GitHub 中的教程 Notebook](https://github.com/Microsoft/Cognitive-Vision-Python)。 
2. 单击绿色按钮克隆或下载教程。 
3. 打开命令提示符并转到文件夹 _Cognitive-Vision-Python-master\Jupyter Notebook_。 
4. 在命令提示符下运行命令 **jupyter notebook**。 这会启动 Jupyter。
5. 在 Jupyter 窗口中，单击“Computer Vision API Example.ipynb”打开教程 Notebook 

## <a name="run-the-tutorial"></a>运行教程

若要使用此 Notebook，需要计算机视觉 API 的订阅密钥。 访问 [Azure 门户](https://portal.azure.cn/)完成注册。 主密钥或辅助密钥都有效。 确保将密钥括在引号中以使其成为字符串。

```python
# Variables

_url = 'https://api.cognitive.azure.cn/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```

