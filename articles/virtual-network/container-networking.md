---
title: 适用于容器的 Azure 虚拟网络 | Azure
description: 了解适用于 Kubernetes 群集的 CNI 插件。该插件可让容器相互通信，以及与虚拟网络中的其他资源通信。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/18/2017
ms.date: 01/22/2018
ms.author: v-yeche
ms.openlocfilehash: abd880888dc07fa254ad909145fb6c1f069c29f2
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52655823"
---
# <a name="container-networking"></a>容器网络

Azure 提供了一个[容器网络接口 (CNI) 插件](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md)，可通过该插件使用本机 Azure 网络功能部署并管理你自己的 Kubernetes 群集。 将 Kubernetes 群集与 [Azure 容器服务引擎](https://github.com/Azure/acs-engine)（或称为 ACS 引擎）部署在一起时会默认启用此插件。

## <a name="networking-capabilities"></a>网络功能

容器可以利用虚拟网络提供的丰富功能集，例如：
-   可为群集创建单独的虚拟网络，或者在现有的虚拟网络部署群集。 
-   群集中的每个 pod 从虚拟网络中接收 IP 地址，并可以直接与群集中的其他 pod 以及虚拟网络中的任何虚拟机通信。 
-   pod 可以通过 ExpressRoute 和站点到站点 VPN 连接，来与对等虚拟网络中的其他 pod 和虚拟机以及本地网络建立连接。 本地资源可与 pod 通信。 
-   可以通过 Azure 负载均衡器在 Internet 上公开 Kubernetes 服务。  
-   已启用服务终结点的子网中的 pod 可以安全连接到 Azure 服务（例如存储和 SQL 数据库）。
-   可以使用用户定义的路由将流量从 pod 路由到网络虚拟设备。 
-   Pod 可以访问 Internet 上的公共资源。
-   可为 pod 分配公共 IP 地址，该地址可与 DNS 名称相关联。

## <a name="limits"></a>限制
使用该插件时，最多可以在一个 Kubernetes 群集中部署 4,000 个节点，并为每个节点最多部署 250 个 pod，每个群集的 pod 总数限制为 16,000 个。

## <a name="constraints"></a>约束
- 结合使用 [Azure 容器服务 (AKS)](../aks/intro-kubernetes.md?toc=%2fvirtual-network%2ftoc.json) 或 [ACS](../container-service/kubernetes/container-service-intro-kubernetes.md?toc=%2fvirtual-network%2ftoc.json) 和 Kubernetes 部署 Kubernetes 群集时，不会启用该插件。
- 没有提供对 Kubernetes 网络策略（包括 DNS 或访问策略）的本机支持。
- 此插件不支持以 Pod 为单位的网络策略。

## <a name="pricing"></a>定价
使用 CNI 插件不会产生费用。

## <a name="next-steps"></a>后续步骤

了解如何[在自己的虚拟网络](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/features.md#using-azure-integrated-networking-cni)中[部署 Kubernetes 群集](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/deploy.md)。
<!-- Update_Description: wording update -->
