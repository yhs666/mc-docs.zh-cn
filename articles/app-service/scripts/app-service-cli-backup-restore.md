---
title: Azure CLI 脚本示例 - 从备份中还原 Web 应用 | Microsoft Docs
description: Azure CLI 脚本示例 - 从备份中还原 Web 应用
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 12/07/2017
ms.date: 01/02/2017
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: dd20ace8c5317e6b2abca9fe61dd85f330a887bc
ms.sourcegitcommit: 51f9fe7a93207e6b9d61e09b7abf56a7774ee856
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2017
ms.locfileid: "27546623"
---
# <a name="restore-a-web-app-from-a-backup"></a>从备份中还原 Web 应用

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，然后为其创建一次性备份。 

若要运行此脚本，需要 Web 应用的现有备份。 若要创建备份，请参阅[备份 Web 应用](app-service-cli-backup-onetime.md)或[创建 Web 应用的计划备份](app-service-cli-backup-scheduled.md)。



如果选择在本地安装并使用 CLI，则需要使用 Azure CLI 2.0 版或更高版本。 若要查找版本，请运行 `az --version`。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

groupname="myResourceGroup"
webappname="<replace-with-your-app-name>"

# List statuses of all backups that are complete or currently executing.
az webapp config backup list --resource-group $groupname --webapp-name $webappname

# Note the backupItemName and storageAccountUrl properties of the backup you want to restore

# Restore the app by overwriting it with the backup data
# Be sure to replace <backupItemName> and <storageAccountUrl>
az webapp config backup restore --resource-group $groupname --webapp-name $webappname \
--backup-name <backupItemName> --container-url <storageAccountUrl> --overwrite
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az webapp config backup list`](https://docs.azure.cn/zh-cn/cli/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | 获取 Web 应用的备份列表。 |
| [`az webapp config backup restore`](https://docs.azure.cn/zh-cn/cli/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_restore) | 从备份中还原 Web 应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../app-service-cli-samples.md)中找到其他应用服务 CLI 脚本示例。
