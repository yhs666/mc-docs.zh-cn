---
title: Azure Kubernetes 服务 (AKS) 中的配额、SKU 和适用地区
description: 了解 Azure Kubernetes 服务 (AKS) 中的默认配额、受限制的节点 VM SKU 大小和适用地区。
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: conceptual
origin.date: 04/09/2019
ms.date: 05/13/2019
ms.author: v-yeche
ms.openlocfilehash: df82f5f8d8a53cc2b7a09dacbb0f28fc8a37dd69
ms.sourcegitcommit: 8b9dff249212ca062ec0838bafa77df3bea22cc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/10/2019
ms.locfileid: "65520823"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS) 中的配额、虚拟机大小限制和适用地区

所有 Azure 服务都包括某些针对资源和功能的默认限制和配额。 对于节点大小，某些虚拟机 (VM) SKU 被限制使用。

本文详述了 Azure Kubernetes 服务 (AKS) 资源的默认资源限制，以及 Azure 区域中 AKS 服务的可用性。

## <a name="service-quotas-and-limits"></a>服务配额和限制

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>预配的基础结构

所有其他网络、计算和存储限制均适用于预配的基础结构。 有关相关限制，请参阅 [Azure 订阅和服务限制](../azure-subscription-service-limits.md)。

## <a name="restricted-vm-sizes"></a>受限制的 VM 大小

AKS 群集中的每个节点都包含固定数量的计算资源，例如 vCPU 和内存。 如果某个 AKS 节点包含的计算资源不足，则 pod 可能无法正常运行。 若要确保所需的 *kube-system* pod 和你的应用程序能够可靠地调度，不能在 AKS 中使用以下 VM SKU：

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

有关 VM 类型及其计算资源的详细信息，请参阅 [Azure 中的虚拟机的大小][vm-skus]。

## <a name="region-availability"></a>适用地区

有关可以在其中部署和运行群集的地点的最新列表，请参阅 [AKS 适用地区][region-availability]。

## <a name="next-steps"></a>后续步骤

某些默认限制和配额可以提高。 若要请求增加一个或多个资源（如果支持此功能），请提交 [Azure 支持请求][azure-support]（选择“配额”作为“问题类型”）。

<!-- LINKS - External -->
[azure-support]: https://support.azure.cn/zh-cn/support/support-azure/
[region-availability]: https://www.azure.cn/zh-cn/home/features/products-by-region

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md