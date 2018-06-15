---
title: Azure CLI 脚本示例 - 发布资产 | Azure
description: 使用 Azure CLI 脚本发布资产。
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
ms.openlocfilehash: d5360847a6feb67b3c41fc5b679f06ad199332f9
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324286"
---
# <a name="cli-example-publish-an-asset"></a>CLI 示例：发布资产

本文中的 Azure CLI 脚本演示如何创建流式处理定位符并返回流式处理 URL。 



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="warning--this-shell-script-requires-python-3-to-be-installed-to-parse-json"></a>警告：此 shell 脚本需要安装 Python 3 才能分析 JSON。 

# <a name="update-the-following-variables-for-your-own-settings"></a>更新自己的设置的下列变量：
resourceGroup=amsResourceGroup amsAccountName=amsmediaaccountname assetName="myAsset-uniqueID" locatorName="myStreamingLocator" streamingPolicyName="Predefined_DownloadAndClearStreaming"
#<a name="contentpolicyname"></a>contentPolicyName=""

# <a name="delete-the-locator-if-it-already-exists"></a>如果定位符已存在，则将其删除
az ams streaming locator delete \
    -a $amsAccountName \
    -g $resourceGroup \
    -n $locatorName \

# <a name="create-a-new-streaming-locator-modify-the-assetname-variable-to-point-to-the-asset-you-want-to-publish"></a>创建新的流式处理定位符。 修改 assetName 变量，使之指向要发布的资产
# <a name="this-uses-the-predefined-clear-streaming-only-policy-which-allows-for-unencypted-deliver-over-hls-smooth-and-dash-protocols"></a>这样会使用预定义的“仅清除流式处理”策略，以便通过 HLS、Smooth 和 DASH 协议进行未加密的传送。 
az ams streaming locator create \
    -a $amsAccountName \
    -g $resourceGroup \
    --asset-name $assetName \
    -n $locatorName \
    --streaming-policy-name $streamingPolicyName \
    #--end-time 2100-10-10T00:00:00Z \
    #--start-time 2018-04-28T00:00:00Z \
    #--content-policy-name $contentPolicyName \

# <a name="list-the-streaming-endpoints-on-the-account-if-this-is-a-new-account-it-only-has-a-default-endpoint-which-may-be-stopped"></a>列出帐户上的流式处理终结点。 如果这是新帐户，则只有可以停止的“默认”终结点。
# <a name="to-stream-you-must-first-start-a-streaming-endpoint-on-your-account"></a>若要进行流式处理，必须先在帐户上启动一个流式处理终结点。 
# <a name="this-next-commmand-lists-the-streaming-endpoints-and-gets-the-value-of-the-hostname-property-for-the-default-endpoint-to-be-used-when-building"></a>接下来的这个命令用于列出流式处理终结点，并获取“默认”终结点的“主机名”属性的值，该终结点是根据此后的定位符 get-paths 方法生成
# <a name="the-complete-streaming-or-download-url-from-the-locator-get-paths-method-following-this"></a>完整的流式处理或下载 URL 时需要使用的。
# <a name="note-this-command-requires-python-35-to-be-installed"></a>主要：此命令需要安装 Python 3.5。 
hostName=$(az ams streaming endpoint list \
    -a $amsAccountName \
    -g $resourceGroup  | \
    python -c "import sys, json; print(json.load(sys.stdin)[0]['hostName'])")

echo -e "\n" echo -e "默认主机名: https://"$hostName
   
# <a name="list-the-streming-urls-relative-paths-for-the-new-locator--you-must-append-your-streaming-endpoint-hostname-path-to-these-to-resolve-the-full-url"></a>列出新定位符的流式处理 URL 相对路径。  必须将流式处理终结点“主机名”路径附加到这些项才能解析完整的 URL。 
# <a name="note-that-the-asset-must-have-an-ismc-and-be-encoded-for-adaptive-streaming-in-order-to-get-streaming-urls-back-you-can-get-download-paths-for-any-content-type"></a>请注意，此资产必须有一个 .ismc，并且必须针对自适应流式处理进行编码，以便取回流式处理 URL。 可以获取任何内容类型的下载路径。
paths=$(az ams streaming locator get-paths \
    -a $amsAccountName \
    -g $resourceGroup \
    -n $locatorName )

downloadPaths=$(echo $paths | \
                python -c "import sys, json; print(json.load(sys.stdin)['downloadPaths'])" )

streamingPaths=$(echo $paths |\
                python -c "import sys, json; print(json.load(sys.stdin)['streamingPaths'])" )

echo -e "\n" echo "DownloadPaths:" echo $downloadPaths echo -e "\n" echo "StreamingPaths:" echo $streamingPaths

echo "按 [ENTER] 继续。"
read continue
```

