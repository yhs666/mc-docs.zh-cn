---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 09/20/2018
ms.date: 05/20/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: e4701bb37cf46d3e6a89c6a1ddf4b717b6e96c99
ms.sourcegitcommit: 878a2d65e042b466c083d3ede1ab0988916eaa3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835835"
---
<!--Verify Successfully-->
## <a name="using-rbac-to-share-images"></a>使用 RBAC 共享映像

可以使用基于角色的访问控制 (RBAC) 在订阅之间共享映像。 任何对映像版本具有读取权限的用户，即使跨订阅，也能够使用映像版本部署虚拟机。

有关如何使用 RBAC 共享资源的详细信息，请参阅[使用 RBAC 和 Azure CLI 管理访问权限](/role-based-access-control/role-assignments-cli)。

## <a name="list-information"></a>列出信息

使用 [az sig list](https://docs.azure.cn/zh-cn/cli/sig?view=azure-cli-latest#az-sig-list) 获取有关可用映像库的位置、状态和其他信息。

```azurecli 
az sig list -o table
```

使用 [az sig image-definition list](https://docs.azure.cn/zh-cn/cli/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-list) 列出库中的映像定义，包括有关 OS 类型和状态的信息。

```azurecli 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

使用 [az sig image-version list](https://docs.azure.cn/zh-cn/cli/sig/image-version?view=azure-cli-latest#az-sig-image-version-list) 列出库中的共享映像版本。

```azurecli
az sig image-version list -g myGalleryRG -r myGallery -i myImageDefinition -o table
```

使用 [az sig image-version show](https://docs.azure.cn/zh-cn/cli/sig/image-version?view=azure-cli-latest#az-sig-image-version-show) 获取映像版本的 ID。

```azurecli
az sig image-version show \
   -g myGalleryRG \
   -r myGallery \
   -i myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```

<!--Update_Description: new articles on VM common gallery list cli -->
<!--ms.date: 05/20/2019-->