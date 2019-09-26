---
title: Azure CLI 脚本示例 - 重置帐户凭据 | Microsoft Docs
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
origin.date: 08/20/2019
ms.date: 09/23/2019
ms.author: v-jay
ms.openlocfilehash: 084ad709e5dafc0c9fac08dc280ce96ed77b2c3c
ms.sourcegitcommit: 8248259e4c3947aa0658ad6c28f54988a8aeebf8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71125640"
---
# <a name="cli-example-reset-the-account-credentials"></a>CLI 示例：重置帐户凭据

本文中的 Azure CLI 脚本演示如何重置帐户凭据和恢复 app.config 设置。

## <a name="prerequisites"></a>先决条件 

[创建媒体服务帐户](create-account-cli-how-to.md)。

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>示例脚本

```
# Update the following variables for your own settings:
$resourceGroup=amsResourceGroup
$amsAccountName=amsmediaaccountname

az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --resource-group $resourceGroup 
```

## <a name="next-steps"></a>后续步骤

* [az ams](/cli/ams?view=azure-cli-latest)
* [重置凭据](/cli/ams/account/sp?view=azure-cli-latest#az-ams-account-sp-reset-credentials)
