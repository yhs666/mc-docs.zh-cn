---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/04/2018
ms.date: 09/05/2019
ms.author: v-junlch
ms.openlocfilehash: 868381a32ce9e3c0988113993e71e4c427c85881
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805756"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group) 创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源（例如函数应用、数据库和存储帐户）的逻辑容器。

以下示例创建名为 `myResourceGroup` 的资源组。  

```azurecli
az cloud set -n AzureChinaCloud
az login
az group create --name myResourceGroup --location chinanorth
```

通常在附近的区域中创建资源组和资源。 

