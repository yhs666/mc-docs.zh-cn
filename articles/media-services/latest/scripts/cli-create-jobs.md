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
ms.openlocfilehash: 21a65efb5fb95d3561bdee675124d221f5cf52fe
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647010"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI 示例：创建并提交作业

本文中的 Azure CLI 脚本演示如何使用 HTTPs URL 创建作业并将其提交到简单编码的转换。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```cli
#!/bin/bash

# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname
outputAssetName=myOutputAsset
transformName=audioAnalyzerTransform

# NOTE: First create the Transforms in the Create-Transform.sh for these jobs to work!

# Create a Media Services Asset to output the job results to.
az ams asset create \
    -n $outputAssetName \
    -a $amsAccountName \
    -g $resourceGroup \

# Submit a Job to a simple encoding Transform using HTTPs URL
az ams job start \
    --name myFirstJob_007 \
    --transform-name $transformName \
    --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.chinacloudapi.cn/00000000-b215-4409-80af-529c3e853622/' \
    --files 'Ignite-short.mp4' \
    --output-asset-names $outputAssetName \
    -a $amsAccountName \
    -g $resourceGroup \

echo "press  [ENTER]  to continue."
read continue
```

