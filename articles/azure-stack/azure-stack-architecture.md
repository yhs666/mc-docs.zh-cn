---
title: "Azure Stack 开发工具包体系结构 | Microsoft Docs"
description: "查看 Azure Stack 开发工具包体系结构。"
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/21/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.reviewer: unknown
ms.openlocfilehash: d90779d2ffc42b72aa5fd380a13c0260cabac1c0
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="azure-stack-development-kit-architecture"></a>Azure Stack 开发工具包体系结构

*适用于：Azure Stack 开发工具包*

Azure Stack 开发工具包是单节点部署的 Azure Stack。 所有组件安装在单主机计算机上运行的虚拟机中。 

## <a name="logical-architecture-diagram"></a>逻辑体系结构示意图
下图演示了 Azure Stack 开发工具包及其组件的逻辑体系结构。

![](./media/azure-stack-architecture/image1.png)

## <a name="virtual-machine-roles"></a>虚拟机角色
Azure Stack 开发工具包提供在主机上使用以下 VM 的服务：

| Name | 说明 |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack 存储服务。|
| **AzS-ADFS01** | Active Directory 联合身份验证服务 (ADFS)。  |
| **AzS-BGPNAT01** | 边缘路由器，为 Azure Stack 提供 NAT 和 VPN 功能。 |
| **AzS-CA01** | Azure Stack 角色服务的证书颁发机构服务。|
| **AzS-DC01** | Azure Stack 的 Active Directory、DNS 和 DHCP 服务。|
| **AzS-ERCS01** | 紧急恢复控制台 VM。 |
| **AzS-GWY01** | 边缘网关服务，例如租户网络的 VPN 站点到站点连接。|
| **AzS-NC01** | 用于管理 Azure Stack 网络服务的网络控制器。  |
| **AzS-SLB01** | Azure Stack 中用于租户和 Azure Stack 基础结构服务的负载均衡多路复用器服务。  |
| **AzS-SQL01** | Azure Stack 基础结构角色的内部数据存储。  |
| **AzS-WAS01** | Azure Stack 管理门户和 Azure 资源管理器服务。|
| **AzS-WASP01**| Azure Stack 用户（租户）门户和 Azure 资源管理器服务。|
| **AzS-XRP01** | Azure Stack 的基础结构管理控制器，包括计算、网络和存储资源提供程序。|


## <a name="next-steps"></a>后续步骤
[部署 Azure Stack](azure-stack-deploy.md)

[尝试的第一个方案](azure-stack-first-scenarios.md)


