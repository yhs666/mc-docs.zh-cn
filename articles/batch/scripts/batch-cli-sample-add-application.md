---
title: "Azure CLI 脚本示例 - 在批处理中添加应用程序 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 在批处理中添加应用程序"
services: batch
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 05/02/2017
ms.date: 07/04/2017
ms.author: v-junlch
ms.openlocfilehash: 3992c9406f535049af160cfe08ebd72ac39ff986
ms.sourcegitcommit: 01b8f9a7e857463f49531e70dbb911c6f0286d76
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>使用 Azure CLI 将应用程序添加到 Azure Batch

此脚本演示如何设置要与 Azure Batch 池或任务配合使用的应用程序。 要设置应用程序，请将可执行文件与所有依赖文件一起打包为 .zip 文件。 在此示例中，可执行 zip 文件名为“my-application-exe.zip”。

## <a name="prerequisites"></a>先决条件

- 按照 [Azure CLI 安装指南](/cli/install-azure-cli)中提供的说明安装 Azure CLI（如果尚未这样做）。
- 创建 Batch 帐户（如果还没有帐户）。 有关创建帐户的示例脚本，请参阅[使用 Azure CLI 创建 Batch 帐户](/batch/scripts/batch-cli-sample-create-account/)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Authenticate CLI session.
az login

# Create a new application.
az batch application create \
    --resource-group myresourcegroup \
    --name mybatchaccount \
    --application-id myapp \
    --display-name "My Application"

# An application can reference multiple application executable packages
# of different versions. The executables and any dependencies will need
# to be zipped up for the package. Once uploaded, the CLI will attempt
# to activate the package so that it's ready for use.
az batch application package create \
    --resource-group myresourcegroup \
    --name mybatchaccount \
    --application-id myapp \
    --package-file my-application-exe.zip \
    --version 1.0

# We will update our application to assign the newly added application
# package as the default version.
az batch application set \
    --resource-group myresourcegroup \
    --name mybatchaccount \
    --application-id myapp \
    --default-version 1.0
```

## <a name="clean-up-application"></a>清除应用程序

运行上述示例脚本后，可运行以下命令以删除应用程序及其已上传的应用程序包。

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建应用程序并上传应用程序包。
表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az batch application create](/cli/batch/application#create) | 创建应用程序。  |
| [az batch application set](/cli/batch/application#set) | 更新应用程序的属性。  |
| [az batch application package create](/cli/batch/application/package#create) | 将应用程序包添加到指定的应用程序。  |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure Batch CLI 文档](../batch-cli-samples.md)中找到其他批处理 CLI 脚本示例。

