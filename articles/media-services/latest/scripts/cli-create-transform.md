---
title: Azure CLI 脚本示例 - 创建转换 | Azure
description: 使用 Azure CLI 脚本创建转换。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 05/11/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: cc493449490403e4e10b363912676b48de185d61
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405383"
---
# <a name="cli-example-create-a-transform"></a>CLI 示例：创建转换

本文中的 Azure CLI 脚本演示如何创建转换。 转换描述了处理视频或音频文件的任务的简单工作流（通常被称作“脚本”）。 应始终检查具有所需名称和“脚本”的转换是否已存在。 如果已存在，应再次使用该转换。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```cli
#!/bin/bash

# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname

# Create a simple Transform for Adaptive Bitrate Encoding
az ams transform create \
 --name myFirstTransform \
 --preset-names AdaptiveStreaming \
 --description 'a simple Transform for Adaptive Bitrate Encoding' \
 -g $resourceGroup \
 -a $amsAccountName \

 # Create a Transform for Video Analyer Preset
az ams transform create \
 --name videoAnalyzerTransform \
 --preset-names  VideoAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

 # Create a Transform for Audio Analzyer Preset
az ams transform create \
 --name audioAnalyzerTransform \
 --preset-names  AudioAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

# Create a Transform with two built-in Presets executed in sequence
az ams transform create \
 --name twoPresetTransform \
 --preset-names AdaptiveStreaming VideoAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

# List all the Transforms in an account
az ams transform list -a $amsAccountName -g $resourceGroup

echo "press  [ENTER]  to continue."
read continue
```

