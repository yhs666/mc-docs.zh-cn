---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 03/04/2019
ms.author: v-junlch
ms.openlocfilehash: d85b8a1ca29e030378a3a521aa37c552e8a22d46
ms.sourcegitcommit: 115087334f6170fb56c7925a8394747b07030755
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2019
ms.locfileid: "57254125"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group) 创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源（例如函数应用、数据库和存储帐户）的逻辑容器。

以下示例创建名为 `myResourceGroup` 的资源组。  

```azurecli
az cloud set -n AzureChinaCloud
az login
az group create --name myResourceGroup --location chinanorth
```
通常在附近的区域中创建资源组和资源。 若要查看应用服务计划的所有支持位置，请运行 [az appservice list-locations](/cli/appservice#az-appservice-list-locations) 命令。

<!-- ms.date: 03/04/2019 -->