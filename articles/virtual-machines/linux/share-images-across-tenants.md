---
title: 在 Azure 中跨租户共享库映像 | Azure
description: 了解如何使用共享映像库跨 Azure 租户共享 VM 映像。
services: virtual-machines-linux
author: rockboyfor
manager: digimobile
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
origin.date: 04/05/2019
ms.date: 07/01/2019
ms.author: v-yeche
ms.openlocfilehash: 38ff1c504b511013193d9f66d067da77b9a9b41b
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569668"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>跨 Azure 租户共享库 VM 映像

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

## <a name="create-a-vm-using-azure-cli"></a>使用 Azure CLI 创建 VM

使用租户 1 的 appID、应用密钥以及 ID 登录到租户 1 的服务主体。 可以根据需要使用 `az account show --query "tenantId"` 获取租户 ID。

```azurecli
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```

使用租户 2 的 appID、应用密钥以及 ID 登录到租户 2 的服务主体：

```azurecli
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

创建 VM。 请将示例中的信息替换为你自己的。

```azurecli
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

<!--Not Available on ## Next steps-->

<!--Not Available on [troubleshoot shared image galleries](troubleshooting-shared-images.md)-->

<!-- Update_Description: new article about share images across tenants -->
<!--ms.date: 05/20/2019-->