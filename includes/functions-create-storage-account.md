---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 28b65cbb40773cb2e12bab4b3707cf08e88e2800
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034417"
---
## <a name="create-an-azure-storage-account"></a>创建 Azure 存储帐户

Functions 使用通用帐户在 Azure 存储中保留状态以及有关函数的其他信息。 使用 [az storage account create](/cli/storage/account#az-storage-account-create) 命令在创建的资源组中创建通用存储帐户。

在以下命令中，请将 `<storage_name>` 占位符替换成全局唯一存储帐户名。 存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。

```azurecli
az storage account create --name <storage_name> --location chinanorth --resource-group myResourceGroup --sku Standard_LRS
```

