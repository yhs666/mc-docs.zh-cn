---
title: 将 Linux VM 部署到 Azure Stack | Microsoft Docs
description: 将应用部署到 Azure Stack。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 4fb6bda666f5990885d123350c89d0aea37c1ef7
ms.sourcegitcommit: 20bff6864fd10596b5fc2ac8e059629999da8ab1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "67135483"
---
# <a name="deploy-a-linux-vm-to-host-a-web-app-in-azure-stack"></a>在 Azure Stack 中部署用于托管 Web 应用的 Linux VM

可以使用市场中的 Ubunutu 映像创建和部署基本的 Linux 虚拟机 (VM)，以托管使用 Web 框架创建的 Web 应用。 此 VM 可以托管使用以下语言编写的 Web 应用：

- **Python**。 常见的 Python Web 框架包括 Flask、Bottle 和 Django。
- **Go**。 常见的 Go 框架包括 Revel、Martini、Gocraft/Web 和 Gorilla。 
- **Ruby**。 可将 Ruby on Rails 设置为交付 Ruby Web 应用的框架。 
- **Java**。 可以使用 Java 来开发要发布到 Apache Tomcat 服务器的 Web 应用。 可将 Tomcat 安装在 Linux 上，然后将 Java WAR 文件直接部署到服务器。 

可根据本文中的说明，通过任何 Web 应用、框架和使用 Linux OS 的后端技术来启动并运行。 然后可以使用 Azure Stack 来管理基础结构，并使用技术内部的管理工具来处理应用的维护任务。

## <a name="deploy-a-linux-vm-for-a-web-app"></a>部署适用于 Web 应用的 Linux VM

创建机密密钥，使用 Linux VM 的基础映像，指定 VM 的特定属性，然后创建 VM。 创建 VM 后，打开使用 VM 所需的端口，并让 VM 托管你的应用。 此外，将会创建 DNS 名称。 最后，连接到 VM 并使用 apt-get 更新计算机。 完成本操作指南中的步骤后，即已在 Azure Stack 中创建了一个可以托管 Web 应用的 VM。

可以直接跳转到说明，否则需要确保一切已准备就绪。

## <a name="prerequisites-and-considerations"></a>先决条件和注意事项

- 需要一个 Azure Stack 订阅。
- 该订阅需有权访问 **Ubuntu Server 16.04 LTS** 映像。 可以使用更高版本的映像，但请注意，本文中的说明是根据 16.04 LTS 编写的。 如果没有此映像，请联系云操作员，以将映像放入 Azure Stack 市场。

<!-- Azure Stack specific considerations

### Authentication

Azure Stack works with two identity management services, Azure Active Directory (Azure AD) and Active Directory Federated Services (AD FS).  This section addresses how this procedure will work with either version.

### Connectivity

Azure Stack can be run in connected to completely disconnected scenarios. This section addresses considerations about the use case in relation to connectivity.

### Azure Stack Development Kit and Integrated Systems

While the two version of the product are the same product both version behave differently. Call out considerations about either version. 

### Azure Stack version

Place any version specific calls outs. The procedure will contain steps for the latest version. This section will contain call outs for previous version that are still supported. -->

## <a name="deploy-vm-using-the-portal"></a>使用门户部署 VM

使用 Putty 之类的应用创建服务器的 SSH 公钥。 访问 Azure Stack 门户并添加 Ubuntu 服务器。 设置网络和 DNS 条目。 连接到服务器以将其更新。

### <a name="create-your-vm"></a>创建 VM

1. 创建服务器的 SSH 公钥。 有关详细信息，请参阅[如何使用 SSH 公钥](azure-stack-dev-start-howto-ssh-public-key.md)。
2. 打开 Azure Stack 门户。
3. 选择“创建资源” > “计算” > “Ubuntu Server 16.04 LTS”。   

    ![将 Web 应用部署到 Azure Stack VM](media/azure-stack-dev-start-howto-deploy-linux/001-portal-compute.png)

4. 在“创建虚拟机”边栏选项卡中，对于“1.   配置基本设置”：
    1. 键入 **VM 的名称**。
    1. 选择 **VM 磁盘类型**：“高级 SSD”或“标准 HDD”。  
    1. 键入你的**用户名**。
    1. 选择“SSH 公钥”作为“身份验证类型”。  
    1. 检索创建的 SSH 公钥。 在文本编辑器中打开此公钥，复制密钥并将其贴到“SSH 公钥”框中。  包含从 `---- BEGIN SSH2 PUBLIC KEY ----` 到 `---- END SSH2 PUBLIC KEY ----` 的文本。 将整个文本块贴到密钥框中：
       ```text  
            ---- BEGIN SSH2 PUBLIC KEY ----
            Comment: "rsa-key-20190207"
            <Your key block>
            ---- END SSH2 PUBLIC KEY ----
       ```
    1. 选择你的 Azure Stack 订阅。
    1. 根据要为应用组织资源的方式，创建新的**资源组**或使用现有的资源组。
    1. 选择你的位置。 ASDK 通常位于**本地**区域。 具体的位置取决于你的 Azure Stack。
1. 对于“2.  大小”，请键入：
    - 针对可在 Azure Stack 中使用的 VM，选择数据大小和 RAM。
    - 可以浏览列表，或根据“计算类型”、“CPU”和存储空间筛选 VM 的大小。  
    - 显示的价格是以当地货币估算的价格，其中仅包括 Azure 基础结构费用以及适用于相应订阅和位置的任何折扣。 这些价格不包括任何适用的软件费用。 建议的大小根据硬件和软件要求，由所选映像的发布者确定。
    - 使用标准磁盘而不是高级磁盘可能会影响操作系统的性能。

1. 在“3.  配置可选功能”中，键入：
    1. 对于“高可用性”，可以选择一个可用性集。  若要为应用程序提供冗余，可将两个或更多个虚拟机组合到一个可用性集中。 这种配置可以确保在发生计划内或计划外维护事件时，至少有一个虚拟机可用，并满足 99.95% 的 Azure SLA 要求。 创建虚拟机后无法更改虚拟机的可用性集。
    1. 对于“存储”，请选择“高级磁盘(SSD)”或“标准磁盘(HDD)”。    高级磁盘 (SSD) 基于固态硬盘，提供一致的低延迟性能。 高级磁盘可在价格与性能之间实现最佳平衡，非常适合用于 I/O 密集型应用程序和生产工作负荷。 标准磁盘 (HDD) 基于磁驱动器，适用于不经常访问数据的应用程序。
    1. 可以选择“使用托管磁盘”。  启用此功能可让 Azure 自动管理磁盘的可用性，以提供数据冗余和容错能力，而无需自行创建和管理存储帐户。 托管磁盘并非在所有区域中均可用。 有关详细信息，请参阅 [Azure 托管磁盘简介](/virtual-machines/windows/managed-disks-overview)。
    1. 选择“虚拟网络”以配置网络。  虚拟网络以逻辑方式在 Azure 中相互隔离。 虚拟网络与数据中心内的传统网络非常类似，可以配置其 IP 地址范围、子网、路由表、网关和安全设置。 默认情况下，同一虚拟网络中的虚拟机可以相互访问。 
    1. 选择“子网”以配置子网。  子网是虚拟网络中的某个 IP 地址范围，可用于相互隔离虚拟机，或者将虚拟机与 Internet 相隔离。 
    1. 选择“公共 IP 地址”，以配置对 VM 或 VM 上运行的服务的访问。  若要与虚拟网络外部的虚拟机通信，请使用公共 IP 地址。 
    1. 选择“基本”或“高级”**网络安全组**。  设置允许或拒绝发往 VM 的网络流量的规则。 
    1. 选择“公共入站端口”，以设置使用常用或自定义协议对 VM 设置的访问。  该服务会指定此规则的目标协议和端口范围。 可以选择预定义的服务（例如 RDP 或 SSH），或提供自定义端口范围。  
        对于 Web 服务器，需要打开 HTTP (80)、HTTPS (443) 和 SSH (22)。 如果你打算使用 RDP 连接来管理计算机，请打开端口 3389。
    1. 若要将扩展添加到 VM，请选择“扩展”。  扩展可将新功能（例如配置管理或防病毒保护）添加到虚拟机。 
    1. 禁用或启用“监视”。  捕获主机上运行的虚拟机的串行控制台输出和屏幕截图有助于诊断启动问题。 
    1. 选择“诊断存储帐户”，以指定用于保存指标的存储帐户。  指标将写入存储帐户，因此你可以使用自己的工具对其进行分析。 上获取。 
    1. 选择“确定”  。
1. 复查“4.  摘要”：
    - 门户将验证你的设置。
    - 若要将设置重复用于 Azure 资源管理器工作流，可以下载适用于你的 VM 的 Azure 资源管理器模板。
    - 通过验证后，按“确定”。  部署 VM 需要几分钟时间。

### <a name="specify-the-open-ports-and-dns-name"></a>指定打开的端口和 DNS 名称

你希望网络中的用户可以通过打开用于连接计算机的端口，并添加可在其 Web 浏览器中使用的 DNS 易记名称（例如 `mywebapp.local.cloudapp.azurestack.external`），来访问你的 Web 应用。

#### <a name="open-inbound-ports"></a>打开入站端口

可以修改预定义服务（例如 RDP 或 SSH）的目标协议和端口范围，或提供自定义端口范围。 例如，你可能想要使用 Web 框架的端口范围。 例如，GO 在端口 3000 上通信。

1. 打开租户的 Azure Stack 门户。
1. 找到你的 VM。 你可能已将 VM 固定到仪表板；或者，可以在“搜索资源”框中搜索该 VM。 
1. 在 VM 边栏选项卡中选择“网络”。 
1. 选择“添加入站端口”规则以打开一个端口。 
1. 对于“源”，请保留默认设置“任何”。 
1. 对于“源端口范围”，请保留通配符 (*)。
1. 对于“目标端口范围”，请添加要打开的端口，例如 `3000`。
1. 对于“协议”，请保留“任何”。  
1. 将“操作”设置为“允许”。  
1. 对于“优先级”，请保留默认设置。 
1. 键入**名称**和**说明**，以帮助记住打开端口的原因。
1. 选择“设置”  （应用程序对象和服务主体对象）。

#### <a name="add-a-dns-name-for-your-server"></a>添加服务器的 DNS 名称

此外，可以创建服务器的 DNS 名称，然后，用户可以使用 URL 连接到你的网站。

1. 打开租户的 Azure Stack 门户。
1. 找到你的 VM。 你可能已将 VM 固定到仪表板；或者，可以在“搜索资源”框中搜索该 VM。 
1. 选择“概述”。 
1. 选择 VM 下的“配置”  。
1. 为“分配”选择“动态”。  
1. 键入 DNS 名称标签（例如 `mywebapp`），使完整的 URL 变成：`mywebapp.local.cloudapp.azurestack.external`（适用于 ASDK 应用）。

### <a name="connect-via-ssh-to-update-your-vm"></a>通过 SSH 进行连接以更新 VM

1. 在 Azure Stack 所在的同一网络中打开 SSH 客户端。 有关详细信息，请参阅[如何使用 SSH 公钥](azure-stack-dev-start-howto-ssh-public-key.md)。
1. 键入：
    ```bash  
        sudo apt-get update
        sudo apt-get -y upgrade
    ```

<!--

## Deploy VM using the PowerShell

Include a sentence or two to explain only what is needed to complete the procedure.

1. Step one of the procedures.

    | Parameter | Example | Description |
    | --- | --- | --- |
    | item      | "dog"   | Describe what it is and where to find the information. |

2. Step two of the procedure

    ```PowerShell  
    verb-command -item "dog"
    ```

3. Step three of the procedures.

    ```PowerShell  
    verb-command -item "dog"
    ```

4. Step four of the procedures.

    ```PowerShell  
    verb-command -item "dog"
    ```

## Deploy VM using the CLI

Include a sentence or two to explain only what is needed to complete the procedure.

1. Step one of the procedures.

    | Parameter | Example | Description |
    | --- | --- | --- |
    | item      | "dog"   | Describe what it is and where to find the information. |

2. Step two of the procedure

    ```CLI  
    verb-command -item "dog"
    ```

3. Step three of the procedures.

    ```CLI  
    verb-command -item "dog"
    ```

4. Step four of the procedures.

    ```CLI  
    verb-command -item "dog"
    ```

-->

## <a name="next-steps"></a>后续步骤

详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)