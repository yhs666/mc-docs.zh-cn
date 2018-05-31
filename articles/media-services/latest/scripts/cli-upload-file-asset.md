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
ms.openlocfilehash: 3c59d4208c58472cf4fcb12e4633aefa7abec353
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475146"
---
# <a name="cli-example-upload-a-local-file-to-a-container"></a>CLI 示例：将本地文件上传到容器 

本文中的 Azure CLI 脚本演示如何将本地文件上传到存储容器。



如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/upload-file-asset/UploadFile-Asset.sh "Upload a file")]

