---
title: 快速入门 - 使用 Azure 门户在 Azure 中创建专用 Docker 注册表
description: 快速了解如何使用 Azure 门户创建专用 Docker 容器注册表。
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: quickstart
origin.date: 11/06/2018
ms.date: 12/17/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: da610eca2c7a6fdb9ed914f408200175d4714a64
ms.sourcegitcommit: 1db6f261786b4f0364f1bfd51fd2db859d0fc224
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286742"
---
# <a name="quickstart-create-a-container-registry-using-the-azure-portal"></a>快速入门：使用 Azure 门户创建容器注册表

Azure 容器注册表是 Azure 中的专用 Docker 注册表，你可在其中存储和管理专用 Docker 容器映像。 在本快速入门中，请使用 Azure 门户创建容器注册表，然后将容器映像推送到注册表中。 

<!--Not Available on and finally deploy the container from your registry into Azure Container Instances (ACI).--> 要完成本快速入门，必须在本地安装 Docker。 Docker 提供的包可在任何 [Mac][docker-mac]、[Windows][docker-windows] 或 [Linux][docker-linux] 系统上轻松配置 Docker。

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.cn 登录到 Azure 门户。

## <a name="create-a-container-registry"></a>创建容器注册表

<!-- Notice:  Customized to match MC--> 选择“创建资源”，在搜索筛选器中键入“容器注册表”，然后单击返回键。

![在 Azure 门户中创建容器注册表][qs-portal-01]

在搜索结果中选择“容器注册表”项。

![在 Azure 门户中创建容器注册表](./media/container-registry-get-started-portal/qs-portal-chenye-click-item.png)

选择“创建” 。 

![在 Azure 门户中创建容器注册表](./media/container-registry-get-started-portal/qs-portal-chenye-click-create.png)

<!-- Notice:  Customized to match MC--> 为“注册表名称”和“资源组”输入值。 注册表名称在 Azure 中必须唯一，并且包含 5-50 个字母数字字符。 对于本快速入门，在 `China North` 位置创建名为 `myResourceGroup` 的新资源组，对于 **SKU**，选择“基本”。 选择“创建”，部署 ACR 实例。

![在 Azure 门户中创建容器注册表][qs-portal-03]

在本快速入门中，我们将创建基本注册表。 Azure 容器注册表以多个不同 SKU 提供，下表对此进行了简要说明。 有关每个 SKU 的更多详细信息，请参阅[容器注册表 SKU][container-registry-skus]。

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

显示“部署成功”消息时，请在门户中选择容器注册表，然后选择“访问密钥”。

![在 Azure 门户中创建容器注册表][qs-portal-05]

在“管理员用户”下，选择“启用”。 记录以下值：

* 登录服务器
* 用户名
* password

使用 Docker CLI 处理注册表时，可在以下步骤中使用这些值。

![在 Azure 门户中创建容器注册表][qs-portal-06]

## <a name="log-in-to-acr"></a>登录到 ACR

在推送和拉取容器映像之前，必须登录到 ACR 实例。 请使用 [docker login][docker-login] 命令完成此操作。 使用在上一步中记录的值替换用户名、密码和登录服务器。

```bash
docker login --username <username> --password <password> <login server>
```

该命令在完成后返回 `Login Succeeded`。 可能会看见建议使用 `--password-stdin` 参数的安全警告。 虽然本文中未介绍它的用法，但我们建议按照此最佳做法进行操作。 有关详细信息，请参阅 [docker login][docker-login] 命令参考。

## <a name="push-image-to-acr"></a>将映像推送到 ACR

要将映像推送到 Azure 容器注册表中，首先必须具有一个映像。 如果需要，请运行以下命令，从 Docker 中心拉取现有映像。

```bash
docker pull microsoft/aci-helloworld
```

将映像推送到注册表之前，必须使用 ACR 登录服务器名称标记映像。 使用 [docker tag][docker-tag] 命令标记映像。 使用之前记录的登录服务器名称替换登录服务器。 添加“存储库名称”，例如 **`myrepo`**，以便将映像放入存储库。

```bash
docker tag microsoft/aci-helloworld <login server>/<repository name>/aci-helloworld:v1
```

最后，使用 [docker push][docker-push] 将映像推送到 ACR 实例。 将“登录服务器”替换为 ACR 实例的登录服务器名称，并将“存储库名称”替换为在上一个命令中使用的存储库名称。

```bash
docker push <login server>/<repository name>/aci-helloworld:v1
```

成功的 `docker push` 命令的输出类似于：

```
The push refers to repository [specificregistryname.azurecr.cn/myrepo/aci-helloworld]
31ba1ebd9cf5: Pushed
cd07853fe8be: Pushed
73f25249687f: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:565dba8ce20ca1a311c2d9485089d7ddc935dd50140510050345a1b0ea4ffa6e size: 1576
```

## <a name="list-container-images"></a>列出容器映像

要列出 ACR 实例中的映像，请在门户中导航到注册表并选择“存储库”，然后选择使用 `docker push` 创建的存储库。

在本示例中，我们选择 aci-helloworld 存储库，并可在“标记”下看到 `v1` 标记的映像。

![在 Azure 门户中创建容器注册表][qs-portal-09]

<!-- Not Available on ## Deploy image to ACI-->
<!-- Notice: Microsoft/Container-Instance is invalid on MC-->
<!-- Not Available on ## View the application-->
## <a name="clean-up-resources"></a>清理资源

若要清理资源，请导航到门户中的 **myResourceGroup** 资源组。 加载资源组以后，请单击“删除资源组”，以便删除资源组、Azure 容器注册表以及所有 Azure 容器实例。

![在 Azure 门户中删除资源组][qs-portal-08]

<!-- Not Availble on ## Next steps-->


<!-- Not Availble on > [Azure Container Instances tutorials][container-instances-tutorial-prepare-app]-->

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-04]: ./media/container-registry-get-started-portal/qs-portal-04.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-06]: ./media/container-registry-get-started-portal/qs-portal-06.png
[qs-portal-07]: ./media/container-registry-get-started-portal/qs-portal-07.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png
[qs-portal-10]: ./media/container-registry-get-started-portal/qs-portal-10.png
[qs-portal-11]: ./media/container-registry-get-started-portal/qs-portal-11.png
[qs-portal-12]: ./media/container-registry-get-started-portal/qs-portal-12.png
[qs-portal-13]: ./media/container-registry-get-started-portal/qs-portal-13.png
[qs-portal-14]: ./media/container-registry-get-started-portal/qs-portal-14.png
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
<!-- Not Availble on [container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md-->

[container-registry-skus]: container-registry-skus.md
<!-- Update_Description: update link, wording update -->