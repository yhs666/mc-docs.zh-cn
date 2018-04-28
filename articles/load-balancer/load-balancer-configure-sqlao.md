---
title: 为 SQL Server Always On 配置负载均衡器 | Azure
description: 介绍如何将负载均衡器配置为适用于 SQL Server Always On，以及如何使用 PowerShell 为 SQL Server 实现创建负载均衡器
services: load-balancer
documentationcenter: na
author: rockboyfor
manager: digimobile
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/25/2017
ms.date: 04/30/2018
ms.author: v-yeche
ms.openlocfilehash: 3cbbf4a2874edd2046069b1470ff472c575a3f84
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-load-balancer-for-sql-server-always-on"></a>为 SQL Server Always On 配置负载均衡器

SQL Server Always On 可用性组现在可以与内部负载均衡器一起运行。 可用性组是 SQL Server 用于实现高可用性和灾难恢复的旗舰解决方案。 无论配置中的副本数目是多少，可用性组侦听器都可让客户端应用程序无缝连接到主副本。

侦听器 (DNS) 名称映射到负载均衡的 IP 地址。 Azure 负载均衡器只将传入流量定向到副本集中的主服务器。

可以使用 SQL Server Always On（侦听器）终结点的内部负载均衡器支持。 现在可以控制侦听器的可访问性。 可以从虚拟网络中的特定子网选择负载均衡的 IP 地址。

如果在侦听器上使用内部负载均衡器，只有以下项可以访问 SQL Server 终结点（例如 Server=tcp:ListenerName,1433;Database=DatabaseName）：

* 同一虚拟网络中的服务和 VM。
* 来自连接的本地网络的服务和 VM。
* 来自互连的虚拟网络的服务和 VM。

![内部负载均衡器 SQL Server Always On](./media/load-balancer-configure-sqlao/sqlao1.png)

## <a name="add-an-internal-load-balancer-to-the-service"></a>将内部负载均衡器添加到服务

1. 在以下示例中，请配置包含名为“Subnet-1”的子网的虚拟网络：

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. 在每个 VM 上为内部负载均衡器添加负载均衡的终结点。

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    在前面的示例中，你有两个 VM，分别名为“sqlsvc1”和“sqlsvc2”，在云服务“Sqlsvc”中运行。 使用 `DirectServerReturn` 开关创建内部负载均衡器以后，请将负载均衡的终结点添加到内部负载均衡器。 负载均衡的终结点允许 SQL Server 为可用性组配置侦听器。

有关 SQL Server Always On 的详细信息，请参阅[在 Azure 中为 Always On 可用性组配置内部负载均衡器](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md)。

## <a name="see-also"></a>另请参阅
* [开始配置公共负载均衡器](load-balancer-get-started-internet-arm-ps.md)
* [开始配置内部负载均衡器](load-balancer-get-started-ilb-arm-ps.md)
* [配置负载均衡器分发模式](load-balancer-distribution-mode.md)
* [配置负载均衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
<!-- Update_Description: update meta properties  -->