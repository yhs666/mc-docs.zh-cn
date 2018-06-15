---
title: Azure CLI 脚本示例 - 创建和提交作业 | Azure
description: 本主题中的 Azure CLI 脚本演示如何使用 HTTPs URL 将作业提交到简单编码的转换。
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
ms.openlocfilehash: 1282c62dc61a5f58fb6fd7fc1100274107c54c84
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324275"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI 示例：创建并提交作业

本文中的 Azure CLI 脚本演示如何使用 HTTPs URL 创建作业并将其提交到简单编码的转换。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="update-the-following-variables-for-your-own-settings"></a>更新自己的设置的下列变量：
resourceGroup=amsResourceGroup amsAccountName=amsmediaaccountname outputAssetName=myOutputAsset transformName=audioAnalyzerTransform

# <a name="note-first-create-the-transforms-in-the-create-transformsh-for-these-jobs-to-work"></a>注意：首先请在 Create-Transform.sh 中为这些要运行的作业创建转换！

# <a name="create-a-media-services-asset-to-output-the-job-results-to"></a>创建要将作业结果输出到其中的媒体服务资产。
az ams asset create \
    -n $outputAssetName \
    -a $amsAccountName \
    -g $resourceGroup \

# <a name="submit-a-job-to-a-simple-encoding-transform-using-https-url"></a>使用 HTTPS URL 将作业提交到简单编码转换
az ams job start \
    --name myFirstJob_007 \
    --transform-name $transformName \
    --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/00000000-b215-4409-80af-529c3e853622/' \
    --files 'Ignite-short.mp4' \
    --output-asset-names $outputAssetName \
    -a $amsAccountName \
    -g $resourceGroup \

echo "按 [ENTER] 继续。"
read continue
```

