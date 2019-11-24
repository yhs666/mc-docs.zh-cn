---
title: 配置 Azure Stack 的时间服务器 | Microsoft Docs
description: 了解如何配置 Azure Stack 的时间服务器。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/10/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: thoroet
ms.lastreviewed: 10/10/2019
ms.openlocfilehash: 512b7d5563e6f1e9f6fd5c4d6e8c607e96517441
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020491"
---
# <a name="configure-the-time-server-for-azure-stack"></a>配置 Azure Stack 的时间服务器

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*  

可以使用特权终结点 (PEP) 来更新 Azure Stack 中的时间服务器。 使用一个可以解析成两个或多个 NTP 服务器 IP 地址的主机名。

Azure Stack 使用网络时间协议 (NTP) 连接到 Internet 上的时间服务器。 NTP 服务器提供准确的系统时间。 Azure Stack 的物理网络交换机、硬件生命周期主机、基础结构服务和虚拟机都使用时间。 如果时钟未同步，Azure Stack 可能会遇到严重的网络和身份验证问题。 在创建日志文件、文档和其他文件时，时间戳可能会不正确。

必须提供一个时间服务器 (NTP)，这样 Azure Stack 才能同步时间。 部署 Azure Stack 时，请提供 NTP 服务器的地址。 时间是重要的数据中心基础结构服务。 如果服务更改，则需更新时间。

> [!NOTE]
> Azure Stack 支持只与一个时间服务器 (NTP) 同步时间。 不能提供多个 NTP 供 Azure Stack 与其同步时间。

## <a name="configure-time"></a>配置时间

1. [连接到 PEP](azure-stack-privileged-endpoint.md)。 
    > [!Note]  
    > 不需开具支持票证来解锁特权终结点。

2. 运行以下命令即可查看当前的已配置 NTP 服务器：

    ```PowerShell
    Get-AzsTimeSource
    ```

3. 运行以下命令即可更新 Azure Stack，以便使用新的 NTP 服务器并立即同步时间。

    > [!Note]  
    > 此过程不更新物理交换机上的时间服务器

    ```PowerShell
    Set-AzsTimeSource -TimeServer NEWTIMESERVERIP -resync
    ```

4. 请查看命令输出中是否有错误。


## <a name="next-steps"></a>后续步骤

[查看就绪性报表](azure-stack-validation-report.md)  
[有关 Azure Stack 集成的一般注意事项](azure-stack-datacenter-integration.md)  
