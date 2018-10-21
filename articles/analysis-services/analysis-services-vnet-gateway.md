---
title: 对 Azure 虚拟网络数据源使用本地数据网关 | Azure
description: 了解如何配置服务器以为 VNet 上的数据源使用网关。
author: rockboyfor
manager: digimobile
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 07/03/2018
ms.date: 10/22/2018
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: 6f47a0f0e0165a9813074cae1513359a4a703538
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453973"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>为 Azure 虚拟网络 (VNet) 上的数据源使用网关

本文介绍了当数据源位于 [Azure 虚拟网络 (VNet)](../virtual-network/virtual-networks-overview.md) 上时要使用的 **AlwaysUseGateway** 服务器属性。

## <a name="server-access-to-vnet-data-sources"></a>对 VNet 数据源的服务器访问

如果通过 VNet 访问数据源，则 Azure Analysis Services 服务器必须连接到那些数据源，就像它们位于本地自己的环境中一样。 可以配置 **AlwaysUseGateway** 服务器属性来指定服务器通过[本地网关](analysis-services-gateway.md)访问所有数据源数据。 

> [!NOTE]
> 仅当安装并配置了[本地数据网关](analysis-services-gateway.md)时此属性才有效。 网关可以在 VNet 上。

## <a name="configure-alwaysusegateway-property"></a>配置 AlwaysUseGateway 属性

1. 在“SSMS”>“服务器”>“属性” > “常规”中，选择“显示高级(全部)属性”。
2. 在“ASPaaS\AlwaysUseGateway”中，选择“true”。

    ![始终使用网关属性](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)

## <a name="see-also"></a>另请参阅
[连接到本地数据源](analysis-services-gateway.md)   
[安装并配置本地数据网关](analysis-services-gateway-install.md)   
[Azure 虚拟网络 (VNET)](../virtual-network/virtual-networks-overview.md)

<!-- Update_Description: new articles on analysis services vnet gateway  -->
<!--ms.date: 10/22/2018-->