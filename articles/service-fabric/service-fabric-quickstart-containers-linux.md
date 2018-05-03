---
title: 在 Linux 上创建 Azure Service Fabric 容器应用程序 | Azure
description: 在本快速入门中，请在 Azure Service Fabric 上创建第一个 Linux 容器应用程序。  生成包含应用程序的 Docker 映像，将该映像推送到容器注册表，并生成并部署 Service Fabric 容器应用程序。
services: service-fabric
documentationcenter: linux
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 04/11/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: b86a29537f0ee64a6e007d779a70ef7e2fd77420
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>快速入门：在 Azure 上部署 Azure Service Fabric Linux 容器应用程序
Azure Service Fabric 是一款分布式系统平台，可用于部署和管理可缩放的可靠微服务和容器。 

本快速入门介绍如何将 Linux 容器部署到 Service Fabric 群集。 完成后，Service Fabric 群集中会运行一个由 Python Web 前端和 Redis 后端组成的 Voting 应用程序。 此外还介绍如何对群集中的应用程序进行故障转移和缩放。

![Voting 应用网页][quickstartpic]

在本快速入门中，请在本地 Shell 中使用 Bash 环境来运行 Service Fabric CLI 命令。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

## <a name="get-the-application-package"></a>获取应用程序包
若要将容器部署到 Service Fabric，需要一组描述各个容器以及应用程序的清单文件（应用程序定义）。

在本地 Shell 中，使用 git 克隆一份应用程序定义，然后将目录更改为克隆中的 `Voting` 目录。

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```

## <a name="create-a-service-fabric-cluster"></a>创建 Service Fabric 群集
若要将应用程序部署到 Azure，需要通过 Service Fabric 群集来运行该应用程序。 该群集使用单个自签名证书来确保节点到节点和客户端到节点的安全。

<!-- Not Avaiable on Party cluster content -->

若要了解如何创建自己的群集，请参阅[在 Azure 上创建 Service Fabric 群集](service-fabric-tutorial-create-vnet-and-linux-cluster.md)。

> [!Note]
>如果创建自己的群集，请注意，Web 前端服务配置为侦听端口 80 上是否有传入流量。 请确保此端口在群集中处于打开状态。
>

## <a name="configure-your-environment"></a>配置环境
Service Fabric 提供多种可以用来管理群集及其应用程序的工具：

- Service Fabric Explorer，一种基于浏览器的工具。
- Service Fabric 命令行界面 (CLI)，在 Azure CLI 2.0 基础上运行。
- PowerShell 命令。 

在本快速入门中，请使用 Service Fabric Explorer。

<!-- Not Available on ### Configure certificate for the Service Fabric CLI -->

## <a name="deploy-the-service-fabric-application"></a>部署 Service Fabric 应用程序 
1. 在本地 Shell 中，使用 CLI 连接到 Azure 中的 Service Fabric 群集。

```bash
sfctl cluster select --endpoint https://linh1x87d1d.chinanorth.cloudapp.chinacloudapi.cn:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
```


2. 使用安装脚本将 Voting 应用程序定义复制到群集，注册应用程序类型，并创建应用程序的实例。

    ```bash
    ./install.sh
    ```

2. 打开 Web 浏览器，导航到群集的 Service Fabric Explorer 终结点。 终结点的格式如下：https://\<my-azure-service-fabric-cluster-url>:19080/Explorer，例如 `https://linh1x87d1d.chinanorth.cloudapp.chinacloudapi.cn:19080/Explorer`。 </br>

3. 展开“应用程序”节点，可以看到 Voting 应用程序类型的条目以及创建的实例。

    ![Service Fabric Explorer][sfx]

3. 若要连接到正在运行的容器，请打开 Web 浏览器，导航到群集的 URL，例如 `http://linh1x87d1d.chinanorth.cloudapp.chinacloudapi.cn:80`。 浏览器中应会显示该投票应用程序。

    ![Voting 应用网页][quickstartpic]

> [!NOTE]
> 也可使用 Docker Compose 来部署 Service Fabric 应用程序。 例如，可以使用 Docker Compose 通过以下命令在群集上部署和安装应用程序。
>  ```bash
> sfctl compose create --deployment-name TestApp --file-path ../docker-compose.yml
> ```

## <a name="fail-over-a-container-in-a-cluster"></a>故障转移群集中的容器
Service Fabric 可确保在发生故障时，将容器实例自动转移到群集中的其他节点。 也可以手动清空容器的节点，然后将其正常转移到群集中的其他节点。 Service Fabric 提供多种缩放服务的方式。 在以下步骤中，请使用 Service Fabric Explorer。

若要故障转移前端容器，请执行以下步骤：

1. 在群集中打开 Service Fabric Explorer，例如 `https://linh1x87d1d.chinanorth.cloudapp.chinacloudapi.cn:19080/Explorer`。
2. 在树视图中单击“fabric:/Voting/azurevotefront”节点，展开分区节点（以 GUID 表示）。 注意树视图中的节点名称，它显示了当前正在运行容器的节点，例如 `_nodetype_4`。
3. 在树视图中展开“节点”节点。 单击正在运行容器的节点旁边的省略号 (...)。
4. 选择“重启”以重启该节点，并确认重启操作。 重启会导致容器故障转移到群集中的另一个节点。

    ![Service Fabric Explorer 中的“节点”视图][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>在群集中缩放应用程序和服务
可以跨群集轻松缩放 Service Fabric 服务，以适应服务上的负载。 可以通过更改群集中运行的实例数量来缩放服务。

若要缩放 Web 前端服务，请按照以下步骤操作：

1. 在群集中打开 Service Fabric Explorer，例如 `https://linh1x87d1d.chinanorth.cloudapp.chinacloudapi.cn:19080`。
2. 在树视图中单击“fabric:/Voting/azurevotefront”节点旁边的省略号（三个点），选择“缩放服务”。

    ![Service Fabric Explorer 缩放服务开始][containersquickstartscale]

  现在可以缩放 Web 前端服务的实例数量。

3. 将数字更改为 2，再单击“缩放服务”。
4. 在树视图中单击“fabric:/Voting/azurevotefront”节点，展开分区节点（以 GUID 表示）。

    ![Service Fabric Explorer 缩放服务完成][containersquickstartscaledone]

    现在可以看到该服务有两个实例。 在树视图中，可以看到运行实例的节点。

通过这一简单的管理任务，你已让前端服务用来处理用户负载的资源数量翻了一番。 有必要了解的是，服务无需多个实例便能可靠运行。 如果服务出现故障，Service Fabric 可确保在群集中运行新的服务实例。

## <a name="clean-up-resources"></a>清理资源
1. 使用模板中提供的卸载脚本 (uninstall.sh) 从群集中删除应用程序实例并取消注册应用程序类型。 此脚本需要一定的时间来清理实例，因此不应在运行此脚本后立即运行 install script。 可以使用 Service Fabric Explorer 来确定实例何时已删除以及应用程序类型何时已取消注册。 

```bash
./uninstall.sh
```

2. 如果群集已使用完毕，则可从证书存储中删除证书。 例如：
   - 在 Windows 上：使用[“证书”MMC 管理单元](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in?view=azure-dotnet)。 在添加管理单元时，确保选择“我的用户帐户”。 导航到 `Certificates - Current User\Personal\Certificates`，然后删除证书。
   - 在 Mac 上：使用 Keychain 应用。
   - 在 Ubuntu 上：按照查看证书时所使用的步骤删除此证书。
## <a name="next-steps"></a>后续步骤

<!-- Not Availabel on [Create a Linux container app](./service-fabric-tutorial-create-container-images.md)-->

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png

<!--Update_Description: update meta properties, wording update -->