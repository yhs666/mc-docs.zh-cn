---
title: 快速入门 - 使用 PowerShell 在 Azure 中创建专用 Docker 注册表
description: 快速了解如何使用 PowerShell 在 Azure 中创建专用 Docker 容器注册表。
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: quickstart
origin.date: 05/08/2018
ms.date: 11/12/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: afef09b9baedd090d5fdea98bcb9cf50179e9849
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52656707"
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>教程：使用 PowerShell 创建 Azure 容器注册表

Azure 容器注册表是托管的专用 Docker 容器注册表服务，用于生成、存储和提供 Docker 容器映像。 本快速入门介绍如何使用 PowerShell 创建 Azure 容器注册表。 创建注册表以后，向其推送容器映像。
<!-- Not Available on  then deploy the container from your registry into Azure Container Instances (ACI)-->

## <a name="prerequisites"></a>先决条件

本快速入门需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可确定已安装的版本。 如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

还必须在本地安装 Docker。 Docker 提供的包适用于 [macOS][docker-mac]、[Windows][docker-windows] 和 [Linux][docker-linux] 系统。

由于 Azure Cloud Shell 并不包括所有必需的 Docker 组件（`dockerd` 守护程序），因此不能将 Cloud Shell 用于本快速入门。

## <a name="sign-in-to-azure"></a>登录 Azure

使用 [Connect-AzureRmAccount][Connect-AzureRmAccount] 命令登录到 Azure 订阅，然后按屏幕说明操作。

```powershell
Connect-AzureRmAccount -Environment AzureChinaCloud
```

## <a name="create-resource-group"></a>创建资源组

通过 Azure 进行身份验证以后，请使用 [New-AzureRmResourceGroup][New-AzureRmResourceGroup] 创建资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location ChinaEast
```

## <a name="create-container-registry"></a>创建容器注册表

接下来，请使用 [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry] 命令在新的资源组中创建容器注册表。

注册表名称在 Azure 中必须唯一，并且包含 5-50 个字母数字字符。 以下示例创建名为“myContainerRegistry007”的注册表。 替换以下命令中的 *myContainerRegistry007*，然后运行该命令以创建注册表：

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-registry"></a>登录到注册表

在推送和拉取容器映像之前，必须登录到注册表。 使用 [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential] 命令先获取注册表的管理员凭据：

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

接下来，运行用于登录的 [docker login][docker-login] 命令：

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

登录成功会返回`Login Succeeded`：

```console
PS Azure:\> $creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
Login Succeeded
```

## <a name="push-image-to-registry"></a>将映像推送到注册表

登录到注册表以后，即可向其推送容器映像。 若要获取可以推送到注册表的映像，请从 Docker 中心拉取公共的 [aci-helloworld][aci-helloworld-image] 映像。 [aci-helloworld][aci-helloworld-github] 映像是一个小型 Node.js 应用程序，其提供的静态 HTML 页面显示 Azure 容器实例徽标。

```powershell
docker pull microsoft/aci-helloworld
```

拉取映像时，输出类似于：

```console
PS Azure:\> docker pull microsoft/aci-helloworld
Using default tag: latest
latest: Pulling from microsoft/aci-helloworld
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
3be0618266da: Pull complete
9e232827e52f: Pull complete
b53c152f538f: Pull complete
Digest: sha256:a3b2eb140e6881ca2c4df4d9c97bedda7468a5c17240d7c5d30a32850a2bc573
Status: Downloaded newer image for microsoft/aci-helloworld:latest
```

将映像推送到 Azure 容器注册表之前，必须使用注册表的完全限定域名对其进行标记。 Azure 容器注册表的 FQDN 采用 *\<registry-name\>.azurecr.cn* 格式。

在变量中填充完整的映像标记。 包括登录服务器、存储库名称（“aci-helloworld”）和映像版本（“v1”）：

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
```

现在，请使用 [docker tag][docker-tag] 来标记映像：

```powershell
docker tag microsoft/aci-helloworld $image
```

最后，请使用 [docker push][docker-push] 将其推送到注册表：

```powershell
docker push $image
```

Docker 客户端推送映像时，输出应类似于：

```console
PS Azure:\> docker push $image
The push refers to repository [myContainerRegistry007.azurecr.cn/aci-helloworld]
31ba1ebd9cf5: Pushed
cd07853fe8be: Pushed
73f25249687f: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:565dba8ce20ca1a311c2d9485089d7ddc935dd50140510050345a1b0ea4ffa6e size: 1576
```

祝贺！ 你已将第一个容器映像推送到注册表。

<!--Not Available on ## Deploy image to ACI -->


<!--Not Available on ## View running application-->

## <a name="clean-up-resources"></a>清理资源

使用完在本快速入门中创建的资源以后，请通过 [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] 命令删除资源组和容器注册表：<!-- Not Available on  and the container instance-->

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

<!-- Not Available on ## Next steps-->
<!-- Not Available on > [Azure Container Instances tutorial](../container-instances/container-instances-tutorial-prepare-app.md)-->

<!-- LINKS - external -->
[aci-helloworld-github]: https://github.com/Azure-Samples/aci-helloworld
[aci-helloworld-image]: https://hub.docker.com/r/microsoft/aci-helloworld/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerGroup]: https://docs.microsoft.com/powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Get-AzureRmContainerRegistryCredential]: https://docs.microsoft.com/powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-module
[New-AzureRmContainerGroup]: https://docs.microsoft.com/powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[New-AzureRMContainerRegistry]: https://docs.microsoft.com/powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-psh-01-running-app]: ./media/container-registry-get-started-powershell/qs-psh-01-running-app.png

<!-- Update_Description: wording update, update meta properties -->