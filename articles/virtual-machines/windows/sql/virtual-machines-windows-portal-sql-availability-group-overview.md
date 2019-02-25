---
title: SQL Server 可用性组 - Azure 虚拟机 - 概述 | Azure
description: 本文介绍 Azure 虚拟机上的 SQL Server 可用性组。
services: virtual-machines
documentationCenter: na
author: rockboyfor
manager: digimobile
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 01/13/2017
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: e7845416f706dd2a19b9ac52e44e5437cc7add00
ms.sourcegitcommit: dd6cee8483c02c18fd46417d5d3bcc2cfdaf7db4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666091"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Azure 虚拟机上的 SQL Server AlwaysOn 可用性组简介 #

本文介绍 Azure 虚拟机上的 SQL Server 可用性组。 

Azure 虚拟机上的 AlwaysOn 可用性组类似于本地的 AlwaysOn 可用性组。 有关详细信息，请参阅 [Always On 可用性组 (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx)。 

下图阐述了 Azure 虚拟机中完整 SQL Server 可用性组的各个部分。

![可用性组](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Azure 虚拟机中可用性组的主要区别是 Azure 虚拟机需要[负载均衡器](../../../load-balancer/load-balancer-overview.md)。 负载均衡器保存可用性组侦听器的 IP 地址。 如果有多个可用性组，则每个组都需要一个侦听器。 一个负载均衡器可以支持多个侦听器。

准备好在 Azure 虚拟机上生成 SQL Server 可用性组时，请参阅这些教程。

<!--Not Available on ## Automatically create an availability group from a template-->
<!-- Not Available on [Configure Always On availability group in Azure VM automatically - Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)-->

## <a name="manually-create-an-availability-group-in-azure-portal"></a>在 Azure 门户中手动创建可用性组

还可以自行创建虚拟机，不需模板。 首先完成先决条件，然后创建可用性组。 请参阅以下主题： 

- [在 Azure 虚拟机上配置 SQL Server Always On 可用性组的先决条件](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [创建 Always On 可用性组以提高可用性和灾难恢复能力](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>后续步骤

[在不同区域中的 Azure 虚拟机上配置 SQL Server Always On 可用性组](virtual-machines-windows-portal-sql-availability-group-dr.md)。

<!-- Update_Description: wording update, update link -->