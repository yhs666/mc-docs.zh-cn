---
title: include 文件
description: include 文件
services: virtual-machines-windows
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: include
origin.date: 05/17/2018
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 4900c6296ed0fc22b7163091e5e8ac495ae277b6
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34723123"
---
每个终结点都有一个公用端口和一个专用端口：

* Azure 负载均衡器使用公用端口侦听从 Internet 传入的虚拟机流量。
* 虚拟机使用专用端口侦听传入流量（通常发送到虚拟机上运行的应用程序或服务）。

使用 Azure 门户创建终结点时，将为 IP 协议和众所周知的网络协议的 TCP 或 UDP 端口提供默认值。 对于自定义终结点，需要指定正确的 IP 协议（TCP 或 UDP），以及公用和专用端口。 要将传入流量随机分布到多个虚拟机上，必须创建包含多个终结点的负载均衡集。

创建终结点后，可以使用访问控制列表 (ACL) 定义规则，根据传入流量的源 IP 地址允许或拒绝终结点的公用端口的传入流量。 但是，如果虚拟机在 Azure 虚拟网络中，则应改为使用网络安全组。 有关详细信息，请参阅[关于网络安全组](../articles/virtual-network/security-overview.md)。

> [!NOTE]
> 对与 Azure 自动设置的远程连接终结点关联的端口自动完成 Azure 虚拟机的防火墙配置。 对于为所有其他终结点指定的端口，不会为虚拟机的防火墙自动进行任何配置。 为虚拟机创建终结点时，需要确保虚拟机的防火墙也允许与终结点配置对应的协议和专用端口的流量。 若要配置防火墙，请参阅有关在虚拟机上运行的操作系统的文档或联机帮助。
>
>

## <a name="create-an-endpoint"></a>创建终结点
1. 如果你尚未登录 [Azure 门户](https://portal.azure.cn)，请先登录。
2. 单击“虚拟机”，并单击要配置的虚拟机的名称 。
3. 单击“设置”组中的“终结点”。 “终结点”页面列出虚拟机的所有当前终结点  。 （此示例为 Windows VM。 Linux VM 将默认显示 SSH 终结点。）

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![终结点](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. 在终结点条目上方的命令栏中，单击“添加”。
5. 在“添加终结点”页面的“名称”中，键入终结点的名称。
6. 在“协议”中，选择“TCP”或“UDP”。
7. 在“公用端口”中，键入来自 Internet 的传入流量的端口号。 在“专用端口”中，键入虚拟机正在侦听的端口号 。 这些端口号可以是不同的。 确保已将虚拟机的防火墙配置为允许与协议（在步骤 6 中）和专用端口对应的流量。
10. 单击“确定” 。

新的终结点会在“终结点”页上列出  。

![成功创建终结点](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>管理终结点上的 ACL
若要定义一组可以发送流量的计算机，终结点上的 ACL 可以基于源 IP 地址限制流量。 按照下列步骤在终结点上添加、修改或删除 ACL。

> [!NOTE]
> 如果终结点是负载均衡集的一部分，则你对一个终结点上的 ACL 做出的任何更改都将应用于该集中的所有终结点。
>
>

如果虚拟机在 Azure 虚拟网络中，则建议使用网络安全组（而不是 ACL）。 有关详细信息，请参阅[关于网络安全组](../articles/virtual-network/security-overview.md)。

1. 如果尚未登录 Azure 门户，请先登录。
2. 单击“虚拟机”，并单击要配置的虚拟机的名称 。
3. 单击“终结点” 。 从列表中选择适当的终结点。 ACL 列表位于页面底部。

   ![指定 ACL 详细信息](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. 使用列表中的行为 ACL 添加、删除或编辑规则，并更改其顺序。 
            **远程子网** 值是从 Internet 传入流量的 IP 地址范围，Azure 负载均衡器使用该值根据流量的源 IP 地址允许或拒绝传入流量。 请务必以 CIDR 格式（也称为地址前缀格式）指定 IP 地址范围。 例如 `10.1.0.0/8`。

 ![新的 ACL 条目](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)

可以使用规则只允许来自与 Internet 上计算机对应的特定计算机的流量，或拒绝来自特定已知地址范围的流量。

按照从第一个规则开始并以最后一个规则结束的顺序评估规则。 这意味着规则应按最少限制到最多限制排序。 有关示例和更多信息，请参阅[什么是网络访问控制列表](../articles/virtual-network/virtual-networks-acl.md)。
