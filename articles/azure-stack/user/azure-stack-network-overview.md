---
title: Azure Stack 网络简介
description: 了解 Azure Stack 网络
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: get-started-article
origin.date: 02/28/2018
ms.date: 03/27/2018
ms.author: v-junlch
ms.openlocfilehash: 6376c7516d72d0dfff57563be3c85135ea415279
ms.sourcegitcommit: 6d7f98c83372c978ac4030d3935c9829d6415bf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="introduction-to-azure-stack-networking"></a>Azure Stack 网络简介

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 提供可以结合使用或单独使用的各种网络功能：
- Azure Stack 资源之间的连接：在云中的安全专用虚拟网络内将 Azure 资源连接在一起。
- Internet 连接：通过 Internet 在 Azure 资源之间进行通信。
- 本地连接：在 Internet 上通过虚拟专用网络 (VPN) 或者通过与 Azure Stack 建立的专用连接将本地网络连接到 Azure Stack 资源。
- 负载均衡和流量方向：对发往同一位置中的服务器的流量进行负载均衡，并将流量定向到不同位置中的服务器。
- 安全性：筛选网络子网或单个虚拟机 (VM) 之间的网络流量。
- 路由：使用默认路由，或者完全控制 Azure Stack 与本地资源之间的路由。
- 可管理性：监视和管理 Azure Stack 网络资源。
- 部署和配置工具：使用基于 Web 的门户或跨平台命令行工具来部署和配置网络资源。


## <a name="next-steps"></a>后续步骤
- [Azure Stack 网络注意事项](azure-stack-network-differences.md)


<!-- Update_Description: update metedata properties -->