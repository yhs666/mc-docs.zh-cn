---
title: 概念 - 在 Azure Kubernetes 服务 (AKS) 中缩放应用程序
description: 了解 Azure Kubernetes 服务 (AKS) 中的缩放，包括水平 Pod 自动缩放程序、群集自动缩放程序和 Azure 容器实例连接器。
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: conceptual
origin.date: 02/28/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: ed5a960c4f41387dd9735898f054c841281e781d
ms.sourcegitcommit: 1d4dc20d24feb74d11d8295e121d6752c2db956e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73068897"
---
# <a name="scaling-options-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS) 中的应用程序缩放选项

在 Azure Kubernetes 服务 (AKS) 中运行应用程序时，可能需要增加或减少计算资源量。 随着所需应用程序实例数量的变化，可能还需要更改基础 Kubernetes 节点的数量。 可能还需要快速预配大量其他应用程序实例。

本文介绍有助于在 AKS 中缩放应用程序的核心概念：

- [手动缩放](#manually-scale-pods-or-nodes)
- [水平 Pod 自动缩放程序 (HPA)](#horizontal-pod-autoscaler)

<!--Not Available on - [Cluster autoscaler](#cluster-autoscaler)-->
<!--Not Available on - [Azure Container Instance (ACI) integration with AKS](#burst-to-azure-container-instances)-->

## <a name="manually-scale-pods-or-nodes"></a>手动缩放 Pod 或节点

可以手动缩放副本 (Pod) 和节点，以测试应用程序如何响应可用资源和状态的更改。 手动缩放资源还可以定义用于维持固定成本的设定数量的资源，例如节点数。 若要手动缩放，请定义副本或节点计数。 然后，Kubernetes API 根据该副本或节点计数计划创建其他 Pod 或排空节点。

若要开始使用手动缩放 Pod 和节点，请参阅[在 AKS 中缩放应用程序][aks-scale]。

## <a name="horizontal-pod-autoscaler"></a>水平 Pod 自动缩放程序

Kubernetes 使用水平 Pod 自动缩放程序 (HPA) 来监视资源需求并自动缩放副本数量。 默认情况下，水平 Pod 自动缩放程序每隔 30 秒检查一次指标 API，以了解副本计数所需的任何更改。 当需要进行更改时，副本的数量会相应增加或减少。 水平 Pod 自动缩放程序与已经为 Kubernetes 1.8+ 部署了指标服务器的 AKS 群集配合使用。

![Kubernetes 水平 Pod 自动缩放](media/concepts-scale/horizontal-pod-autoscaling.png)

为给定部署配置水平 Pod 自动缩放程序时，请定义可运行的最小和最大副本数。 还可以定义指标以监视任何缩放决策并以此为依据，例如 CPU 使用情况。

若要开始使用 AKS 中的水平 Pod 自动缩放程序，请参阅[在 AKS 中自动缩放 Pod][aks-hpa]。

### <a name="cooldown-of-scaling-events"></a>缩放事件的冷却时间

由于水平 Pod 自动缩放程序每 30 秒检查一次指标 API，因此在进行另一次检查之前，先前的缩放事件可能尚未成功完成。 此行为可能导致水平 Pod 自动缩放程序会在上一个缩放事件能够接收应用程序工作负荷且需要对资源进行相应调整之前更改副本数。

为最大限度地减少这些争用事件，可以设置冷却时间值或延迟值。 这些值定义水平 Pod 自动缩放程序在执行一个缩放事件之后，触发另一个缩放事件之前必须等待的时间。 此行为允许新副本计数生效，指标 API 反映分布式工作负荷。 默认情况下，纵向扩展事件的延迟为 3 分钟，纵向缩减事件的延迟为 5 分钟

目前，无法从默认值调整这些冷却时间值。

<!--Not Available on ## Cluster autoscaler-->

### <a name="scale-up-events"></a>纵向扩展事件

如果节点没有足够的计算资源来运行请求的 Pod，则该 Pod 无法按照计划继续运行。 除非节点池中有其他可用的计算资源，否则无法启动该 Pod。

当群集自动缩放程序通知由于节点池资源限制而无法将 Pod 列入计划时，节点池中的节点数量会增加，提供额外的计算资源。 当这些额外的节点成功部署并可在节点池中使用时，可将 Pod 计划为运行。

如果应用程序需要快速缩放，则某些 Pod 可能会保持等待计划的状态，直到群集自动缩放程序部署的其他节点可以接受列入计划的 Pod。 对于具有高突发需求的应用程序，可以使用虚拟节点和 Azure 容器实例进行缩放。

### <a name="scale-down-events"></a>纵向缩减事件

群集自动缩放程序还会监视最近未收到新计划请求的节点的 Pod 计划状态。 此方案表明节点池具有的计算资源多于所需资源，并且可以减少节点数。

默认情况下，计划删除传递超过 10 分钟不再需要的阈值的节点。 发生这种情况时，会计划 Pod 在节点池中的其他节点上运行，并且群集自动缩放程序会减少节点数。

当群集自动缩放程序减少节点数时，由于在不同节点上计划 Pod，应用程序可能会发生一些中断。 为最大限度地减少中断，请避免使用单个 Pod 实例的应用程序。

<!--Not Available on ## Burst to Azure Container Instances-->

## <a name="next-steps"></a>后续步骤

若要开始缩放应用程序，请首先按照[使用 Azure CLI 创建 AKS 群集的快速入门][aks-quickstart]进行操作。 然后，可以开始手动或自动缩放 AKS 群集中的应用程序：

- 手动缩放 [Pod][aks-manually-scale-pods] 或[节点][aks-manually-scale-nodes]
- 使用[水平 Pod 自动缩放程序][aks-hpa]

<!--Not Avaialble on - Use the [cluster autoscaler][aks-cluster-autoscaler]-->

有关核心 Kubernetes 和 AKS 概念的详细信息，请参阅以下文章：

- [Kubernetes/AKS 群集和工作负荷][aks-concepts-clusters-workloads]
- [Kubernetes/AKS 访问和标识][aks-concepts-identity]
- [Kubernetes/AKS 安全性][aks-concepts-security]
- [Kubernetes/AKS 虚拟网络][aks-concepts-network]
- [Kubernetes/AKS 存储][aks-concepts-storage]

<!-- LINKS - external -->

<!-- LINKS - internal -->

[aks-quickstart]: kubernetes-walkthrough.md
[aks-hpa]: tutorial-kubernetes-scale.md#autoscale-pods
[aks-scale]: tutorial-kubernetes-scale.md
[aks-manually-scale-pods]: tutorial-kubernetes-scale.md#manually-scale-pods
[aks-manually-scale-nodes]: tutorial-kubernetes-scale.md#manually-scale-aks-nodes

<!--Not Available on [aks-cluster-autoscaler]: autoscaler.md-->

[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-network]: concepts-network.md

<!-- Update_Description: wording update, update link -->