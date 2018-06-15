---
title: Azure CLI 脚本示例 - 创建 Azure 媒体服务资产 | Azure
description: 本主题中的 Azure CLI 脚本演示如何创建媒体服务资产供上传内容使用。
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
origin.date: 04/15/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: 05a34ad86b69c35394a30c059f04b0dfabb6a8fc
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324288"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>CLI 示例：创建 Azure 媒体服务帐户

本文中的 Azure CLI 脚本演示如何创建媒体服务资产供上传内容使用。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="update-the-following-variables-for-your-own-settings"></a>更新自己的设置的下列变量：
resourceGroup=amsResourceGroup amsAccountName=amsmediaaccountname assetName="myAsset-uniqueID" expiry=$(date -u +"%Y-%m-%dT%TZ" -d "+23 小时")

# <a name="create-a-media-services-asset-to-upload-content-to"></a>创建要将内容上传到其中的媒体服务资产。
# <a name="in-the-v3-api-asset-names-are-unique-arm-resource-identifiers-and-must-be-unique-to-the-account"></a>在 v3 API 中，资产名称是唯一的 ARM 资源标识符，对帐户来说必须是唯一的。
# <a name="its-recommended-to-use-short-unique-ids-or-guids-to-keep-the-names-unique-to-the-account"></a>建议使用短的唯一 ID 或 GUID，使名称对帐户来说始终是唯一的。
az ams asset create \
    -n $assetName \
    -a $amsAccountName \
    -g $resourceGroup \

# <a name="get-the-sas-urls-to-upload-content-to-the-container-for-the-asset"></a>获取 SAS URL，以便将内容上传到资产的容器
# <a name="default-is-23-hour-expiration-but-you-can-adjust-with-the---expiry-flag"></a>默认的到期时间为 23 小时，但可以使用 --expiry 标记来调整。 
# <a name="max-supported-is-24-hours"></a>支持的最大到期时间为 24 小时。 
az ams asset get-sas-urls \
    -n $assetName \
    -a $amsAccountName \
    -g $resourceGroup \
    --expiry  $expiry\
    --permissions ReadWrite \

# <a name="use-the-az-storage-modules-to-upload-a-local-file-to-the-container-using-the-sas-url-from-previous-step"></a>使用 az storage modules 通过上一步的 SAS URL 将本地文件上传到容器。
# <a name="if-you-are-logged-in-already-to-the-subscription-with-access-to-the-storage-account-you-do-not-need-to-use-the---sas-token-at-all-just-eliminate-it-below"></a>如果已登录到订阅并有了存储帐户的访问权限，则根本不需使用 --sas-token。 只需在下面将它消除即可。
# <a name="the-container-name-is-in-the-sas-url-path-and-should-be-set-with-the--c-option"></a>容器名称位于 SAS URL 路径中，应使用 -c 选项进行设置。
# <a name="use-the--f-option-to-point-to-a-local-file-on-your-machine"></a>使用 -f 选项指向计算机上的本地文件。
# <a name="use-the--n-option-to-name-the-blob-in-storage"></a>使用 -n 选项命名存储中的 Blob。
# <a name="use-the---account-name-option-to-point-to-the-storage-account-name-to-use"></a>使用 --account-name 选项指向要使用的存储帐户名称 
# <a name="use-the---sas-token-option-to-place-the-sas-token-after-the-query-string-from-previous-step"></a>使用 --sas-token 选项将 SAS 令牌置于上一步的查询字符串后面。 
# <a name="note-that-the-sas-token-is-only-good-for-up-to-24-hours-max"></a>请注意，SAS 令牌的有效期最长为 24 小时。 

#   <a name="az-storage-blob-upload-"></a>az storage blob upload \
#       <a name="-c-asset-84045780-a71c-4511-801b-711b1a2e76b2-"></a>-c asset-84045780-a71c-4511-801b-711b1a2e76b2 \
#       <a name="-f-cvideosignite-shortmp4-"></a>-f C:\Videos\ignite-short.mp4 \
#       <a name="-n-ignite-shortmp4-"></a>-n ignite-short.mp4 \
#       <a name="--account-name-mconverticlitest0003-"></a>--account-name mconverticlitest0003 \
#       <a name="--sas-token-sv2015-07-08srcsigbvmxdcojr2fop22fyi6lvknc4gcq7fiun5tst8jged7zy3dse2018-04-25t000000zsprwl-"></a>--sas-token "?sv=2015-07-08&sr=c&sig=BvMXDCOjR%2FOP2%2FYi6lVknC4Gcq7fIun5tst8jgED7zY%3D&se=2018-04-25T00:00:00Z&sp=rwl" \

echo "按 [ENTER] 继续。"
read continue
```



