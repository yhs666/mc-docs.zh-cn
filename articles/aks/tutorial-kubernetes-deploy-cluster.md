---
title: Azure 上的 Kubernetes 教程 - 部署群集
description: 此 Azure Kubernetes 服务 (AKS) 教程介绍如何创建 AKS 群集并使用 kubectl 连接到 Kubernetes 主节点。
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: tutorial
origin.date: 12/19/2018
ms.date: 08/26/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 1f48cd262849e12f985712c516a9358b351d2f3d
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993560"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>教程：部署 Azure Kubernetes 服务 (AKS) 群集

Kubernetes 为容器化应用程序提供一个分布式平台。 使用 AKS 可以快速创建生产就绪的 Kubernetes 群集。 在本教程的第 3 部分（共 7 部分）中，在 AKS 中部署了 Kubernetes 群集。 你将学习如何执行以下操作：

> [!div class="checklist"]
> * 创建用于资源交互的服务主体
> * 部署一个 Kubernetes AKS 群集
> * 安装 Kubernetes CLI (kubectl)
> * 配置 kubectl，以便连接到 AKS 群集

在其他教程中，Azure 投票应用程序将部署到群集，并进行缩放和更新。

## <a name="before-you-begin"></a>准备阶段

在以前的教程中，已创建容器映像并上传到 Azure 容器注册表实例。 如果尚未完成这些步骤，并且想要逐一完成，请先参阅[教程 1 - 创建容器映像][aks-tutorial-prepare-app]。

此教程需要运行 Azure CLI 2.0.53 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][azure-cli-install]。

## <a name="create-a-service-principal"></a>创建服务主体

若要允许 AKS 群集与其他 Azure 资源交互，请使用 Azure Active Directory 服务主体。 可以通过 Azure CLI 或门户自动创建此服务主体，也可以预先创建一个服务主体并分配其他权限。 在本教程中，你将创建一个服务主体，授予对上一教程中创建的 Azure 容器注册表 (ACR) 实例的访问权限，然后创建 AKS 群集。

使用 [az ad sp create-for-rbac][] 命令创建服务主体。 `--skip-assignment` 参数限制分配任何其他权限。 默认情况下，此服务主体的有效期为一年。

```azurecli
az ad sp create-for-rbac --skip-assignment
```

输出类似于以下示例：

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

记下 *appId* 和 *password*。 后续步骤会用到这些值。

## <a name="configure-acr-authentication"></a>配置 ACR 身份验证

若要访问 ACR 中存储的映像，必须授予 AKS 服务主体从 ACR 提取映像的适当权限。

首先，使用 [az acr show][] 获取 ACR 资源 ID。 将 `<acrName>` 注册表名称更新为 ACR 实例和 ACR 实例所在的资源组的名称。

```azurecli
az acr show --resource-group myResourceGroup --name <acrName> --query "id" --output tsv
```

若要为 AKS 群集授予正确的访问权限来拉取 ACR 中存储的映像，请使用 [az role assignment create][] 命令分配 `AcrPull` 角色。 将 `<appId`> 和 `<acrId>` 替换为在前两个步骤中收集的值。

```azurecli
az role assignment create --assignee <appId> --scope <acrId> --role acrpull
```

## <a name="create-a-kubernetes-cluster"></a>创建 Kubernetes 群集

AKS 群集可以使用 Kubernetes 基于角色的访问控制 (RBAC)。 可以使用这些控制根据分配给用户的角色定义资源访问权限。 权限可以组合（如果为用户分配了多个角色），可以局限于单个命名空间，也可以涵盖整个群集。 默认情况下，Azure CLI 会在你创建 AKS 群集时自动启用 RBAC。

使用 [az aks create][] 创建 AKS 群集。 以下示例在名为 *myResourceGroup* 的资源组中创建名为 *myAKSCluster* 的群集。 此资源组是在[上一教程][aks-tutorial-prepare-acr]中创建的。 提供前面在创建服务主体时指定的 `<appId>` 和 `<password>`。

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --service-principal <appId> \
    --client-secret <password> \
    --generate-ssh-keys
```

几分钟后，部署完成并返回有关 AKS 部署的 JSON 格式信息。

## <a name="install-the-kubernetes-cli"></a>安装 Kubernetes CLI

若要从本地计算机连接到 Kubernetes 群集，请使用 [kubectl][kubectl]（Kubernetes 命令行客户端）。

<!--MOONCAKE Unique content 03/28/2019-->
<!--Sync with 3. Install kubectl in https://github.com/Azure/container-service-for-azure-china/tree/master/aks--> 

如果使用 Azure 本地 Shell，也可使用 [az aks install-cli][] 命令在本地安装它：

```azurecli
az aks install-cli --install-location <kubectl-download-path>
```

> [!NOTE]
> 原始的 `az aks install-cli` 命令在 Azure 中国区无效，详见[此文](https://mirror.azk8s.cn/help/kubernetes.html)。

* 可以通过 PR [添加适用于 Azure 中国区的“az aks install-cli”支持](https://github.com/Azure/azure-cli/pull/8675)来修复此问题。以下命令会在 Linux 上启动容器化的 azure-cli(`dockerhub.azk8s.cn/andyzhangx/azure-cli:v2.0.60-china`) 将最新的 `kubectl` 版本下载到 `/usr/local/bin/`：
    
    ```
    # docker run -v ${HOME}:/root -v /usr/local/bin/:/kube -it dockerhub.azk8s.cn/andyzhangx/azure-cli:v2.0.60-china
    root@09feb993f352:/# az cloud set --name AzureChinaCloud
    root@09feb993f352:/# az aks install-cli --install-location /kube/kubectl
    ```

<!--MOONCAKE Unique content 03/28/2019-->

## <a name="connect-to-cluster-using-kubectl"></a>使用 kubectl 连接到群集

若要将 `kubectl` 配置为连接到 Kubernetes 群集，请使用 [az aks get-credentials][] 命令。 以下示例获取 myResourceGroup  中名为“myAKSCluster”  的 AKS 群集的凭据：

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

若要验证与群集之间的连接，请运行 [kubectl get nodes][kubectl-get] 命令：

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-12345678-0   Ready    agent   32m   v1.13.9
```

## <a name="next-steps"></a>后续步骤

本教程在 AKS 中部署了一个 Kubernetes 群集并将 `kubectl` 配置为连接到该群集。 你已了解如何：

> [!div class="checklist"]
> * 创建用于资源交互的服务主体
> * 部署一个 Kubernetes AKS 群集
> * 安装 Kubernetes CLI (kubectl)
> * 配置 kubectl，以便连接到 AKS 群集

请继续学习下一教程，了解如何将应用程序部署到群集。

> [!div class="nextstepaction"]
> [在 Kubernetes 中部署应用程序][aks-tutorial-deploy-app]

<!-- LINKS - external -->

[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->

[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: https://docs.azure.cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az acr show]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-show
[az role assignment create]: https://docs.azure.cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-create
[az aks create]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
[az aks install-cli]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az aks get-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[azure-cli-install]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest

<!-- Update_Description: wording update, update link -->