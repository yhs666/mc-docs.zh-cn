---
title: 快速入门 - 使用 Azure CLI 在 Azure 中创建专用 Docker 注册表
description: 快速了解如何使用 Azure CLI 创建专用 Docker 容器注册表。
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: quickstart
origin.date: 03/03/2018
ms.date: 12/17/2018
ms.author: v-yeche
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: d3f14dcfa60bff283f6a6569832538e6d6bc20db
ms.sourcegitcommit: 1db6f261786b4f0364f1bfd51fd2db859d0fc224
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286730"
---
# <a name="quickstart-create-a-container-registry-using-the-azure-cli"></a>快速入门：使用 Azure CLI 创建容器注册表

Azure 容器注册表是托管的 Docker 容器注册表服务，用于存储专用的 Docker 容器映像。 本指南详述了如何使用 Azure CLI 创建 Azure 容器注册表实例，将容器映像推送到注册表中。

<!-- Not Availablle on  finally deploying the container from your registry into Azure Container Instances (ACI)-->

本快速入门需要运行 Azure CLI 2.0.27 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][azure-cli]。

还必须在本地安装 Docker。 Docker 提供的包可在任何 [macOS][docker-mac]、[Windows][docker-windows] 或 [Linux][docker-linux] 系统上轻松配置 Docker。

## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create][az-group-create] 命令创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。

以下示例在“chinanorth”位置创建名为“myResourceGroup”的资源组。

```azurecli
az group create --name myResourceGroup --location chinanorth
```

<!--Notice:  Currently the Container Registry is only valid on ChinaNorth-->

## <a name="create-a-container-registry"></a>创建容器注册表

在本快速入门中，请创建基本注册表。 Azure 容器注册表以多个不同 SKU 提供，下表对此进行了简要说明。 有关每个 SKU 的更多详细信息，请参阅[容器注册表 SKU][container-registry-skus]。

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

使用 [az acr create][az-acr-create] 命令创建 ACR 实例。 注册表名称在 Azure 中必须唯一，并且包含 5-50 个字母数字字符。 以下示例使用 myContainerRegistry007。 将其更新为唯一值。

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

创建注册表时，输出与下面类似：

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "chinanorth",
  "loginServer": "myContainerRegistry007.azurecr.cn",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

<!--Notice:  Currently the Container Registry is only valid on ChinaNorth-->

在本快速入门的剩余部分，`<acrName>` 是容器注册表名称的占位符。

## <a name="log-in-to-acr"></a>登录到 ACR

在推送和拉取容器映像之前，必须登录到 ACR 实例。 为此，请使用 [az acr login][az-acr-login] 命令。

```azurecli
az acr login --name <acrName>
```

该命令在完成后会返回消息 `Login Succeeded`。

## <a name="push-image-to-acr"></a>将映像推送到 ACR

要将映像推送到 Azure 容器注册表，首先必须具有一个映像。 如果尚不具有任何本地容器映像，请运行以下命令，从 Docker 中心拉取现有映像。

```bash
docker pull microsoft/aci-helloworld
```

将映像推送到注册表之前，必须使用 ACR 登录服务器的完全限定的名称进行标记。 运行以下命令，获取 ACR 实例的完整登录服务器名称。

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

使用 [docker tag][docker-tag] 命令标记映像。 使用 ACR 实例的登录服务器名称替换 `<acrLoginServer>`。

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

最后，使用 [docker push][docker-push] 将映像推送到 ACR 实例。 使用 ACR 实例的登录服务器名称替换 `<acrLoginServer>`。

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>列出容器映像

以下示例列出了注册表中的存储库：

```azurecli
az acr repository list --name <acrName> --output table
```

输出：

```bash
Result
----------------
aci-helloworld
```

以下示例列出了 aci-helloworld 存储库中的标记。

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

输出：

```bash
Result
--------
v1
```

<!-- Not Available on ## Deploy image to ACI -->

<!-- Not Available on ## View the application-->

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组、ACR 实例和所有容器映像，可以使用 [az group delete][az-group-delete] 命令将其删除。

```azurecli
az group delete --name myResourceGroup
```

<!-- Not Available on ## Next steps-->
<!-- Not Available on  Continue to the Azure Container Instances tutorial for a deeper look at ACI.-->
<!-- Not Available on  > [Azure Container Instances tutorial][container-instances-tutorial-prepare-app]-->
<!-- IMAGES> -->
<!-- Not Available on [aci-app-browser]: ../container-instances/media/container-instances-quickstart/aci-app-browser.png-->

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: https://docs.azure.cn/zh-cn/cli/acr?view=azure-cli-latest#az-acr-create
[az-acr-login]: https://docs.azure.cn/zh-cn/cli/acr?view=azure-cli-latest#az-acr-login
[az-group-create]: https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create
[az-group-delete]: https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-delete
[azure-cli]: https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest

<!--Not Available on [az-container-show]: https://docs.azure.cn/zh-cn/cli/container?view=azure-cli-latest#az-container-show-->
<!--Not Available on [container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md-->

[container-registry-skus]: container-registry-skus.md

<!-- Not Available on [container-registry-auth-aci]: container-registry-auth-aci.md-->
<!-- Update_Description: wording update, updat meta properties -->