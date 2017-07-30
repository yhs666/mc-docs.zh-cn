---
title: "Azure 云服务的常见连接和网络问题解答 | Azure"
description: "本文列出有关 Microsoft Azure 云服务的常见连接和网络问题。"
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/10/2017
ms.author: v-yiso
ms.date: 07/31/2017
ms.openlocfilehash: f350647b4b3b200060e99bbc0b3433434ca25cda
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure 云服务的连接和网络问题：常见问题解答 (FAQ)

本文包含 [Azure 云服务](/cloud-services/)的常见连接和网络问题。 还可以参阅[云服务 VM 大小页面](./cloud-services-sizes-specs.md)，了解大小信息。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>无法在多 VIP 云服务中保留 IP
首先，请确保已打开想要为其保留 IP 的虚拟机实例。 其次，请确保为过渡和生产部署使用保留 IP。 **请勿**在部署升级过程中更改设置。

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>设置了 NSG 时，如何使用远程桌面？
将规则添加到 NSG，允许端口 **3389** 和 **20000** 上的流量。  远程桌面使用端口 **3389**。  云服务实例经过负载均衡，因此无法直接控制要连接到哪个实例。  RemoteForwarder 和 RemoteAccess 代理管理 RDP 流量，允许客户端发送 RDP cookie 和指定要连接到的单个实例。  *RemoteForwarder* 和 *RemoteAccess* 代理要求打开端口 **20000**（如果创建了 NSG，此端口可能已被阻止）。

## <a name="can-i-ping-a-cloud-service"></a>是否可以 ping 云服务？

否，使用普通的 "ping"/ ICMP 协议是行不通的。 不允许通过 Azure 负载均衡器使用 ICMP 协议。

若要测试连接，我们建议执行端口 ping。 Ping.exe 使用 ICMP，而 PSPing、Nmap 和 telnet 等其他工具可以测试特定 TCP 端口的连接。

有关详细信息，请参阅[使用端口 ping 而不是 ICMP 来测试 Azure VM 连接](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/)。

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>如何阻止未知 IP 地址向云服务发起的、可能表示某种恶意攻击的数千次访问？
Azure 实施多层网络安全性来防范其平台服务遭到分布式拒绝服务 (DDoS) 攻击。 Azure DDoS 防御系统是 Azure 持续监视过程的一部分，已通过渗透测试持续得到改善。 此 DDoS 防御系统不仅能够承受外部的攻击，而且也能承受其他 Azure 租户的攻击。 有关更多详细信息，请参阅 [Microsoft Azure 网络安全](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf)。

还可以创建一个启动任务，以便有选择性地阻止某些特定的 IP 地址。 有关详细信息，请参阅[阻止特定 IP 地址](./cloud-services-startup-tasks-common.md#block-a-specific-ip-address)。

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>尝试通过 RDP 连接到云服务实例时收到消息“用户帐户已过期”。
如果绕过 RDP 设置中配置的过期日期，则可能会收到错误消息“此用户帐户已过期”。 可以在门户中执行以下步骤来更改过期日期：
1. 登录到 Azure 管理控制台 (https://manage.windowsazure.cn)，导航到你的云服务，然后选择“配置”选项卡。
2. 选择“远程”。
3. 更改“过期”日期，然后保存配置。

现在，应该能够通过 RDP 连接到计算机。

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>负载均衡器为何不能以均等的方式均衡流量？
有关内部负载均衡器工作原理的信息，请参阅 [Azure 负载均衡器的新分配模式](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/)。

使用的分配算法是将流量映射到可用服务器的 5 元组（源 IP、源端口、目标 IP、目标端口和协议类型）哈希。 它仅在传输会话内部提供粘性。 同一 TCP 或 UDP 会话中的数据包会定向到负载均衡终结点后面的同一数据中心 IP (DIP) 实例。 客户端从同一源 IP 关闭再重新打开连接或启动新会话时，源端口会更改，并导致流量定向到其他 DIP 终结点。


<!--Update_Description: update meta data only-->