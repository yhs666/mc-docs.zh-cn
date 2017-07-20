---
title: "在服务配置文件中指定 DNS 设置 | Azure"
description: "使用虚拟网络的服务配置文件指定自定义 DNS 设置"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/24/2016
ms.date: 04/26/2016
ms.author: v-dazen
ms.openlocfilehash: 8e9844833065286935ba61cc14919aa3d12ad74b
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>在服务配置文件中指定 DNS 设置
## <a name="dns-elements"></a>DNS 元素
服务配置文件可能包含 DnsServers 元素，该元素具有该服务将使用的域名系统 (DNS) 服务器的 IPv4 地址列表。 服务配置文件中的设置比网络配置文件中的设置具有优先权。 有关详细信息，请参阅 [Azure 服务配置架构（.cscfg 文件）](https://msdn.microsoft.com/library/azure/ee758710.aspx)。

**NetworkConfiguration 元素**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> **DnsServer** 元素中的 **name** 属性仅用作引用名称。 它不表示 DNS 服务器的主机名。 每个 **DnsServer** 属性值必须在整个 Azure 订阅中是唯一的。
> 
> 

## <a name="see-also"></a>另请参阅
[Azure 服务配置架构 (.cscfg)](https://msdn.microsoft.com/library/azure/ee758710)

[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100)

[使用网络配置文件配置虚拟网络](/virtual-network/virtual-networks-create-vnet-classic-portal)
