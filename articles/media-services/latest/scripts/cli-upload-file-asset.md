---
title: Azure CLI 脚本示例 - 将文件上传到容器 | Azure
description: 使用 Azure CLI 脚本将本地文件上传到存储容器。
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
ms.openlocfilehash: ce46e34c3e61cf514616f988c6b848e4f633b985
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324287"
---
# <a name="cli-example-upload-a-local-file-to-a-container"></a>CLI 示例：将本地文件上传到容器 

本文中的 Azure CLI 脚本演示如何将本地文件上传到存储容器。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="update-the-following-variables-for-your-own-settings"></a>更新自己的设置的下列变量：
storageAccountName=build2018storage assetContainer="asset-4c834446-7e55-4760-9a25-f2d4fb1f4657" localFile="..\Media\ignite-short.mp4" blobName="ignite-short.mp4" assetName="myAsset-uniqueID" sasToken="?sv=2015-07-08&sr=c&sig=u1uy9OIeXnZUEN62hE0bDgg%2FPXYgRDNGnQxE%2BSi51dM%3D&se=2018-04-29T18:42:02Z&sp=rwl"

# <a name="use-the-az-storage-modules-to-upload-a-local-file-to-the-container-using-the-sas-url-from-previous-step"></a>使用 az storage modules 通过上一步的 SAS URL 将本地文件上传到容器。
# <a name="if-you-are-logged-in-already-to-the-subscription-with-access-to-the-storage-account-you-do-not-need-to-use-the---sas-token-at-all-just-eliminate-it-below"></a>如果已登录到订阅并有了存储帐户的访问权限，则根本不需使用 --sas-token。 只需在下面将它消除即可。
# <a name="the-container-name-is-in-the-sas-url-path-and-should-be-set-with-the--c-option"></a>容器名称位于 SAS URL 路径中，应使用 -c 选项进行设置。
# <a name="use-the--f-option-to-point-to-a-local-file-on-your-machine"></a>使用 -f 选项指向计算机上的本地文件。
# <a name="use-the--n-option-to-name-the-blob-in-storage"></a>使用 -n 选项命名存储中的 Blob。
# <a name="use-the---account-name-option-to-point-to-the-storage-account-name-to-use"></a>使用 --account-name 选项指向要使用的存储帐户名称 
# <a name="use-the---sas-token-option-to-place-the-sas-token-after-the-query-string-from-previous-step"></a>使用 --sas-token 选项将 SAS 令牌置于上一步的查询字符串后面。 
# <a name="note-that-the-sas-token-is-only-good-for-up-to-24-hours-max"></a>请注意，SAS 令牌的有效期最长为 24 小时。 
#
az storage blob upload \
    -c $assetContainer \
    -f $localFile \
    -n $blobName \
    --account-name $storageAccountName \
    --sas-token $sasToken \

echo "按 [ENTER] 继续。"
read continue
```

