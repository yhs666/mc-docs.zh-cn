---
title: 如何在 Azure Stack 上扩展数据中心 | Microsoft Docs
description: 了解如何在 Azure Stack 上扩展数据中心。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: how-to
origin.date: 10/19/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 10/19/2019
ms.openlocfilehash: 6e7945d4a789a96292e34d089155284f42d66a26
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020585"
---
# <a name="how-to-extend-the-data-center-on-azure-stack"></a>如何在 Azure Stack 上扩展数据中心

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

本文提供 Azure Stack 存储基础架构信息，可帮助你确定如何将 Azure Stack 集成到现有的网络环境。 Azure Stack 上的存储空间有限。 可将存储与 Azure Stack 外部的存储集成，也可将存储集成到本地数据中心。 在大致介绍如何扩展数据中心后，本文将演示两种不同的方案。 可以连接到 Windows 文件存储服务器。 也可以连接到 Windows iSCSI 服务器。

## <a name="overview-of-extending-storage-to-azure-stack"></a>将存储扩展到 Azure Stack 的概述

下图描绘了一种方案，其中，运行工作负荷的单个虚拟机连接并利用外部（在 VM 和 Azure Stack 本身的外部）存储来读取/写入数据。本文重点说明简单的文件检索，但你也可以扩展本示例，使其适用于更复杂的方案，例如远程存储数据库文件。

![Azure Stack 扩展存储](./media/azure-stack-network-howto-extend-datacenter/image1.png)

在上图中可以看到，Azure Stack 系统上已部署了包含多个 NIC 的 VM。 无论是从冗余还是存储最佳做法的立场来看，目标与目的地之间都必须有多个路径。 让情况变得更为复杂的是，Azure Stack 中的 VM 同时有公共和专用 IP，就像在 Azure 中的情况一样。 如果外部存储必须连接到该 VM，就只能通过公共 IP 进行连接，因为专用 IP 主要用于 Azure Stack 系统内部的 vNet 和子网。 外部存储无法与 VM 的专用 IP 空间通信，除非它能通过站点到站点 VPN 连接到 vNet 本身。 因此，本示例主要通过公共 IP 空间进行通信。 请注意，在图中的公共 IP 空间内，有 2 个不同的公共 IP 池子网。 默认情况下，Azure Stack 只需要一个用于公共 IP 地址的池即可，但考虑到冗余路由，可以添加另一个池。 但是，由于无法选择特定池中的 IP 地址，因此 VM 最后可能具有来自同一池、但跨多个虚拟网络卡的公共 IP。

在本示例中，可以假设我们要处理边界设备与外部存储之间的路由，而流量可以正常遍历网络。 在本示例中，主干是 1 GbE、10 GbE、25 GbE 还是更快的速度并不重要，但在规划集成时必须考虑到这一点，使所有应用程序获得所需的性能来访问此外部存储。

## <a name="connect-to-a-windows-server-file-server-storage"></a>连接到 Windows Server 文件服务器存储

在此方案中，需在 Azure Stack 上部署并配置 Windows Server 2019 虚拟机，并让其准备好连接到外部文件服务器（也可以运行 Windows Server 2019）。 在适当的情况下，请设置 SMB 多通道等重要功能，以优化性能，以及 VM 与外部存储之间的连接。

### <a name="deploy-the-windows-server-2019-vm-on-azure-stack"></a>在 Azure Stack 上部署 Windows Server 2019 VM

1.  在“Azure Stack 管理员门户”中，假设已正确注册此系统并已将其连接到市场，请选择“市场管理”；然后，假设尚未创建 Windows Server 2019 映像，请选择“从 Azure 添加”，并搜索“Windows Server 2019”，以添加“Windows Server 2019 Datacenter”映像。     

    ![](./media/azure-stack-network-howto-extend-datacenter/image2.png)

    下载 Windows Server 2019 映像可能需要一段时间。

2.  在 Azure Stack 环境中包含 Windows Server 2019 映像后，登录到 Azure Stack 用户门户**。

3.  登录到 Azure Stack 用户门户后，确保具有[某个套餐的订阅](/azure-stack/operator/azure-stack-subscribe-plan-provision-vm?view=azs-1908)可用于预配 IaaS 资源（计算、存储和网络）。

4.  获得可用的订阅后，返回 Azure Stack 用户门户中的**仪表板**，并依次选择“创建资源”、“计算”、“Windows Server 2019 Datacenter 库项”。   

5.  在“基本”  边栏选项卡上：

    a.  **名称**：VM001

    b.  **用户名**：localadmin

    c.  **密码**和**确认密码**：\<所选的密码>

    d.  **订阅**：\<包含计算/存储/网络资源的所选订阅>

    e. **资源组**：新建：storagetesting

    f.  然后选择“确定” 

6.  在“选择大小”边栏选项卡上，选择“Standard_F8s_v2”。  

7.  在“设置”边栏选项卡上选择“虚拟网络”，在“创建虚拟网络”边栏选项卡上，将地址空间调整为 **10.10.10.0/23** 并将子网地址范围更新为 **10.10.10.0/24**，然后选择“确定”。    

8.  选择“公共 IP 地址”，然后在“创建公共 IP 地址”边栏选项卡中选择“静态”。   

9.  在“选择公共入站端口”中选择“RDP (3389)”。  

10. 保留其他默认值，然后选择“确定”。 
    
    ![](./media/azure-stack-network-howto-extend-datacenter/image3.png)

11. 阅读摘要、等待验证，然后选择“确定”开始部署。  部署应该可在大约 10 分钟内完成。

12. 完成部署后，在“资源”下，选择虚拟机名称 **VM001** 转到“概述”边栏选项卡。  
    
    ![](./media/azure-stack-network-howto-extend-datacenter/image4.png)

13. 在“DNS 名称”下，选择“配置”并提供 DNS 名称标签 **vm001**，选择“保存”，然后选择痕迹导航中的“VM001”返回“概述”边栏选项卡。    

14. 在“概述”边栏选项卡的右侧，选择“虚拟网络/子网”文本下的“storagetesting-vnet/default”。 

15. 在“storagetesting-vnet”边栏选项卡中，依次选择“子网”、“+子网”，然后在新的“添加子网”边栏选项卡中输入以下信息：   

    a.  **名称**：subnet2

    b.  **地址范围(CIDR 块)** ：10.10.11.0/24

    c. **网络安全组**：无

    d.  **路由表**：无

    e. 选择“确定”： 

16. 保存后，选择痕迹导航中的“VM001”返回“概述”边栏选项卡。 

17. 选择“网络”  。

18. 依次选择“链接网络接口”、“创建网络接口”。  

19. 在“创建网络接口”边栏选项卡上： 

    a.  **名称**：vm001nic2

    b.  **子网**：确保子网为 10.10.11.0/24

    c. **网络安全组**：VM001-nsg

    d.  **资源组**：storagetesting

20. 成功创建后，选择痕迹导航中的“VM001”，然后选择“停止”以关闭 VM。  

21. VM 停止（解除分配）后，依次选择“网络”、“附加网络接口”、“vm001nic2”、“确定”。     片刻之后，附加的 NIC 将添加到 VM。

22. 在“网络”边栏选项卡上选择“vm001nic2”选项卡，然后选择“网络接口: vm001nic2”。   

23. 在“vm001nic 接口”边栏选项卡上选择“IP 设置”，然后在边栏选项卡的中央选择“ipconfig1”。  

24. 在“ipconfig1 设置”边栏选项卡上，为“公共 IP 地址”选择“已启用”，选择“配置所需的设置”、“新建”，输入 vm001nic2pip 作为名称，然后依次选择“静态”、“确定”、“保存”。      

25. 成功保存后，返回 VM001 概述边栏选项卡，然后选择“启动”以启动配置的 Windows Server 2019 VM。 

### <a name="configure-the-windows-server-2019-file-server-storage"></a>配置 Windows Server 2019 文件服务器存储

对于这第一个示例，需要验证以下配置：其中的 Windows Server 2019 文件服务器是 Hyper-V 上运行的虚拟机。 将为此虚拟机配置八个虚拟处理器、一个 VHDX 文件，最重要的是，还要配置两个虚拟网络适配器。 在理想情况下，这两个网络适配器将有不同的可路由子网，但在此测试中，它们位于同一子网上。

![](./media/azure-stack-network-howto-extend-datacenter/image5.png)

文件服务器可以是在 Hyper-V、VMware 或所选替代平台上运行的 Windows Server 2016 或 2019 物理机或虚拟机。 此处的重点是与 Azure Stack 系统建立入站和出站连接，但源与目标之间最好有多个路径，这样可以提供额外的冗余，并可以使用更高级的功能（例如 SMB 多通道）来提升性能。

使用最新的累积更新和修复程序更新文件服务器，重新启动，然后继续配置文件共享。

更新并重新启动后，可将此服务器配置为文件服务器。

1) 在文件服务器计算机上，从 **CMD** 运行 **ipconfig /all**，并记下文件服务器使用的“DNS 服务器”。 

2)  打开“服务器管理器”，依次选择“管理”、“添加角色和功能”。   

3)  依次选择“下一步”、“基于角色或基于功能的安装”，然后继续完成各项选择，直到出现“选择服务器角色”页。   

4)  依次展开“文件和存储服务”、“文件和 iSCSI 服务”，然后选中“文件服务器”框。    完成后，关闭“服务器管理器”。 

5)  重新打开“服务器管理器”，选择“文件和存储服务”。  

6)  选择“共享”。 

7)  在“共享”框中选择“若要创建文件共享，请启动新建共享向导”链接。  

8)  在“新建共享向导”框中，依次选择“SMB 共享 - 快速”、“下一步”，然后继续在向导中操作，并选择“C:\\\\     卷”。

9)  将共享命名为 **TestStorage**，然后选择“下一步”。 

10) 返回“新建共享向导”，依次选择“下一步”、“创建”、“关闭”。    

现已在文件服务器上创建了文件共享。

### <a name="testing-file-storage-performance-and-connectivity"></a>测试文件存储的性能和连接

若要检查通信并运行一些基本测试，请在 **Azure Stack** 系统上重新登录到 Azure Stack 用户门户，然后导航到 **VM001** 的“概述”边栏选项卡。 

1)  选择“连接”以便与 VM001 建立 RDP 连接。 

2)  若要通过“主机名”通信，需要编辑 **Hosts** 文件；但是，在生产环境中，DNS 将以最佳方式配置，名称可自动解析。 也可以使用 IP 地址。 对于本示例，我们将编辑 **Hosts** 文件。

3)  打开“记事本”（请务必使用“以管理员身份运行”选项打开）。  

4)  打开记事本之后，依次选择“文件”、“打开”，导航到 c:\\Windows\System32\Drivers\etc\,，然后选择“打开”对话框右下角的“所有文件”。    选择 **hosts** 文件，然后选择“打开”。 

5)  编辑 hosts 文件，在其中添加文件服务器的 IP 地址和 DNS 名称。

    ![](./media/azure-stack-network-howto-extend-datacenter/image6.png)

6)  保存该文件并关闭记事本。

7)  打开 **CMD**，并 ping 在 hosts 文件中输入的文件服务器名称。 这应该会返回文件服务器上某个网络适配器的 IP 地址，因此，请通过 ping IP 地址来手动测试与其他适配器的通信。

## <a name="connect-to-a-windows-server-iscsi-target-server"></a>连接到 Windows Server iSCSI 目标服务器

在此方案中，我们将在 Azure Stack 上部署并配置 Windows Server 2019 虚拟机，并让其准备好连接到外部 iSCSI 目标服务器（也运行 Windows Server 2019）。

> [!Note]  
> 如果已完成方案 1，请直接跳到自定义计算机的步骤。

### <a name="deploy-the-windows-server-2019-vm-on-azure-stack"></a>在 Azure Stack 上部署 Windows Server 2019 VM

如果尚未在 Azure Stack 上部署 Windows Server 2019 VM，请遵循[在 Azure Stack 上部署 Windows Server 2019 VM](#deploy-the-windows-server-2019-vm-on-azure-stack) 中的步骤。 然后可以配置 Windows Server 2019 iSCSI 目标。

### <a name="configure-the-windows-server-2019-iscsi-target"></a>配置 Windows Server 2019 iSCSI 目标

对于这第二个方案，需验证以下配置：其中的 Windows Server 2019 iSCSI 目标是 Hyper-V 上运行的虚拟机。 将为此虚拟机配置八个虚拟处理器、一个 VHDX 文件，最重要的是，还要配置两个虚拟网络适配器。 在理想情况下，这两个网络适配器将有不同的可路由子网，但在此测试中，它们位于同一子网上。

![](./media/azure-stack-network-howto-extend-datacenter/image5.png)

iSCSI 目标可以是在 Hyper-V、VMware 或所选替代平台上运行的 Windows Server 2016 或 2019 物理机或虚拟机。 此处的重点是与 Azure Stack 系统建立入站与出站连接，但是，最好在源与目标之间使用多个路径，这样可以提供额外的冗余和吞吐量。

使用最新的累积更新和修复程序更新文件服务器，重新启动，然后继续配置 iSCSI 目标服务器。

更新并重新启动后，可将此服务器配置为 iSCSI 目标服务器。

## <a name="next-steps"></a>后续步骤

[Azure Stack 网络的差异和注意事项](azure-stack-network-differences.md)  