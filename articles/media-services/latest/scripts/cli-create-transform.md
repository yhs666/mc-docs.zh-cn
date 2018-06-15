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
ms.openlocfilehash: 3a17ec1511e7befaf1758d723d49848a7f69e0ae
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324303"
---
# <a name="cli-example-create-a-transform"></a>CLI 示例：创建转换

本文中的 Azure CLI 脚本演示如何创建转换。 转换描述了处理视频或音频文件的任务的简单工作流（通常被称作“脚本”）。 应始终检查具有所需名称和“脚本”的转换是否已存在。 如果已存在，应再次使用该转换。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="update-the-following-variables-for-your-own-settings"></a>更新自己的设置的下列变量：
resourceGroup=amsResourceGroup amsAccountName=amsmediaaccountname

# <a name="create-a-simple-transform-for-adaptive-bitrate-encoding"></a>创建适用于自适应比特率编码的简单转换
az ams transform create \
 --name myFirstTransform \
 --preset-names AdaptiveStreaming \
 --description '适用于自适应比特率编码的简单转换' \
 -g $resourceGroup \
 -a $amsAccountName \

 # <a name="create-a-transform-for-video-analyer-preset"></a>创建适用于视频分析器预设的转换
az ams transform create \
 --name videoAnalyzerTransform \
 --preset-names  VideoAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

 # <a name="create-a-transform-for-audio-analzyer-preset"></a>创建适用于音频分析器预设的转换
az ams transform create \
 --name audioAnalyzerTransform \
 --preset-names  AudioAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

# <a name="create-a-transform-with-two-built-in-presets-executed-in-sequence"></a>创建两个内置预设按顺序执行的转换
az ams transform create \
 --name twoPresetTransform \
 --preset-names AdaptiveStreaming VideoAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

# <a name="list-all-the-transforms-in-an-account"></a>列出帐户中的所有转换
az ams transform list -a $amsAccountName -g $resourceGroup

echo "按 [ENTER] 继续。"
read continue
```

