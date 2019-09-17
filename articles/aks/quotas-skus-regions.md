---
title: Azure Kubernetes 服务 (AKS) 中的配额、SKU 和适用地区
description: 了解 Azure Kubernetes 服务 (AKS) 中的默认配额、受限制的节点 VM SKU 大小和适用地区。
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: conceptual
origin.date: 04/09/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: e86e9e18cc913ea8dd072afe9068614c40f8b9b7
ms.sourcegitcommit: 57994a3f6a263c95ff3901361d3e48b10cfffcdd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70500733"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS) 中的配额、虚拟机大小限制和适用地区

所有 Azure 服务都设置针对资源和功能的默认限制和配额。 某些虚拟机 (VM) SKU 也被限制使用。

本文详述了 Azure Kubernetes 服务 (AKS) 资源的默认资源限制，以及 Azure 区域中 AKS 的可用性。

## <a name="service-quotas-and-limits"></a>服务配额和限制

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>预配的基础结构

所有其他网络、计算和存储限制均适用于预配的基础结构。 若要了解相关限制，请参阅 [Azure 订阅和服务限制](../azure-subscription-service-limits.md)。

> [!IMPORTANT]
> 升级 AKS 群集时，会临时使用其他资源。 这些资源包括虚拟网络子网中的可用 IP 地址，或者虚拟机 vCPU 配额。

<!--Not Available on  If you use Windows Server containers (currently in preview in AKS)-->
<!--Not Available on  [Upgrade a node pool in AKS][nodepool-upgrade]-->

## <a name="restricted-vm-sizes"></a>受限制的 VM 大小

AKS 群集中的每个节点都包含固定数量的计算资源，例如 vCPU 和内存。 如果某个 AKS 节点包含的计算资源不足，则 Pod 可能无法正常运行。 若要确保所需的 *kube-system* Pod 和你的应用程序能够可靠地调度，请勿在 AKS 中使用以下 VM SKU：

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

有关 VM 类型及其计算资源的详细信息，请参阅 [Azure 中的虚拟机的大小][vm-skus]。

## <a name="region-availability"></a>上市区域

有关可以在其中部署和运行群集的地点的最新列表，请参阅 [AKS 适用地区][region-availability]。

## <a name="next-steps"></a>后续步骤

某些默认限制和配额可以提高。 如果资源支持提高配额，请通过 [Azure 支持请求][azure-support]来请求提高配额（请选择“配额”作为“支持类型”   ）。

<!--MOONCAKE: CORRECT ON Support Type with Quota-->

<!-- LINKS - External -->

[azure-support]: https://support.azure.cn/zh-cn/support/support-azure/
[region-availability]: https://www.azure.cn/zh-cn/home/features/products-by-region

<!-- LINKS - Internal -->

[vm-skus]: ../virtual-machines/linux/sizes.md

<!--Not Available on [nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool-->

<!-- Update_Description: wording update, update link -->