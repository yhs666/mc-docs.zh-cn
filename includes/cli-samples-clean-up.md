---
author: cephalin
ms.service: app-service-web
ms.topic: include
origin.date: 11/21/2018
ms.author: v-biyu
ms.date: 06/17/2019
ms.openlocfilehash: d2f0c5deda44e91aef93f0a053c15a2f70398c3d
ms.sourcegitcommit: d7db02d1b62c7b4deebd5989be97326b4425d1d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687451"
---
## <a name="clean-up-resources"></a>清理资源

在前面的步骤中，在资源组中创建了 Azure 资源。 如果认为将来不需要这些资源，请在 Cloud Shell 中运行以下命令删除资源组：

```azurecli
az group delete --name myResourceGroup
```

此命令可能需要花费一分钟时间运行。