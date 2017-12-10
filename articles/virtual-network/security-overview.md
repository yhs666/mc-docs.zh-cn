---
title: "Azure 网络安全概述 | Azure"
description: "了解用于控制 Azure 资源之间的网络流量流的安全选项。"
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/19/2017
ms.date: 12/11/2017
ms.author: v-yeche
ms.openlocfilehash: b61d3ec7fc53d547a11936954f2410f7fd02e07a
ms.sourcegitcommit: 4c64f6d07fc471fb6589b18843995dca1cbfbeb1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2017
---
# <a name="network-security"></a>网络安全性

可以使用网络安全组限制发往虚拟网络中的资源的网络流量。 网络安全组包含一个安全规则列表，这些规则可根据源或目标 IP 地址、端口和协议允许或拒绝入站或出站网络流量。 

## <a name="network-security-groups"></a>网络安全组

每个网络接口有零个或一个关联的网络安全组。 每个网络接口位于[虚拟网络](virtual-networks-overview.md)子网中。 一个子网也可以有零个或一个关联的网络安全组。 

应用到某个子网时，安全规则会应用到该子网中的所有资源。 除了部署网络接口以外，还可以在子网中部署其他 Azure 服务的实例，例如 HDInsight、虚拟机规模集和应用程序服务环境。

下面描述了当网络安全组同时与网络接口和该网络接口所在的子网相关联时，如何应用网络安全组：

- **入站流量**：首先评估与网络接口所在子网关联的网络安全组。 然后，与网络接口关联的网络安全组会评估允许通过与子网相关联的网络安全组的所有流量。 例如，我们可能要求通过 Internet 在端口 80 上对某个虚拟机进行入站访问。 如果将网络安全组同时关联到网络接口以及该网络接口所在的子网，则与子网和网络接口关联的网络安全组必须允许端口 80。 如果只允许端口 80 上的流量通过与子网或该子网所在的网络接口相关联的网络安全组，则会由于默认安全规则方面的原因而导致通信失败。 有关详细信息，请参阅[默认安全规则](#default-security-rules)。 如果只将网络安全组应用到子网或网络接口，并且该网络安全组包含允许入站端口 80 流量的规则，则通信会成功。 
- **出站流量**：首先评估与网络接口关联的网络安全组。 然后，与子网关联的网络安全组会评估允许通过与网络接口相关联的网络安全组的所有流量。

将网络安全组同时应用到网络接口和子网时，你不一定总能察觉得到。 可以通过查看网络接口的[有效安全规则](virtual-network-manage-nsg-arm-portal.md)，轻松查看已应用到网络接口的聚合规则。 还可以使用 Azure 网络观察程序中的 [IP 流验证](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fvirtual-network%2ftoc.json)功能来确定是否允许发往或发自网络接口的通信。 该工具会告知通信是否受允许，以及哪个网络安全规则允许或拒绝流量。

> [!NOTE]
> 网络安全组关联到子网或关联到部署经典部署模型的虚拟机和云服务，而不是关联到资源管理器部署模型中的网络接口。 若要详细了解 Azure 部署模型，请参阅[了解 Azure 部署模型](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fvirtual-network%2ftoc.json)。

可将同一网络安全组应用到所选的任意数量的各个网络接口和子网。

## <a name="security-rules"></a>安全规则

一个网络安全组包含零个或者不超过 Azure 订阅限制的任意数量的规则。 每个规则指定以下属性：
<!-- Not Available on [limits](../azure-subscription-service-limits.md?toc=%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) -->

|属性  |说明  |
|---------|---------|
|名称|网络安全组中的唯一名称。|
|Priority | 介于 100 和 4096 之间的数字。 规则按优先顺序进行处理。先处理编号较小的规则，因为编号越小，优先级越高。 一旦流量与某个规则匹配，处理即会停止。 因此，不会处理优先级较低（编号较大）的、其属性与高优先级规则相同的所有规则。|
|源或目标| 值为“任何”，或者单个 IP 地址、CIDR 块（例如 10.0.0.0/24）、服务标记或应用程序安全组。 详细了解[服务标记](#service-tags)和[应用程序安全组](#application-security-groups)。 指定范围、服务标记或应用程序安全组可以减少创建的安全规则数。 在一个规则中指定多个单独的 IP 地址和范围（不能指定多个服务标记或应用程序组）的功能称为扩充式安全规则。 详细了解[扩充式安全规则](#augmented-security-rules)。 只能在通过资源管理器部署模型创建的网络安全组中创建扩充式安全规则。 在通过经典部署模型创建的网络安全组中，不能指定多个 IP 地址和 IP 地址范围。|
|协议     | TCP、UDP 或“任何”，包括 TCP、UDP 和 ICMP。 不能仅指定 ICMP，因此，如果需要 ICMP，则必须使用“任何”。 |
|方向| 该规则是应用到入站还是出站流量。|
|端口范围     |可以指定单个端口或端口范围。 例如，可以指定 80 或 10000-10005。 指定范围可以减少创建的安全规则数。 在一个规则中指定多个单独的端口和端口范围的功能以预览版提供，称为扩充式安全规则。 在使用扩充式安全规则之前，请阅读[预览功能](#preview-features)了解重要信息。 只能在通过资源管理器部署模型创建的网络安全组中创建扩充式安全规则。 在通过经典部署模型创建的网络安全组中，不能在同一个安全规则中指定多个端口或端口范围。   |
|操作     | 允许或拒绝        |

安全规则是有状态的。 例如，如果针对通过端口 80 访问的任何地址指定了出站安全规则，则不需要指定入站安全规则来响应出站流量。 如果通信是从外部发起的，则只需指定入站安全规则。 反之亦然。 如果允许通过某个端口发送入站流量，则不需要指定出站安全规则来响应通过该端口发送的流量。

<!-- Not Available ## Augmented security rules-->
## <a name="default-security-rules"></a>默认安全规则

如果网络安全组不与某个子网或网络接口相关联，则允许发往该子网或网络接口的所有入站流量，或发自该子网或网络接口的所有出站流量。 创建网络安全组后，Azure 会立即在该网络安全组中创建以下默认规则：

### <a name="inbound"></a>入站

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priority|源|源端口|目标|目标端口|协议|访问|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|全部|允许|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priority|源|源端口|目标|目标端口|协议|访问|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|全部|允许|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priority|源|源端口|目标|目标端口|协议|访问|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|全部|拒绝|

### <a name="outbound"></a>出站

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priority|源|源端口| 目标 | 目标端口 | 协议 | 访问 |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | 全部 | 允许 |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Priority|源|源端口| 目标 | 目标端口 | 协议 | 访问 |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | 全部 | 允许 |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priority|源|源端口| 目标 | 目标端口 | 协议 | 访问 |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | 全部 | 拒绝 |

在“源”和“目标”列表中，“VirtualNetwork”、“AzureLoadBalancer”和“Internet”是[服务标记](#tags)，而不是 IP 地址。 在“协议”列中，“所有”包含 TCP、UDP 和 ICMP。 创建规则时，可以指定 TCP、UDP 或“所有”，但不能仅指定 ICMP。 因此，如果规则需要 ICMP，则必须为协议选择“所有”。 “源”和“目标”列中的“0.0.0.0/0”表示所有地址。

不能删除默认规则，但可以通过创建更高优先级的规则来替代默认规则。

<!--Not Available ## Service tags-->

<!-- Not Available ## Application security groups-->

## <a name="next-steps"></a>后续步骤

* 完成[创建网络安全组](virtual-networks-create-nsg-arm-pportal.md)教程
<!-- Not Available * Complete the [Create a network security group with application security groups](create-network-security-group-preview.md) tutorial-->


<!--Update_Description: wording update -->