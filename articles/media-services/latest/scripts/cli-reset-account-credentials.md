---
title: Azure CLI 脚本示例 - 重置帐户凭据 | Azure
description: 使用 Azure CLI 脚本重置帐户凭据和恢复 app.config 设置。
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
ms.openlocfilehash: afce5e1cb2d680a79cb4c704ccfe4388f97e6738
ms.sourcegitcommit: d4176361d9c6da60729c06cc93a496cb4702d4c2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35324252"
---
# <a name="cli-example-reset-the-account-credentials"></a>CLI 示例：重置帐户凭据

本文中的 Azure CLI 脚本演示如何重置帐户凭据和恢复 app.config 设置。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="update-the-following-variables-for-your-own-settings"></a>更新自己的设置的下列变量：
resourceGroup=amsResourceGroup amsAccountName=amsmediaaccountname amsSPName=build2018demo

# <a name="reset-your-account-credentials-and-get-the-appconfig-settings-back"></a>重置帐户凭据和恢复 app.config 设置
az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --name $amsSPName \
  --resource-group $resourceGroup \
  --role Owner \
  --xml \
  --years 2 \

echo "按 [ENTER] 继续。"
read continue
```


