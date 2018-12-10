---
title: 什么是 Azure DNS？
description: Microsoft Azure 上的 DNS 托管服务概述。 在 Microsoft Azure 上托管域。
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: cd3ec3fcf72a6a330b18c8bbd1fa26113ff4ce98
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52661927"
---
# <a name="what-is-azure-dns"></a>什么是 Azure DNS？

Azure DNS 是 DNS 域的托管服务，它使用 Microsoft Azure 基础结构提供名称解析。 通过在 Azure 中托管域，可以使用与其他 Azure 服务相同的凭据、API、工具和计费来管理 DNS 记录。

不能使用 Azure DNS 来购买域名。 可以通过第三方域名注册机构购买域名，但需支付年费。 然后，域可以托管在 Azure DNS 中进行记录管理。 有关详细信息，请参阅[向 Azure DNS 委托域](dns-domain-delegation.md)。

以下功能是 Azure DNS 附带的：

## <a name="reliability-and-performance"></a>可靠性和性能

Azure DNS 中的 DNS 域托管在 DNS 名称服务器的 Azure 全球网络上。 Azure DNS 使用任意广播网络，以便每个 DNS 查询由最近的可用 DNS 服务器来应答，从而为你的域提供快速性能和高可用性。

## <a name="security"></a>安全性

Azure DNS 服务基于 Azure 资源管理器，提供以下功能：

* [基于角色的访问控制](https://docs.azure.cn/azure-resource-manager/resource-group-overview#access-control) - 用于控制谁有权访问组织的特定操作。

* [活动日志](https://docs.azure.cn/azure-resource-manager/resource-group-overview#activity-logs) - 用于监视组织内用户对资源的修改情况，或者用于在故障排除时查找错误。

* [资源锁定](https://docs.azure.cn/azure-resource-manager/resource-group-lock-resources) - 用于锁定订阅、资源组或资源，防止组织中的其他用户意外删除或修改关键资源。

有关详细信息，请参阅[如何保护 DNS 区域和记录](dns-protect-zones-recordsets.md)。 


## <a name="ease-of-use"></a>易于使用

Azure DNS 服务可以管理 Azure 服务的 DNS 记录，还可以为外部资源提供 DNS。 Azure DNS 在 Azure 门户中集成，与其他 Azure 服务使用相同的凭据、支持合同和计费。 

DNS 基于在 Azure 中托管的 DNS 区域数和接收的 DNS 查询数进行计费。 若要深入了解定价，请参阅 [Azure DNS 定价](https://azure.cn/pricing/details/dns/)。

可以通过 Azure 门户、Azure PowerShell cmdlet 和跨平台 Azure CLI 对域和记录进行管理。 需要自动化 DNS 管理的应用程序可通过 REST API 和 SDK 与服务集成。

## <a name="customizable-virtual-networks-with-private-domains"></a>可自定义的包含专用域的虚拟网络

Azure DNS 还支持专用 DNS 域（现在为公开预览版）。 这样就可以在专用虚拟网络中使用你自己的自定义域名，而不使用当前可用的由 Azure 提供的名称。

有关详细信息，请参阅[在专用域中使用 Azure DNS](private-dns-overview.md)。


## <a name="next-steps"></a>后续步骤

* 了解 DNS 区域和记录：[DNS 区域和记录概述](dns-zones-records.md)。

* 了解如何在 Azure DNS 中创建区域：[创建 DNS 区域](./dns-getstarted-create-dnszone-portal.md)。

* 有关 Azure DNS 的常见问题，请参阅 [Azure DNS 常见问题](dns-faq.md)。

