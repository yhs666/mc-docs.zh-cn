---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 569684aff4c33ea9203701bea628e878174a3ffe
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034418"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group#az-group-create) 命令创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源（例如函数应用、数据库和存储帐户）的逻辑容器。

以下示例创建名为 `myResourceGroup` 的资源组。  

```azurecli
az cloud set -n AzureChinaCloud
az login
az group create --name myResourceGroup --location chinanorth
```

通常在附近的[区域](https://azure.microsoft.com/global-infrastructure/regions/)中创建资源组和资源。 

