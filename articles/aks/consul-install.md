---
title: 在 Azure Kubernetes 服务 (AKS) 中安装 Hashicorp Consul
description: 了解如何在 Azure Kubernetes 服务 (AKS) 群集中安装和使用 Consul 来创建服务网格
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 06/19/2019
ms.date: 09/23/2019
ms.author: v-yeche
ms.openlocfilehash: d46702f6d4dc61f21a7759129e1403f27c2483e5
ms.sourcegitcommit: 6a62dd239c60596006a74ab2333c50c4db5b62be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2019
ms.locfileid: "71156438"
---
<!--Verify sucessfully-->

# <a name="install-and-use-consul-connect-in-azure-kubernetes-service-aks"></a>在 Azure Kubernetes 服务 (AKS) 中安装并使用 Consul Connect

[Consul][consul-github] 是跨 Kubernetes 群集中的微服务提供关键功能集的开源服务网格。 这些功能包括服务发现、运行状况检查、服务分段和可观察性。 有关 Consul 的详细信息，请参阅官方文档[什么是 Consul？][consul-docs-concepts]。

本文介绍如何安装 Consul。 Consul 组件安装在 AKS 上的 Kubernetes 群集中。

> [!NOTE]
> 本文中的说明适用于 Consul 版本 `1.5`。
>
> Hashicorp 团队已针对 Kubernetes 版本 `1.12`、`1.14` 和 `1.14` 测试了 Consul `1.5.x` 版本。 可以在 [GitHub - Consul 版本][consul-github-releases]中找到其他 Consul 版本，并可以在 [Consul - 发行说明][consul-release-notes]中找到有关每个版本的信息。

在本文中，学习如何：

> [!div class="checklist"]
> * 在 AKS 上安装 Consul 组件
> * 验证 Consul 安装
> * 从 AKS 中卸载 Consul

## <a name="before-you-begin"></a>准备阶段

本文中详述的步骤假设已创建 AKS 群集（已启用 RBAC 的 Kubernetes `1.11` 及更高版本）并已与该群集建立 `kubectl` 连接。 如果需要帮助完成这些项目，请参阅 [AKS 快速入门][aks-quickstart]。

需要使用 [Helm][helm] 按照这些说明安装 Consul。 建议在群集中正确安装并配置版本 `2.12.2` 或更高版本。 安装 Helm 时如需帮助，请参阅 [AKS Helm 安装指南][helm-install]。 所有 Consul Pod 也必须按计划在 Linux 节点上运行。

本文将 Consul 安装指南分为多个独立步骤。 最终结果的结构与官方 Consul 安装[指南][consul-install-k8]相同。

### <a name="install-the-consul-components-on-aks"></a>在 AKS 上安装 Consul 组件

我们首先克隆 Kubernetes GitHub 存储库中的官方 HashiCorp Consul。

```bash
git clone https://github.com/hashicorp/consul-helm.git
cd consul-helm
```

接下来，需要创建用于安装 Consul 的自定义 Helm 值文件。 在安装 Consul 之前创建以下自定义值文件。

```bash
vim consul-values.yaml
```

然后，将以下值复制并粘贴到 consul-values.yaml 文件中。

```yaml
# Sets datacenter name and version Consul to use
global:
  datacenter: dc1
  image: "consul:1.5.2"

# Enables proxies to be injected into pods
connectInject:
  enabled: true

# Enables UI on ClusterIP
ui:
  service:
    type: "ClusterIP"

# Enables GRCP that is required for connectInject
client:
  enabled: true
  grpc: true

# Sets replicase to 3 for HA
server:
  replicas: 3
  bootstrapExpect: 1
  disruptionBudget:
    enabled: true
    maxUnavailable: 1

# Syncs Kubernetes service discovery with Consul
syncCatalog:
  enabled: true
```

创建自定义值文件后，接下来可将 Consul 安装到 AKS 群集中

Bash

```bash
helm install -f consul-values.yaml --name consul --namespace consul .
```

> [!NOTE]
> 如果遇到以下错误消息，可以创建 yaml 文件，并获取 Tiller 服务的服务帐户和角色绑定。
>     ```output
>     Error: release consul failed: namespaces "consul" is forbidden: User "system:serviceaccount:kube-system:default" cannot get resource "namespaces" in API group "" in the namespace "consul"
>     ```
> 
> * 创建 `helm-rbac.yaml` 并在其中填充以下 yaml 内容。
>      ```
>       apiVersion: v1
>       kind: ServiceAccount
>       metadata:
>         name: tiller
>         namespace: kube-system
>       ---
>       apiVersion: rbac.authorization.k8s.io/v1
>       kind: ClusterRoleBinding
>       metadata:
>         name: tiller
>       roleRef:
>         apiGroup: rbac.authorization.k8s.io
>         kind: ClusterRole
>         name: cluster-admin
>      subjects:
>         - kind: ServiceAccount
>           name: tiller
>           namespace: kube-system
>       ```
>
> * Run `kubectl apply` cmdlet to create the servcie account and role binding:
>     ```console
>     kubectl apply -f helm-rbac.yaml
>     ```
>
>     ```output
>     serviceaccount/tiller created
>     clusterrolebinding.rbac.authorization.k8s.io/tiller-clusterrolebinding created
>     ```
>     
>     ```console
>     kubectl create namespace consul
>     helm init --service-account tiller --tiller-image gcr.azk8s.cn/kubernetes-helm/tiller:v2.13.0 `
>            --stable-repo-url https://mirror.azure.cn/kubernetes/charts/ --upgrade
>     
>     helm install -f consul-values.yaml --name consul --namespace consul .
>     ```
>

`Consul` Helm 图表将部署许多对象。 上述 `helm install` 命令的输出会显示对象列表。 部署 Consul 组件可能需要 4 到 5 分钟才能完成，具体取决于群集环境。

> [!NOTE]
> 所有 Consul Pod 必须按计划在 Linux 节点上运行。 

<!--Not Availble on If you have Windows Server node pools in addition to Linux node pools on your cluster, verify that all Consul pods have been scheduled to run on Linux nodes.-->

此时，已将 Consul 部署到 AKS 群集。 为确保成功部署 Consul，让我们转到下一部分来验证 Consul 安装。

## <a name="validate-the-consul-installation"></a>验证 Consul 安装

首先，确认已创建所需的服务。 使用 [kubectl get svc][kubectl-get] 命令查看正在运行的服务。 查询 `consul` 命名空间，`consul` Helm 图表已在其中安装 Consul 组件：

```console
kubectl get svc --namespace consul
```

以下示例输出显示了现在应正在运行的服务：

```console
NAME                                 TYPE           CLUSTER-IP     EXTERNAL-IP             PORT(S)                                                                   AGE
consul                               ExternalName   <none>         consul.service.consul   <none>                                                                    38m
consul-consul-connect-injector-svc   ClusterIP      10.0.89.113    <none>                  443/TCP                                                                   40m
consul-consul-dns                    ClusterIP      10.0.166.82    <none>                  53/TCP,53/UDP                                                             40m
consul-consul-server                 ClusterIP      None           <none>                  8500/TCP,8301/TCP,8301/UDP,8302/TCP,8302/UDP,8300/TCP,8600/TCP,8600/UDP   40m
consul-consul-ui                     ClusterIP      10.0.117.164   <none>                  80/TCP                                                                    40m
```

接下来，确认已创建所需的 Pod。 使用 [kubectl get pods][kubectl-get] 命令，然后再次查询 `consul` 命名空间：

```console
kubectl get pods --namespace consul
```

以下示例输出显示了正在运行的 Pod：

```console
NAME                                                              READY   STATUS    RESTARTS   AGE
consul-consul-7cc9v                                               1/1     Running   0          37m
consul-consul-7klg7                                               1/1     Running   0          37m
consul-consul-connect-injector-webhook-deployment-57f88df8hgmfs   1/1     Running   0          37m
consul-consul-lq8qb                                               1/1     Running   0          37m
consul-consul-server-0                                            1/1     Running   0          37m
consul-consul-server-1                                            1/1     Running   0          36m
consul-consul-server-2                                            1/1     Running   0          36m
consul-consul-sync-catalog-7cf7d5bfff-jjbjv                       1/1     Running   2          37m
```

 所有 pod 应显示 `Running` 状态。 如果 Pod 没有这些状态，请在运行之前等待一两分钟。 如果任何 Pod 报告问题，请使用 [kubectl describe pod][kubectl-describe] 命令查看其输出和状态。

## <a name="accessing-the-consul-ui"></a>访问 Consul UI

上述安装过程已安装 Consul UI，它为 Consul 提供基于 UI 的配置。 Consul 的 UI 不会通过外部 IP 地址公开。 若要访问加载项用户界面，请使用 [kubectl port-forward][kubectl-port-forward] 命令。 此命令在客户端计算机与 AKS 群集中相关 Pod 之间建立安全连接。

```bash
kubectl port-forward -n consul svc/consul-consul-ui 8080:80
```
现在可以打开一个浏览器并指向 `http://localhost:8080/ui`，以打开 Consul UI。 打开 UI 时，应会看到以下内容：

![Consul UI](./media/consul/consul-ui.png)

## <a name="uninstall-consul-from-aks"></a>从 AKS 中卸载 Consul

> [!WARNING]
> 从正在运行的系统中删除 Consul 可能会导致服务之间出现流量相关的问题。 在继续之前，请确保对系统进行预配，以便在没有 Consul 的情况下系统仍可正常运行。

### <a name="remove-consul-components-and-namespace"></a>删除 Consul 组件和命名空间

若要从 AKS 群集中删除 Consul，请使用以下命令。 `helm delete` 命令将删除 `consul` 图表，`kubectl delete ns` 命令将删除 `consul` 命名空间。

```bash
helm delete --purge consul
kubectl delete ns consul
```

## <a name="next-steps"></a>后续步骤

若要了解 Consul 的更多安装和配置选项，请参阅以下官方 Consul 文章：

- [Consul - Helm 安装指南][consul-install-k8]
- [Consul - Helm 安装选项][consul-install-helm-options]

也可以关注使用 [Consul 示例应用程序][consul-app-example]的其他方案。

<!-- LINKS - external -->

[Hashicorp]: https://hashicorp.com
[cosul-github]: https://github.com/hashicorp/consul
[helm]: https://helm.sh

[consul-docs-concepts]: https://www.consul.io/docs/platform/k8s/index.html
[consul-github]: https://github.com/hashicorp/consul
[consul-github-releases]: https://github.com/hashicorp/consul/releases
[consul-release-notes]: https://github.com/hashicorp/consul/blob/master/CHANGELOG.md
[consul-install-download]: https://www.consul.io/downloads.html
[consul-install-k8]: https://www.consul.io/docs/platform/k8s/run.html
[consul-install-helm-options]: https://www.consul.io/docs/platform/k8s/helm.html#configuration-values-
[consul-app-example]: https://github.com/hashicorp/demo-consul-101/tree/master/k8s
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->

[aks-quickstart]: ./kubernetes-walkthrough.md
[consul-scenario-mtls]: ./consul-mtls.md
[helm-install]: ./kubernetes-helm.md

<!-- Update_Description: new article about aks consul install -->
<!--ms.date: 09/23/2019-->