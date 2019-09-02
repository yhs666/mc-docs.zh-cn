---
title: Azure CLI 脚本示例 - 创建 Azure 媒体服务帐户 | Azure
description: 使用 Azure CLI 脚本创建 Azure 媒体服务帐户。
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
ms.openlocfilehash: c977be3dbdefd632c24cce4307b9d64ff05ace06
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52644945"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>CLI 示例：创建 Azure 媒体服务帐户

本主题中的 Azure CLI 脚本演示如何创建 Azure 媒体服务帐户。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```cli
#!/bin/bash

# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
storageName=amsstorename
amsAccountName=amsmediaaccountname
amsSPName=mediaserviceprincipal
amsSPPassword=mediasppassword

# Create a resource resourceGroupName
az group create \
  --name $resourceGroup \
  --location chinaeast

# Create an azure storage account, General Purpose v2, Standard RAGRS
az storage account create \
  --name $storageName \
  --kind StorageV2 \
  --sku Standard_RAGRS \
  --location chinaeast \
  --resource-group $resourceGroup

# Create an azure media service account
az ams account create \
  --name $amsAccountName \
  --resource-group $resourceGroup \
  --storage-account $storageName \
  --location chinaeast

# Create a service principal with password and configure its access to an Azure Media Services account.
az ams account sp create \
  --account-name $amsAccountName \
  --name $amsSPName \
  --resource-group $resourceGroup \
  --role Owner \
  --xml \
  --years 2 \

echo "press  [ENTER]  to continue."
read continue
```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令以删除资源组及其相关的所有资源。

```azurecli
az group delete --name amsResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az storage account create](/cli/storage/account#az_storage_account_create) | 创建存储帐户。 |
| [az ams account create](/cli/azure/ams/account?view=azure-cli-latest#az-ams-account-create) | 创建媒体服务帐户。 |
| [az ams account sp create](/cli/azure/ams/account/sp?view=azure-cli-latest#az-ams-account-sp-create) | 通过密码创建服务主体并配置其对 Azure 媒体服务帐户的访问权限。 
| [az group delete](/cli/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |


