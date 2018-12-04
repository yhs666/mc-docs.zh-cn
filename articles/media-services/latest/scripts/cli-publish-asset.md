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
ms.openlocfilehash: be9849ac0e7a8421ef9e4f24bf719fd72f8e05bc
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647015"
---
# <a name="cli-example-publish-an-asset"></a>CLI 示例：发布资产

本文中的 Azure CLI 脚本演示如何创建流式处理定位符并返回流式处理 URL。 



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```cli
#!/bin/bash

# WARNING:  This shell script requires Python 3 to be installed to parse JSON. 

# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname
assetName="myAsset-uniqueID"
locatorName="myStreamingLocator"
streamingPolicyName="Predefined_DownloadAndClearStreaming"
#contentPolicyName=""

# Delete the locator if it already exists
az ams streaming locator delete \
    -a $amsAccountName \
    -g $resourceGroup \
    -n $locatorName \

# Create a new Streaming Locator. Modify the assetName variable to point to the Asset you want to publish
# This uses the predefined Clear Streaming Only policy, which allows for unencypted deliver over HLS, Smooth and DASH protocols. 
az ams streaming locator create \
    -a $amsAccountName \
    -g $resourceGroup \
    --asset-name $assetName \
    -n $locatorName \
    --streaming-policy-name $streamingPolicyName \
    #--end-time 2100-10-10T00:00:00Z \
    #--start-time 2018-04-28T00:00:00Z \
    #--content-policy-name $contentPolicyName \

# List the Streaming Endpoints on the account. If this is a new account it only has a 'default' endpoint, which may be stopped.
# To stream, you must first Start a Streaming Endpoint on your account. 
# This next commmand lists the Streaming Endpoints, and gets the value of the "hostname" property for the 'default' endpoint to be used when building
# the complete Streaming or download URL from the locator get-paths method following this.
# NOTE: This command requires Python 3.5 to be installed. 
hostName=$(az ams streaming endpoint list \
    -a $amsAccountName \
    -g $resourceGroup  | \
    python -c "import sys, json; print(json.load(sys.stdin)[0]['hostName'])")

echo -e "\n"
echo -e "Default hostname: https://"$hostName
   
# List the Streming URLs relative paths for the new locator.  You must append your Streaming Endpoint "hostname" path to these to resolve the full URL. 
# Note that the asset must have an .ismc and be encoded for Adaptive streaming in order to get Streaming URLs back. You can get download paths for any content type.
paths=$(az ams streaming locator get-paths \
    -a $amsAccountName \
    -g $resourceGroup \
    -n $locatorName )

downloadPaths=$(echo $paths | \
                python -c "import sys, json; print(json.load(sys.stdin)['downloadPaths'])" )

streamingPaths=$(echo $paths |\
                python -c "import sys, json; print(json.load(sys.stdin)['streamingPaths'])" )

echo -e "\n"
echo "DownloadPaths:" 
echo $downloadPaths
echo -e "\n"
echo "StreamingPaths:"
echo $streamingPaths

echo "press  [ENTER]  to continue."
read continue
```

