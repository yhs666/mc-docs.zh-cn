---
title: "Azure CLI 脚本示例 - 使用批处理运行作业 | Microsoft Docs"
description: "Azure CLI 脚本示例 - 使用批处理运行作业"
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
ms.openlocfilehash: 0c7713daca2642bccc2f0bb3abdc8b395715d86b
ms.sourcegitcommit: 01b8f9a7e857463f49531e70dbb911c6f0286d76
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>使用 Azure CLI 在 Azure Batch 上运行作业

此脚本创建一个批处理作业，并将一系列任务添加到该作业。 它还演示了如何监视作业及其任务。 最后，它演示如何有效地查询 Batch 服务，以获取有关作业任务的信息。

## <a name="prerequisites"></a>先决条件

- 按照 [Azure CLI 安装指南](/cli/install-azure-cli)中提供的说明安装 Azure CLI（如果尚未这样做）。
- 创建 Batch 帐户（如果还没有帐户）。 有关创建帐户的示例脚本，请参阅[使用 Azure CLI 创建 Batch 帐户](/batch/scripts/batch-cli-sample-create-account/)。
- 将应用程序配置为从启动任务运行（如果尚未这样做）。 有关用于创建应用程序并将应用程序包上传到 Azure 的示例脚本，请参阅[使用 Azure CLI 将应用程序添加到 Azure Batch](/batch/scripts/batch-cli-sample-add-application/)。
- 配置将在其中运行作业的池。 有关使用云服务配置或虚拟机配置创建池的示例脚本，请参阅[使用 Azure CLI 管理 Azure Batch 池](/batch/scripts/batch-cli-sample-manage-pool/)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Authenticate Batch account CLI session.
az batch account login -g myresource group -n mybatchaccount

# Create a new job to encapsulate the tasks that we want to add.
# We'll assume a pool has already been created with the ID 'mypool' - for more information
# see the sample script for managing pools.
az batch job create --id myjob --pool-id mypool

# Now we will add tasks to the job.
# We'll assume an application package has already been uploaded with the ID 'myapp' - for
# more information see the sample script for adding applications.
az batch task create \
    --job-id myjob \
    --task-id task1 \
    --application-package-references myapp#1.0
    --command-line "cmd /c %AZ_BATCH_APP_PACKAGE_MYAPP#1.0%\\myapp.exe"

# If we want to add many tasks at once - this can be done by specifying the tasks
# in a JSON file, and passing it into the command. See tasks.json for formatting.
az batch task create --job-id myjob --json-file tasks.json

# Now that all the tasks are added - we can update the job so that it will automatically
# be marked as completed once all the tasks are finished.
az batch job set --job-id myjob --on-all-tasks-complete terminateJob

# Monitor the status of the job.
az batch job show --job-id myjob

# Monitor the status of a task.
az batch task show --job-id myjob --task-id task1
```

## <a name="clean-up-job"></a>清除作业

运行上述示例脚本后，可运行以下命令以删除作业及其所有任务。 请注意，池将需要单独删除。 有关创建和删除池的详细信息，请参阅[使用 Azure CLI 管理 Azure Batch 池](./batch-cli-sample-manage-pool.md)。

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建批处理作业和任务。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az batch account login](/cli/batch/account#login) | 针对批处理帐户进行身份验证。  |
| [az batch job create](/cli/batch/job#create) | 创建批处理作业。  |
| [az batch job set](/cli/batch/job#set) | 更新批处理作业的属性。  |
| [az batch job show](/cli/batch/job#show) | 检索指定批处理作业的详细信息。  |
| [az batch task create](/cli/batch/task#create) | 将任务添加到指定的批处理作业。  |
| [az batch task show](/cli/batch/task#show) | 从指定的批处理作业中检索任务的详细信息。  |
| [az batch task list](/cli/batch/task#list) | 列出与指定的作业关联的任务。  |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview)。

可以在 [Azure Batch CLI 文档](../batch-cli-samples.md)中找到其他批处理 CLI 脚本示例。

