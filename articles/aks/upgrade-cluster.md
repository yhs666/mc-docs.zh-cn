---
title: 升级 Azure Kubernetes 服务 (AKS) 群集
description: 了解如何升级 Azure Kubernetes 服务 (AKS) 群集
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 05/31/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: 52761e260c0eec44350e20b346b822f3e55f73d9
ms.sourcegitcommit: 84485645f7cc95b8cfb305aa062c0222896ce45d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68731214"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>升级 Azure Kubernetes 服务 (AKS) 群集

在 AKS 群集的生命周期中，经常需要升级到最新的 Kubernetes 版本。 必须应用最新的 Kubernetes 安全版本，或者通过升级来获取最新功能。 本文演示如何在 AKS 群集中升级主组件或单个默认的节点池。

<!--Not Available on multiple node pools or Windows Server nodes-->

## <a name="before-you-begin"></a>准备阶段

本文要求运行 Azure CLI 2.0.65 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][azure-cli-install]。

## <a name="check-for-available-aks-cluster-upgrades"></a>检查是否有可用的 AKS 群集升级

若要检查哪些 Kubernetes 版本可用于群集，请使用 [az aks get-upgrades][az-aks-get-upgrades] 命令。 以下示例在名为 *myResourceGroup* 的资源组中检查是否有可供名为 *myAKSCluster* 的群集使用的升级：

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> 升级 AKS 群集时，不能跳过 Kubernetes 次要版本。 例如，允许从 *1.11.x* 升级到 *1.12.x*，或者从 *1.12.x* 升级到 *1.13.x*，但不允许从 *1.11.x* 升级到 *1.13.x*。
>
> 若要从 *1.11.x* 升级到 *1.13.x*，请先从 *1.11.x* 升级到 *1.12.x*，然后从 *1.12.x* 升级到 *1.13.x*。

以下示例输出表明，群集可以升级到版本 *1.12.7* 或 *1.12.8*：

```console
Name     ResourceGroup    MasterVersion  NodePoolVersion  Upgrades
-------  ---------------  -------------  ---------------  --------------
default  myResourceGroup  1.11.9         1.11.9           1.12.7, 1.12.8
```

## <a name="upgrade-an-aks-cluster"></a>升级 AKS 群集

如果有一系列适用于 AKS 群集的版本，则可使用 [az aks upgrade][az-aks-upgrade] 命令进行升级。 在升级过程中，AKS 将向运行指定 Kubernetes 版本的群集添加一个新节点，然后仔细地一次[隔离并清空][kubernetes-drain]一个旧节点，将对正在运行的应用程序造成的中断情况降到最低。 确认新节点运行应用程序 Pod 以后，就会删除旧节点。 此过程会重复进行，直至群集中的所有节点都已升级完毕。

以下示例将群集升级到版本 *1.12.8*：

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.12.8
```

升级群集需要几分钟时间，具体取决于有多少节点。

若要确认升级是否成功，请使用 [az aks show][az-aks-show] 命令：

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

以下示例输出表明群集现在运行 *1.12.8*：

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  chinaeast2      myResourceGroup  1.12.8               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.chinaeast2.cx.prod.service.azk8s.cn
```

## <a name="next-steps"></a>后续步骤

本文演示了如何升级现有的 AKS 群集。 若要详细了解如何部署和管理 AKS 群集，请参阅相关教程系列。

> [!div class="nextstepaction"]
> [AKS 教程][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest
[az-aks-get-upgrades]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-upgrades
[az-aks-upgrade]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-upgrade
[az-aks-show]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-show

<!--Not Available on [nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool-->