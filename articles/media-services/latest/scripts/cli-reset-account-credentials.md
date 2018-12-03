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
ms.openlocfilehash: 78dcd459c3935988c584d7b4e043b709ab7ebda9
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52648981"
---
# <a name="cli-example-reset-the-account-credentials"></a>CLI 示例：重置帐户凭据

本文中的 Azure CLI 脚本演示如何重置帐户凭据和恢复 app.config 设置。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```cli
#!/bin/bash

# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname
amsSPName=build2018demo

# Reset your account credentials and get the app.config settings back
az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --name $amsSPName \
  --resource-group $resourceGroup \
  --role Owner \
  --xml \
  --years 2 \

echo "press  [ENTER]  to continue."
read continue
```


