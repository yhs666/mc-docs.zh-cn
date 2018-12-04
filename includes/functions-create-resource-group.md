---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 11/21/2018
ms.author: v-junlch
ms.openlocfilehash: 170f2d8c8370a3d64f16db6670bdd585eb9ec30a
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673192"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group#az_group_create) 创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源（例如函数应用、数据库和存储帐户）的逻辑容器。

以下示例创建名为 `myResourceGroup` 的资源组。  

```azurecli
az cloud set -n AzureChinaCloud
az login
az group create --name myResourceGroup --location chinanorth
```
通常在附近的区域中创建资源组和资源。 若要查看应用服务计划的所有支持位置，请运行 [az appservice list-locations](/cli/appservice#az-appservice-list-locations) 命令。

<!-- ms.date: 11/21/2018 -->