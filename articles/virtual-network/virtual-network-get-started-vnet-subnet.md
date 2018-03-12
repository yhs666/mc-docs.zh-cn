---
title: "创建首个 Azure 虚拟网络 | Azure"
description: "了解如何创建 Azure 虚拟网络 (VNet)、将两个虚拟机 (VM) 连接到 VNet，以及如何连接到 VM。"
services: virtual-network
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/27/2016
ms.date: 03/12/2018
ms.author: v-yeche
ms.openlocfilehash: 976a0d7134f82586ac357090d63d2925acac7c48
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="create-your-first-virtual-network"></a>创建自己的首个虚拟网络

了解如何创建包含两个子网的虚拟网络 (VNet)、如何创建两个虚拟机 (VM)，以及如何将每个 VM 连接到其中一个子网，如下图所示：

![虚拟网络关系图](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Azure 虚拟网络 (VNet) 是你自己的网络在云中的表示形式。 可以控制 Azure 网络设置并定义 DHCP 地址块、DNS 设置、安全策略和路由。 若要详细了解 VNet 概念，请阅读[虚拟网络概述](virtual-networks-overview.md)一文。 完成以下步骤创建图中所示的资源：

1. [创建包含两个子网的 VNet](#create-vnet)
2. [创建两个 VM，每个都有一个网络接口 (NIC)](#create-vms)，并将一个网络安全组 (NSG) 关联到每个 NIC
3. [连接到 VM 和从 VM 连接](#connect-to-from-vms)
4. [删除所有资源](#delete-resources)。 对于该练习中创建的某些资源，其预配时会产生费用。 为尽量削减费用，完成练习后，请务必完成本节中的步骤以删除创建的资源。

在完成本节中的步骤后，可以基本了解如何使用 VNet。 提供后续步骤是为了让用户在更深的层次详细了解如何使用 VNet。

## <a name="create-vnet"></a>创建包含两个子网的虚拟网络

若要创建包含两个子网的虚拟网络，请完成以下步骤。 通常使用不同的子网来控制子网之间的流量。

1. 登录到 [Azure 门户](<https://portal.azure.cn>)。 如果还没有帐户，可注册 [1 个月期限的试用版](https://www.azure.cn/pricing/1rmb-trial)。 
2. 单击“创建资源” > “网络” > “虚拟网络”。
4.  在“虚拟网络”边栏选项卡中，选择“Resource Manager”作为部署模型，然后单击“创建”。
5.  在显示的“创建虚拟网络”边栏选项卡中，输入以下值，然后单击“创建”：

    |**设置**|**值**|**详细信息**|
    |---|---|---|
    |**名称**|*MyVNet*|名称在资源组中必须唯一。|
    |**地址空间**|*10.0.0.0/16*|可通过 CIDR 注释指定任意地址空间。|
    |**子网名称**|*前端*|子网名称在虚拟网络中必须唯一。|
    |**子网地址范围**|*10.0.0.0/24*| 指定的范围必须位于为该虚拟网络定义的地址空间内。|
    |**订阅**|*[你的订阅]*|选择要在其中创建 VNet 的订阅。 VNet 位于单个订阅内。|
    |**资源组**|**新建：** *MyRG*|创建资源组。 资源组名称在所选订阅中必须唯一。 若要详细了解资源组，请阅读 [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fvirtual-network%2ftoc.json#resource-groups) 概述文章。|
    |**位置**|*华北*| 通常选择在物理位置上最接近位置。|

    VNet 创建需耗时几秒。 创建后，即可看到 Azure 门户仪表板。

6. 创建虚拟网络以后，在 Azure 门户的“收藏夹”窗格中单击“所有资源”。 单击“所有资源”边栏选项卡中的“MyVNet”虚拟网络。 如果所选订阅中已包含多个资源，则可在“按名称筛选…”框中输入“MyVNet”， 轻松访问 VNet。
7. “MyVNet” 边栏选项卡会打开并显示 VNet 相关信息，如下图所示：

    ![虚拟网络关系图](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. 如上图所示，单击“子网”即可显示 VNet 中子网的列表。 存在的唯一子网是在步骤 5 中创建的子网“前端”。
9. 在“MyVNet - 子网”边栏选项卡中，若要使用以下信息创建一个子网，可单击“+ 子网”，然后单击“确定”创建该子网：

    |**设置**|**值**|**详细信息**|
    |---|---|---|
    |**名称**|*后端*|名称在虚拟网络中必须唯一。|
    |**地址范围**|*10.0.1.0/24*|指定的范围必须位于为该虚拟网络定义的地址空间内。|
    |**网络安全组**和**路由表**|*无*（默认值）|本文下文中介绍了网络安全组 (NSG)。 若要详细了解用户定义的路由，请阅读[用户定义的路由](virtual-networks-udr-overview.md)一文。|

10. 将新的子网添加到 VNet 以后，即可关闭“MyVNet - 子网”边栏选项卡，然后再关闭“所有资源”边栏选项卡。

## <a name="create-vms"></a>创建虚拟机

VNet 和子网创建完毕后，即可创建 VM。 虽然两个 VM 可运行 Azure 支持的任意操作系统（包括多个不同的 Linux 分发），但在该练习中，它们都运行 Windows Server 操作系统。

### <a name="create-web-server-vm"></a>创建 Web 服务器 VM

若要创建 Web 服务器 VM，请完成以下步骤：

1. 在 [Azure 门户](https://portal.azure.cn)中，单击“创建资源” > “计算” > “Windows Server 2016 Datacenter”。
2. 单击“创建”。
3. 在显示的“基本信息”边栏选项卡中，输入或选择以下值，然后单击“确定”：

    |**设置**| **值**|**详细信息**|
    |---|---|---|
    |**名称**|*MyWebServer*|此 VM 用作 Internet 资源连接到的 Web 服务器。|
    |**VM 磁盘类型**|*SSD*|
    |**用户名**|*由用户选择*|
    |**密码和确认密码**|*由用户决定*|
    | **订阅**|*<Your subscription>*|该订阅必须等同于在本文 [创建包含两个子网的虚拟网络](#create-vnet) 部分的步骤 5 中所选的订阅。 VM 所连接到的 VNet 必须与该 VM 位于同一订阅中。|
    |**资源组**|**使用现有项:** 选择 *MyRG*|虽然使用的资源组与 VNet 所用的相同，但资源无需位于同一资源组中。|
    |**位置**|*华北*|该位置必须等同于在本文 [创建包含两个子网的虚拟网络](#create-vnet) 部分的步骤 5 中指定的位置。 VM 及其连接到的 VNet 必须位于同一位置。|

4. 在“选择大小”边栏选项卡中单击“DS1_V2 标准”，然后单击“选择”。 请阅读 [Windows VM 大小](../virtual-machines/windows/sizes.md?toc=%2fvirtual-network%2ftoc.json)一文，了解 Azure 所支持的所有 Windows VM 大小的列表。
5. 在“设置”边栏选项卡中，输入或选择以下值，然后单击“确定”：

    |**设置**|**值**|**详细信息**|
    |---|---|---|
    |**存储：使用托管磁盘**|*是*||
    |**虚拟网络**| 选择“MyVNet” |可选择要创建的 VM 所在的同一位置下的任意 VNet。 若要详细了解 VNet 和子网，请参阅[虚拟网络](virtual-networks-overview.md)一文。|
    |**子网**|选择“前端” |可选择 VNet 中的任意子网。|
    |**公共 IP 地址**|接受默认值|公共 IP 地址允许通过 Internet 连接到 VM。 若要详细了解公共 IP 地址，请阅读 [IP 地址](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)一文。|
    |**网络安全组(防火墙)**|接受默认值|单击门户创建的“(新建) MyWebServer-nsg”默认 NSG 查看其设置。 在打开的“创建网络安全组”边栏选项卡中，可以看到其中有一个入站规则，该规则允许来自任意源 IP 地址的 TCP/3389 (RDP) 流量。|
    |**其他所有值**|接受默认值|若要详细了解其余设置，请阅读[关于 VM](../virtual-machines/windows/overview.md?toc=%2fvirtual-network%2ftoc.json) 一文。|

    可以使用网络安全组 (NSG)，针对流入和流出 VM 的网络流量的类型创建入站/出站规则。 默认拒绝所有到 VM 的入站流量。 可以为生产型 Web 服务器添加更多针对 TCP/80 (HTTP) 和 TCP/443 (HTTPS) 的入站规则。 没有出站流量规则，因为默认允许所有出站流量。 可添加/删除规则，根据策略控制流量。 阅读[网络安全组](virtual-networks-nsg.md)一文，了解有关 NSG 的详细信息。

6.  在“摘要”边栏选项卡中查看设置，然后单击“确定”创建 VM。 VM 创建后，门户仪表板上显示“状态”磁贴。 创建操作可能耗时几分钟。 无需等待创建完成。 在 VM 创建期间，可继续执行下一步。

### <a name="create-database-server-vm"></a>创建数据库服务器 VM

若要创建数据库服务器 VM，请完成以下步骤：

1.  在 [Azure 门户](https://portal.azure.cn)中，单击“创建资源” > “计算” > “Windows Server 2016 Datacenter”。
2.  单击“创建”。
3.  在“基本信息”边栏选项卡中，输入或选择以下值，然后单击“确定”：

    |**设置**|**值**|**详细信息**|
    |---|---|---|
    |**名称**|*MyDBServer*|此 VM 充当 Web 服务器连接到，但 Internet 无法连接到的数据库服务器。|
    |**VM 磁盘类型**|*SSD*||
    |**用户名**|由用户选择||
    |**密码和确认密码**|由用户决定||
    |**订阅**|<Your subscription>|该订阅必须等同于在本文 [创建包含两个子网的虚拟网络](#create-vnet) 部分的步骤 5 中所选的订阅。|
    |**资源组**|**使用现有项:** 选择 *MyRG*|虽然使用的资源组与 VNet 所用的相同，但资源无需位于同一资源组中。|
    |**位置**|*华北*|该位置必须等同于在本文 [创建包含两个子网的虚拟网络](#create-vnet) 部分的步骤 5 中指定的位置。|

4.  在“选择大小”边栏选项卡中单击“DS1_V2 标准”，然后单击“选择”。
5.  在“设置”边栏选项卡中，输入或选择以下值，并单击“确定”：

    |**设置**|**值**|**详细信息**|
    |----|----|---|
    |**存储：使用托管磁盘**|*是*||
    |**虚拟网络**|选择“MyVNet” |可选择要创建的 VM 所在的同一位置下的任意 VNet。|
    |**子网**|选择“后端”，方法是：单击“子网”框，然后从“选择子网”边栏选项卡选择“后端”|可以选择 VNet 中存在的任何子网。|
    |**公共 IP 地址**|无 - 单击默认地址，然后从“选择公共 IP 地址”边栏选项卡单击“无”|若没有公共 IP 地址，就只能通过连接到同一 VNet 的其他 VM 连接到此 VM。 不能直接从 Internet 连接到该 VM。|
    |**网络安全组(防火墙)**|接受默认值| 与为 MyWebServer VM 创建的默认 NSG 一样，此 NSG 也有相同的默认入站规则。 可以为数据库服务器添加其他针对 TCP/1433 (MS SQL) 的入站规则。 没有出站流量规则，因为默认允许所有出站流量。 可添加/删除规则，根据策略控制流量。|
    |**其他所有值**|接受默认值||

6.  在“摘要”边栏选项卡中查看设置，然后单击“确定”创建 VM。 VM 创建后，门户仪表板上显示“状态”磁贴。 创建操作可能耗时几分钟。 无需等待创建完成。 在 VM 创建期间，可继续执行下一步。

## <a name="review"></a>查看资源

虽然用户创建了一个 VNet 和两个 VM，但 Azure 门户为用户在 MyRG 资源组中创建了数个其他的资源。 请完成以下步骤，查看 MyRG 资源组的内容：

1. 在 [Azure 门户](https://portal.azure.cn)中，单击“所有服务”。
2. 在包含单词“筛选”的框中键入“资源组”。 若在筛选后的列表中显示，则单击“资源组”  。
3. 在“资源组”窗格中，单击“MyRG”资源组。 如果订阅中有很多现有的资源组，则可在包含文本“按名称筛选…”的框中键入“MyRG”， 以快速访问 MyRG 资源组。
4.  在“MyRG”  边栏选项卡中，会看到资源组包含 12 个资源，如下图所示：

    ![资源组内容](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

若要详细了解 VM、磁盘和存储帐户，请阅读[虚拟机](../virtual-machines/windows/overview.md?toc=%2fvirtual-network%2ftoc.json)、[磁盘](../virtual-machines/windows/about-disks-and-vhds.md?toc=%2fvirtual-network%2ftoc.json)和[存储帐户](../storage/common/storage-introduction.md?toc=%2fvirtual-network%2ftoc.json)这三篇概述文章。 可看到门户代为创建的两个默认 NSG。 还可以看到门户创建了两个网络接口 (NIC) 资源。 VM 可以使用 NIC 通过 VNet 连接到其他资源。 若要详细了解 NIC，请阅读 [NIC](virtual-network-network-interface.md) 一文。 门户还创建了一个公共 IP 地址资源。 公共 IP 地址是针对公共 IP 地址资源的一种设置。 若要详细了解公共 IP 地址，请阅读 [IP 地址](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)一文。

## <a name="connect-to-from-vms"></a>连接到 VM

创建 VNet 和两个 VM 后，可通过完成以下各节中的步骤连接到 VM：

### <a name="connect-from-internet"></a>从 Internet 连接到 Web 服务器 VM

若要从 Internet 连接到 Web 服务器 VM，请完成以下步骤：

1. 在门户中，通过完成本文 [查看资源](#review) 部分中的步骤打开 MyRG 资源组。
2. 在“MyRG”边栏选项卡中，单击“MyWebServer”VM。
3. 在“MyWebServer”边栏选项卡中，单击“连接”，如下图所示：

    ![连接到 Web 服务器 VM](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. 允许浏览器下载“MyWebServer.rdp”  文件，并将其打开。
5. 如果收到一个对话框，显示无法验证远程连接的发布者，请单击“连接” 。
6. 输入凭据时，请确保登录时所用的用户名和密码与本文 [创建 Web 服务器 VM](#create-web-server-vm) 部分的步骤 3 中所指定的相同。 如果显示的“Windows 安全”框没有列出正确的凭据，则可能需要单击“更多选项”，然后单击“使用其他帐户”，以便指定正确的用户名和密码。 单击“确定”  以连接到 VM。
7. 如果出现一个“远程桌面连接”框，提示无法验证远程计算机的身份，则请单击“是”。
8. 现已从 Internet 连接到 MyWebServer VM。 保持远程桌面连接打开，完成下一节中的步骤。

该远程连接指向分配给公共 IP 地址资源的公共 IP 地址，其中该资源是在本文 [创建包含两个子网的虚拟网络](#create-vnet) 部分的步骤 5 中由门户创建的。 之所以允许该连接，是因为在 **MyWebServer-nsg** NSG 中创建的默认规则允许从任何源 IP 地址流向 VM 的 TCP/3389 (RDP) 入站流量。 如果尝试通过任何其他端口连接到 VM，则连接将失败，除非在允许其他端口的 NSG 中添加了其他入站规则。

>[!NOTE]
>向 NSG 添加其他入站规则时，请确保 Windows 防火墙上的相同端口是打开的，否则连接将失败。
>

### <a name="connect-to-internet"></a>从 Web 服务器 VM 连接到 Internet

若要从 Web 服务器 VM 连接到 Internet，请完成以下步骤：

1. 如果尚未打开指向 MyWebServerVM 的远程连接，请完成本文 [从 Internet 连接到 Web 服务器 VM](#connect-from-internet) 部分中的步骤，建立到 VM 的远程连接。
2. 在 Windows 桌面上，打开“Internet Explorer”。 在“设置 Internet Explorer 11”对话框中，单击“不使用推荐设置”，然后单击“确定”。 建议接受生产服务器的推荐设置。
3. 在 Internet Explorer 地址栏中，输入[bing.com](http:www.bing.com)。如果出现一个 Internet Explorer 对话框，则请单击“添加”，然后单击“受信任的站点”对话框中的“添加”，再单击“关闭”。 任何其他 Internet Explorer 对话框均重复此过程。
4. 在必应搜索页面中，输入“whatsmyipaddress” ，并单击“放大镜”按钮。 在创建 VM 时，必应会返回分配到门户创建的公共 IP 地址资源的公共 IP 地址。 如果检查“MyWebServer-ip”  资源的设置，可看到分配给公共 IP 地址资源的 IP 地址，如下图所示。 但分配到 VM 的 IP 地址不同。

    ![连接到 Web 服务器 VM](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  保持远程桌面连接打开，完成下一节中的步骤。

可通过 VM 连接到 Internet，因为默认允许来自 VM 的所有出站连接。 可将其他规则添加到应用于 NIC 的 NSG 和/或 NIC 连接到的子网，进而限制出站连接。

如果通过门户将 VM 置于“已停止”（“已解除分配”）状态，公共 IP 地址可能会更改。 如果要求公共 IP 地址永不更改，则可对 IP 地址使用静态分配方法而非动态分配方法（默认）。 若要详细了解分配方法之间的差异，请阅读 [IP 地址类型和分配方法](virtual-network-ip-addresses-overview-arm.md)一文。

### <a name="webserver-to-dbserver"></a>从 Web 服务器 VM 连接到数据库服务器 VM

若要从 Web 服务器 VM 连接到数据库服务器 VM，请完成以下步骤：

1. 如果尚未打开指向 MyWebServer VM 的远程连接，请完成本文 [从 Internet 连接到 Web 服务器 VM](#connect-from-internet) 部分中的步骤，建立连接到 VM 的远程连接。
2. 单击 Windows 桌面左下角的“开始”按钮，并开始键入“远程桌面” 。 “开始”菜单列表显示“远程桌面连接” 时单击它。
3. 在“远程桌面连接”对话框中，输入“MyDBServer”作为计算机名称，然后单击“连接”。
4. 输入在本文[创建数据库服务器 VM](#create-database-server-vm) 部分的步骤 3 中输入的用户名和密码，然后单击“确定”。
5. 如果收到一个对话框，显示无法验证远程计算机的标识，请单击“是” 。
6. 将到两个服务器的远程桌面连接保持打开状态，完成下一节中的步骤。

可建立从 Web 服务器 VM 到数据库服务器 VM 的连接，原因如下：

- 对于本文 [创建数据库服务器 VM](#create-database-server-vm) 部分的步骤 5 中创建的默认 NSG 中的任何源 IP，启用 TCP/3389 入站连接。
- 启用了来自 Web 服务器 VM 的连接，其与数据库服务器 VM 连接到同一个 VNet。 若要连接到未分配有公共 IP 地址的 VM，必须从连接到同一 VNet 的其他 VM 进行连接，即使该 VM 连接到其他子网也是如此。
- 尽管 VM 连接到不同子网，Azure 仍会创建实现子网间连接的默认路由。 但是，可通过自行创建路由覆盖默认路由。 若要详细了解 Azure 中的路由，请阅读[用户定义的路由](virtual-networks-udr-overview.md)一文。

如果用户尝试从 Internet 启动到数据库服务器 VM 的远程连接，就像在本文[从 Internet 连接到 Web 服务器 VM](#connect-from-internet) 部分那样操作，则会看到“连接”选项灰显。灰显的原因是，未向 VM 分配公共 IP 地址，因此无法建立从 Internet 到该 VM 的入站连接。

### <a name="connect-to-the-internet-from-the-database-server-vm"></a>从数据库服务器 VM 连接到 Internet

请完成以下步骤，建立从数据库服务器 VM 到 Internet 的出站连接：

1. 如果尚未打开从 MyWebServer VM 到 MyDBServer VM 的远程连接，请完成本文 [从 Web 服务器 VM 连接到 数据库服务器 VM](#webserver-to-dbserver) 部分中的步骤。
2. 在 Windows 桌面的 MyDBServer VM 上，打开 Internet Explorer 并响应对话框，如本文 [从 Web 服务器 VM 连接到 Internet](#connect-to-internet) 部分的步骤 2 和 3 中所述。
3. 在地址栏中输入[bing.com](http:www.bing.com) 。
4. 在出现的 Internet Explorer 对话框中单击“添加”，然后再在“受信任的站点”对话框中单击“添加”和“关闭”。 在显示的所有其他对话框中完成这些步骤。
5. 在必应搜索页面中，输入“whatsmyipaddress” ，并单击“放大镜”按钮。 必应返回当前由 Azure 基础结构分配给 VM 的公共 IP 地址。 6. 关闭从 MyWebServer VM 到 MyDBServer VM 的远程桌面连接，然后关闭到 MyWebServer VM 的远程连接。

之所以允许到 Internet 的出站连接，是因为所有出站流量都是默认允许的，即使未向 MyDBServer VM 分配公共 IP 地址资源。 默认情况下，无论 VM 是否分配有公共 IP 地址资源，所有 VM 均可出站连接到 Internet。 但是，无法从 Internet 连接到公共 IP 地址，正如可为分配有公共 IP 地址资源的 MyWebServer VM 建立连接一样。

## <a name="delete-resources"></a>删除所有资源

若要删除本文中创建的所有资源，请完成以下步骤：

1. 若要查看本文中创建的 MyRG 资源组，请完成本文 [查看资源](#review) 部分中的步骤 1-3。 再次查看资源组中的资源。 如果按照前面的步骤创建了 MyRG 资源组，则会看到图中显示的步骤 4 的 12 项资源。
2. 在“MyRG”边栏选项卡中，单击“删除”  按钮。
3. 门户将要求键入资源组的名称以确认要将其删除。 如果看到的资源不同于在本文[查看资源](#review)部分的步骤 4 中显示的资源，请单击“取消”。 如果只看到在本文中创建的 12 项资源，则请键入 *MyRG* 作为资源组名称，然后单击“删除”。 资源组将连同其内附的所有资源一并删除，因此在删除前，请始终要确认资源组的内容。 门户将删除资源组中包含的所有资源，然后删除资源组本身。 此过程需耗时几分钟。

## <a name="next-steps"></a>后续步骤

在此练习中，创建了一个 VNet 和两个 VM。 在 VM 创建期间指定了一些自定义设置，并接受了多个默认设置。 建议在部署生产 VNet 和 VM 之前阅读以下文章，确保了解所有可用设置：

- [虚拟网络](virtual-networks-overview.md)
- [公共 IP 地址](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [网络接口](virtual-network-network-interface.md)
- [网络安全组](virtual-networks-nsg.md)
- [虚拟机](../virtual-machines/windows/overview.md?toc=%2fvirtual-network%2ftoc.json)

<!--Update_Description: wording update, update reference link-->
