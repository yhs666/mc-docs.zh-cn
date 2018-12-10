---
title: include 文件
description: include 文件
services: virtual-machines-windows
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: include
origin.date: 10/23/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 4f9d489b6693c6c4ddc2c6abe08fcc2b5ca05a58
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675842"
---
每个终结点都有一个公用端口和一个专用端口：

* Azure 负载均衡器使用公用端口侦听从 Internet 传入的虚拟机流量。
* 虚拟机使用专用端口侦听通常发送到虚拟机上运行的应用程序或服务的传入流量。

使用 Azure 门户创建终结点时，将为 IP 协议和众所周知的网络协议的 TCP 或 UDP 端口提供默认值。 对于自定义终结点，请指定正确的 IP 协议（TCP 或 UDP），以及公用和专用端口。 若要将传入流量随机分布到多个虚拟机，请创建包含多个终结点的负载均衡集。

创建终结点后，可以使用访问控制列表 (ACL) 定义规则，根据传入流量的源 IP 地址允许或拒绝终结点的公用端口的传入流量。 但是，如果虚拟机位于 Azure 虚拟网络中，请改用网络安全组。 有关详细信息，请参阅[关于网络安全组](../articles/virtual-network/security-overview.md)。

> [!NOTE]
> 对与 Azure 自动设置的远程连接终结点关联的端口自动完成 Azure 虚拟机的防火墙配置。 对于为所有其他终结点指定的端口，不会为虚拟机的防火墙自动进行任何配置。 为虚拟机创建终结点时，请确保虚拟机的防火墙也允许与终结点配置对应的协议和专用端口的流量。 若要配置防火墙，请参阅有关在虚拟机上运行的操作系统的文档或联机帮助。
>
>

## <a name="create-an-endpoint"></a>创建终结点
1. 登录到 [Azure 门户](https://portal.azure.cn)。

2. 选择“虚拟机”，然后选择要配置的虚拟机。

3. 在“设置”组中选择“终结点”。 此时会显示“终结点”页，其中列出了虚拟机的所有当前终结点。 （本示例适用于 Windows VM。 Linux VM 将默认显示 SSH 终结点。）

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![终结点](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. 在终结点条目上方的命令栏中，选择“添加”。 此时会显示“添加终结点”页。

5. 对于“名称”，请输入终结点的名称。

6. 对于“协议”，请选择“TCP”或“UDP”。

7. 对于“公共端口”，请输入来自 Internet 的传入流量的端口号。 

8. 对于“专用端口”，请输入虚拟机正在侦听的端口号。 公共和专用端口号可以不同。 确保已将虚拟机的防火墙配置为允许与协议和专用端口对应的流量。

9. 选择“确定” 。

新终结点将在“终结点”页上列出。

![成功创建终结点](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>管理终结点上的 ACL
若要定义一组可以发送流量的计算机，终结点上的 ACL 可以基于源 IP 地址限制流量。 按照下列步骤在终结点上添加、修改或删除 ACL。

> [!NOTE]
> 如果终结点是负载均衡集的一部分，则你对一个终结点上的 ACL 做出的任何更改都将应用于该集中的所有终结点。
>
>

如果虚拟机位于 Azure 虚拟网络中，请使用网络安全组，而不要使用 ACL。 有关详细信息，请参阅[关于网络安全组](../articles/virtual-network/security-overview.md)。

1. 登录到 Azure 门户。

2. 选择“虚拟机”，然后选择要配置的虚拟机的名称。

3. 选择“终结点”。 在终结点列表中选择相应的终结点。 ACL 列表位于页面底部。

   ![指定 ACL 详细信息](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. 使用列表中的行为 ACL 添加、删除或编辑规则，并更改其顺序。 “远程子网”值是从 Internet 传入流量的 IP 地址范围，Azure 负载均衡器将使用该值根据流量的源 IP 地址允许或拒绝传入流量。 请务必以无类域间路由 (CIDR) 格式（也称为地址前缀格式）指定 IP 地址范围。 例如，`10.1.0.0/8`。

 ![新的 ACL 条目](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)

可以使用规则只允许来自与 Internet 上计算机对应的特定计算机的流量，或拒绝来自特定已知地址范围的流量。

将按以第一条规则开始、以最后一条规则结束的顺序来计算规则。 因此，规则应按最低限制到最高限制排序。 有关详细信息，请参阅[什么是网络访问控制列表](../articles/virtual-network/virtual-networks-acl.md)。

<!-- Update_Description: update meta properties, wording update -->