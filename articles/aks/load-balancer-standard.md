---
title: 在 Azure Kubernetes 服务 (AKS) 中使用标准 SKU 负载均衡器
description: 了解如何在 Azure Kubernetes 服务 (AKS) 中使用标准 SKU 负载均衡器来公开服务。
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 09/27/2019
ms.date: 10/17/2019
ms.author: v-yeche
ms.openlocfilehash: 40e0214304db21225bfcb530ef90ac150f2f3a26
ms.sourcegitcommit: 4ada17c1bcd36e755afd0a8bd6e353e35cbb228b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578067"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>在 Azure Kubernetes 服务 (AKS) 中使用标准 SKU 负载均衡器

若要在 Azure Kubernetes 服务 (AKS) 中提供对应用程序的访问权限，可以创建并使用 Azure 负载均衡器。 在 AKS 中运行的负载均衡器可用作内部或外部负载均衡器。 内部负载均衡器使得仅 AKS 群集所在的同一虚拟网络中运行的应用程序能够访问 Kubernetes 服务。 外部负载均衡器接收入口的一个或多个公共 IP，并使得 Kubernetes 服务可以通过公共 IP 在外部进行访问。

Azure 负载均衡器以两种 SKU 提供：“基本”和“标准”   。 默认情况下，使用服务清单在 AKS 上创建负载均衡器时，将使用基本  SKU。 使用标准 SKU 负载均衡器可提供其他特性和功能，例如更大的后端池和可用性区域。  在选择使用标准或基本负载均衡器之前，必须了解两者之间的差异。   创建 AKS 群集后，无法更改该群集的负载均衡器 SKU。 有关基本和标准 SKU 的详细信息，请参阅 [Azure 负载均衡器 SKU 的比较][azure-lb-comparison]。  

本文介绍如何在 Azure Kubernetes 服务 (AKS) 中创建和使用标准 SKU Azure 负载均衡器。 

本文假设读者基本了解 Kubernetes 和 Azure 负载均衡器的概念。 有关详细信息，请参阅 [Azure Kubernetes 服务 (AKS) 的 Kubernetes 核心概念][kubernetes-concepts]和[什么是 Azure 负载均衡器？][azure-lb]。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.74 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][install-azure-cli]。

## <a name="before-you-begin"></a>准备阶段

如果使用现有子网或资源组，则 AKS 群集服务主体需要管理网络资源的权限。 通常，将“网络参与者”  角色分配给委派资源上的服务主体。 有关权限的详细信息，请参阅[委派 AKS 访问其他 Azure 资源][aks-sp]。

必须创建一个 AKS 群集，以将负载均衡器的 SKU 设置为“标准”而不是默认的“基本”。  

### <a name="limitations"></a>限制

创建和管理支持标准 SKU 负载均衡器的 AKS 群集时存在以下限制： 

* 至少需要指定一个公共 IP 或 IP 前缀来允许 AKS 群集的出口流量。 此外，需要使用公共 IP 或 IP 前缀来保持控制平面与代理节点之间的连接，以及保持与旧版 AKS 的兼容性。 可以使用以下选项指定标准 SKU 负载均衡器的公共 IP 或 IP 前缀： 
    * 提供自己的公共 IP。
    * 提供自己的公共 IP 前缀。
    * 指定最大为 100 的数字，以允许 AKS 群集在其所在的同一个资源组（名称通常以 *MC_* 开头）中创建多个标准 SKU 公共 IP。  AKS 会将公共 IP 分配到标准 SKU 负载均衡器。  默认情况下，如果未指定公共 IP、公共 IP 前缀或 IP 数目，系统会在 AKS 群集所在的同一个资源组中自动创建一个公共 IP。 此外，必须允许公共地址，并避免创建任何会阻止创建 IP 的 Azure 策略。
* 对负载均衡器使用标准 SKU时，必须使用 Kubernetes 1.13 或更高版本。 
* 只能在创建 AKS 群集时定义负载均衡器 SKU。 创建 AKS 群集后，无法更改负载均衡器 SKU。
* 在一个群集中只能使用一个负载均衡器 SKU。

## <a name="create-a-resource-group"></a>创建资源组

Azure 资源组是在其中部署和管理 Azure 资源的逻辑组。 创建资源组时，系统会要求你指定一个位置， 此位置是资源组元数据的存储位置，如果你在创建资源期间未指定另一个区域，则它还是你的资源在 Azure 中的运行位置。 使用 [az group create][az-group-create] 命令创建资源组。

以下示例在“chinaeast2”  位置创建名为“myResourceGroup”  的资源组。

```azurecli
az group create --name myResourceGroup --location chinaeast2
```

以下示例输出显示已成功创建资源组：

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "chinaeast2",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>创建 AKS 群集
若要运行支持标准 SKU 负载均衡器的 AKS 群集，该群集需将 *load-balancer-sku* 参数设置为 *standard*。  创建群集时，此参数将创建使用标准 SKU 的负载均衡器。  在群集上运行 *LoadBalancer* 服务时，将使用该服务的配置更新标准 SKU 负载均衡器的配置。  使用 [az aks create][az-aks-create] 命令创建名为 *myAKSCluster* 的 AKS 群集。

> [!NOTE]
> 只能在创建群集时使用 *load-balancer-sku* 属性。 创建 AKS 群集后，无法更改负载均衡器 SKU。 此外，在一个群集中只能使用一种类型的负载均衡器 SKU。
> 
> 若要使用自己的公共 IP，请使用 *load-balancer-outbound-ips* 或 *load-balancer-outbound-ip-prefixes* 参数。 也可以在[更新群集](#optional---provide-your-own-public-ips-or-prefixes-for-egress)时使用这两个参数。

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

片刻之后，该命令将会完成，并返回有关群集的 JSON 格式信息。

## <a name="connect-to-the-cluster"></a>连接至群集

若要管理 Kubernetes 群集，请使用 Kubernetes 命令行客户端 [kubectl][kubectl]。 若要在本地安装 `kubectl`，请使用 [az aks install-cli][az-aks-install-cli] 命令：

<!--MOONCAKE: Not Avaiable on If you use Azure Local Shell, `kubectl` is already installed.-->

```azurecli
az aks install-cli
```

若要将 `kubectl` 配置为连接到 Kubernetes 群集，请使用 [az aks get-credentials][az-aks-get-credentials] 命令。 此命令将下载凭据，并将 Kubernetes CLI 配置为使用这些凭据。

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

若要验证到群集的连接，请使用 [kubectl get][kubectl-get] 命令返回群集节点的列表。

```azurecli
kubectl get nodes
```

以下示例输出显示在上一步创建的单个节点。 请确保节点的状态为 *Ready*：

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.13.10
```

## <a name="verify-your-cluster-uses-the-standard-sku"></a>验证群集是否使用标准 SKU 

使用 [az aks show][az-aks-show] 显示群集的配置。

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster

{
  "aadProfile": null,
  "addonProfiles": null,
   ...
   "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerSku": "standard",
    ...
```

验证 *loadBalancerSku* 属性是否显示为 *standard*。

## <a name="use-the-load-balancer"></a>使用负载均衡器

若要在群集上使用负载均衡器，请创建包含服务类型 *LoadBalancer* 的服务清单。 若要显示负载均衡器的工作状况，请创建另一个清单并在其中包含要在群集上运行的示例应用程序。 此示例应用程序将通过负载均衡器公开，并可通过浏览器查看。

创建名为 `sample.yaml` 的清单，如以下示例所示：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: dockerhub.azk8s.cn/microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
```

以上清单配置两个部署：*azure-vote-front* 和 *azure-vote-back*。 若要将 *azure-vote-front* 部署配置为使用负载均衡器公开，请创建名为 `standard-lb.yaml` 的清单，如以下示例所示：

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

服务 *azure-vote-front* 使用 *LoadBalancer* 类型在 AKS 群集上配置负载均衡器，以连接到 *azure-vote-front* 部署。

使用 [kubectl apply][kubectl-apply] 部署示例应用程序和负载均衡器，并指定 YAML 清单的名称：

```console
kubectl apply -f sample.yaml
kubectl apply -f standard-lb.yaml
```

标准 SKU 负载均衡器现已配置为公开示例应用程序。  使用 [kubectl get][kubectl-get] 查看 *azure-vote-front* 的服务详细信息，以查看负载均衡器的公共 IP。 负载均衡器的公共 IP 地址显示在 *EXTERNAL-IP* 列中。 可能需要一两分钟，IP 地址才会从 *\<pending\>* 更改为实际的外部 IP 地址，如以下示例所示：

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

在浏览器中导航到公共 IP，并验证是否能够看到示例应用程序。 在以上示例中，公共 IP 为 `52.179.23.131`。

![浏览到 Azure Vote 的图像](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

> [!NOTE]
> 还可将负载均衡器配置为内部负载均衡器且不公开公共 IP。 若要将负载均衡器配置为内部负载均衡器，请添加 `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` 作为 *LoadBalancer* 服务的注释。 可在[此处][internal-lb-yaml]查看示例 YAML 清单，以及有关内部负载均衡器的更多详细信息。

## <a name="optional---scale-the-number-of-managed-public-ips"></a>可选 - 调整托管公共 IP 的数量

结合默认创建的托管出站公共 IP 使用标准 SKU 负载均衡器时，可以使用 *load-balancer-managed-ip-count* 参数来调整托管出站公共 IP 的数量。 

若要更新现有群集，请运行以下命令。 还可以在创建群集时设置此参数，以指定多个托管出站公共 IP。

```azurecli
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

以上示例将 *myResourceGroup* 中 *myAKSCluster* 群集的托管出站公共 IP 数量设置为 *2*。 

还可以在创建群集时，通过追加 `--load-balancer-managed-outbound-ip-count` 参数并将其设置为所需的值，使用 *load-balancer-managed-ip-count* 参数来设置托管出站公共 IP 的初始数量。 托管出站公共 IP 的默认数量为 1。

## <a name="optional---provide-your-own-public-ips-or-prefixes-for-egress"></a>可选 - 提供自己的出口公共 IP 或前缀

使用标准 SKU 负载均衡器时，AKS 群集将自动在为它创建的同一个资源组中创建公共 IP，并将该公共 IP 分配给标准 SKU 负载均衡器。   或者，可以在创建群集时分配自己的公共 IP，或更新现有群集的负载均衡器属性。

通过引入多个 IP 地址或前缀，可以在单个负载均衡器对象后面定义 IP 地址时定义多个后备服务。 特定节点的出口终结点将依赖于与这些节点关联的服务。

> [!IMPORTANT]
> 必须将出口的标准 SKU 公共 IP 与负载均衡器的标准 SKU 配合使用。   可以使用 [az network public-ip show][az-network-public-ip-show] 命令验证公共 IP 的 SKU：
>
> ```azurecli
> az network public-ip show --resource-group myResourceGroup --name myPublicIP --query sku.name -o tsv
> ```

使用 [az network public-ip show][az-network-public-ip-show] 命令列出公共 IP 的 ID。

```azurecli
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

以上命令显示 *myResourceGroup* 资源组中 *myPublicIP* 公共 IP 的 ID。

结合 *load-balancer-outbound-ips* 参数使用 *az aks update* 命令，以更新群集中的公共 IP。

以下示例结合前一命令返回的 ID 使用 *load-balancer-outbound-ips* 参数。

```azurecli
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

你也可以将出口的公共 IP 前缀与标准 SKU 负载均衡器配合使用。  以下示例使用 [az network public-ip prefix show][az-network-public-ip-prefix-show] 命令列出公共 IP 前缀的 ID：

```azurecli
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

以上命令显示 *myResourceGroup* 资源组中 *myPublicIPPrefix* 公共 IP 前缀的 ID。

以下示例将 *load-balancer-outbound-ip-prefixes* 参数与前一命令返回的 ID 配合使用。

```azurecli
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

> [!IMPORTANT]
> 公共 IP 和 IP 前缀必须位于同一区域，并且与 AKS 群集位于同一订阅中。 

### <a name="define-your-own-public-ip-or-prefixes-at-cluster-create-time"></a>在创建群集时定义自己的公共 IP 或前缀

在创建群集时，你可能想要使用自己的出口 IP 地址或 IP 前缀，以支持出口终结点白名单等方案。 将上面所示的相同参数追加到群集创建步骤，可在群集生命周期的起始部分定义自己的公共 IP 和 IP 前缀。

结合 *load-balancer-outbound-ips* 参数使用 *az aks create* 命令可在启动时使用你的公共 IP 创建新的群集。

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

结合 *load-balancer-outbound-ip-prefixes* 参数使用 *az aks create* 命令可在启动时使用你的公共 IP 前缀创建新的群集。

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="clean-up-the-standard-sku-load-balancer-configuration"></a>清理标准 SKU 负载均衡器配置

若要删除示例应用程序和负载均衡器配置，请使用 [kubectl delete][kubectl-delete]：

```console
kubectl delete -f sample.yaml
kubectl delete -f standard-lb.yaml
```

## <a name="next-steps"></a>后续步骤

在 [Kubernetes 服务文档][kubernetes-services]中详细了解 Kubernetes 服务。

<!-- LINKS - External -->

[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->

[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-show
[az-aks-create]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: https://docs.azure.cn/cli/extension?view=azure-cli-latest#az-extension-add
[az-feature-list]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-list
[az-feature-register]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-register
[az-group-create]: https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create
[az-provider-register]: https://docs.azure.cn/cli/provider?view=azure-cli-latest#az-provider-register
[az-network-public-ip-show]: https://docs.azure.cn/cli/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: https://docs.azure.cn/cli/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: https://docs.azure.cn/cli/role/assignment?view=azure-cli-latest#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: https://docs.azure.cn/cli/extension?view=azure-cli-latest#az-extension-add
[az-extension-update]: https://docs.azure.cn/cli/extension?view=azure-cli-latest#az-extension-update

<!--Update_Description: new articles on load balancer standard -->
<!--ms.date: 10/17/2019-->