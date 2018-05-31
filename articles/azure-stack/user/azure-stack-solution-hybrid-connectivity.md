---
title: 使用 Azure 和 Azure Stack 配置混合云连接 | Microsoft Docs
description: 了解如何使用 Azure 和 Azure Stack 配置混合云连接。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
origin.date: 05/07/2018
ms.date: 05/23/2018
ms.author: v-junlch
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 362f3cc5af0352fd1407aaa61b17da74248ba5c6
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475139"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>教程：使用 Azure 和 Azure Stack 配置混合云连接

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以使用混合连接模式在公有云 Azure 和 Azure Stack 中安全地访问资源。 

在本教程中，我们将构建一个示例环境来完成以下任务：

> [!div class="checklist"]
> - 按照隐私或法规要求将数据存储在本地，但同时可以访问公有云 Azure 资源。
> - 在公有云 Azure 中使用云规模的应用部署和资源的同时，保留旧版系统。

## <a name="prerequisites"></a>先决条件

生成混合连接部署所需的几个组件，可能需要一段时间来准备。 

Azure OEM/硬件合作伙伴可以部署生产型 Azure Stack，所有用户都可以部署 ASDK。 此外，Azure Stack 操作员必须部署应用服务、创建计划和产品/服务、创建租户订阅，并添加 Windows Server 2016 映像。 

如果已准备好其中的某些组件，请确保它们符合要求，然后开始操作。

本主题还假设你对 Azure 和 Azure Stack 有一定的了解。 若要在了解详情之后再继续，请务必从以下主题着手：
 - [Azure 简介](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack 的重要概念](/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure
 - 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。
 - 在 Azure 中创建 [Web 应用](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal)。 记下新 Web 应用的 URL，因为稍后需要用到。

### <a name="azure-stack"></a>Azure Stack
 - 使用生产型 Azure Stack，或通过以下链接部署 Azure Stack 开发工具包：
    - https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 
    - 安装过程通常需要几个小时才能完成，因此请相应地做好规划。
 - 将[应用服务](/azure-stack/azure-stack-app-service-deploy) PaaS 服务部署到 Azure Stack。 
 - 在 Azure Stack 环境中[创建计划/产品/服务](/azure-stack/azure-stack-plan-offer-quota-overview)。 
 - 在 Azure Stack 环境中[创建租户订阅](/azure-stack/azure-stack-subscribe-plan-provision-vm)。 


### <a name="before-you-begin"></a>准备阶段

在开始配置之前，请验证是否符合以下条件：

 - 确认 VPN 设备有一个面向外部的公共 IPv4 地址。 此 IP 地址不得位于 NAT 之后。
 - 确保所有资源都部署在同一区域/位置。

#### <a name="example-values"></a>示例值

本文中的示例使用以下值。 可使用这些值创建测试环境，或根据这些值来更好地理解本文中的示例。 有关通用 VPN 网关设置的详细信息，请参阅[关于 VPN 网关设置](/vpn-gateway/vpn-gateway-about-vpn-gateway-settings)。

连接规范：
 - **VPN 类型**：基于路由
 - **连接类型**：站点到站点 (IPsec)
 - **网关类型**：VPN
 - **Azure 连接名称**：Azure-Gateway-AzureStack-S2SGateway（门户会自动填充此值）
 - **Azure Stack 连接名称**：AzureStack-Gateway-Azure-S2SGateway（门户会自动填充此值）
 - **共享密钥**：任何与 VPN 硬件兼容且在连接两端的值匹配的共享密钥
 - **订阅**：任何首选订阅
 - **资源组**：Test-Infra

| Azure/Azure Stack 连接 | Name | 子网 | IP 地址 |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure vNet | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure Stack vNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure 虚拟网关 | Azure-Gateway |  |  |
| Azure Stack 虚拟网关 | AzureStack-Gateway |  |  |
| Azure 公共 IP | Azure-GatewayPublicIP |  | 在创建时确定 |
| Azure Stack 公共 IP | AzureStack-GatewayPublicIP |  | 在创建时确定 |
| Azure 本地网关 | AzureStack-S2SGateway<br>   10.100.100.0/23 |  | Azure Stack 公共 IP 值 |
| Azure Stack 本地网关 | Azure-S2SGateway<br>10.100.102.0/23 |  | Azure 公共 IP 值 |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>在公有云 Azure 和 Azure Stack 中创建虚拟网络

> [!Note]  
> 必须确保在 Azure 或 Azure Stack vNet 地址空间中没有 IP 重叠现象。 

若要使用 Azure 门户在资源管理器部署模型中创建 vNet，请执行以下步骤。 如果是在教程中使用这些步骤，请使用[示例值](/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values)。 如果并非在教程中使用这些步骤，请务必将其中的值替换为自己的值。 

1. 从浏览器导航到 [Azure 门户](http://portal.azure.cn/)并使用 Azure 帐户登录。
2. 单击“创建资源”。 在“搜索 Marketplace”字段中输入 `virtual network`。 从返回的列表中找到“虚拟网络”，然后打开“虚拟网络”页。
3. 从靠近“虚拟网络”页底部的“选择部署模型”列表中，选择“资源管理器”，然后选择“创建”。 这会打开“创建虚拟网络”页。
4. 在“创建虚拟网络”页上，配置 VNet 设置。 填写字段时，如果在字段中输入的字符有效，红色感叹号标记会变成绿色对钩标记。
5. 在 Azure Stack 的“租户门户”中重复这些步骤。

## <a name="add-a-gateway-subnet"></a>添加网关子网

将虚拟网络连接到网关之前，必须先创建要连接的虚拟网络的网关子网。 网关服务使用网关子网中指定的 IP 地址。

在[门户](http://portal.azure.cn/)中，导航到要为其创建虚拟网关的 Resource Manager 虚拟网络。

1. 在 VNet 页的“设置”部分选择“子网”，展开“子网”页。
2. 在“子网”页中选择“+网关子网”，打开“添加子网”页。

    ![](./media/azure-stack-solution-hybrid-connectivity/image4.png)

3. 子网的“名称”自动填充为值“GatewaySubnet”。 Azure 需要此值才能识别作为网关子网的子网。 根据配置要求调整自动填充的“地址范围”值，然后选择页面底部的“确定”以创建该子网。

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>在 Azure 和 Azure Stack 中创建虚拟网关

1. 在门户页左侧选择“+”，然后在搜索框中输入“虚拟网关”。 在“结果”中找到并选择“虚拟网关”。
2. 在“虚拟网关”页底部选择“创建”。 这会打开“创建虚拟网关”页。
3. 在“创建虚拟网关”页中，指定虚拟网关的值，详见示例值以及下面介绍的其他值。
    - **SKU**：基本
    - **虚拟网络**：选择此前创建的虚拟网络。 它会自动选择此前创建的网关子网。 
    - **首个 IP 配置**：这是网关的公共 IP。 
        - 单击“创建网关 IP 配置”，然后就会转到“选择公共 IP 地址”页。
        - 单击“+新建”打开“创建公共 IP 地址”页。
        - 输入公共 IP 地址的“名称”。 保留 SKU 的设置“基本”，然后选择此页底部的“确定”，保存所做的更改。

    > [!Note]  
    > VPN 网关目前仅支持动态公共 IP 地址分配。 但这并不意味着 IP 地址在分配到 VPN 网关后会更改。 公共 IP 地址只在删除或重新创建网关时更改。 该地址不会因为 VPN 网关大小调整、重置或其他内部维护/升级而更改。

4. 验证设置。 
5. 单击“创建”开始创建 VPN 网关。 将会验证这些设置，并会在仪表板上看到“正在部署虚拟网络网关”磁贴。 创建网关最多可能需要 45 分钟。 可能需要刷新门户页才能看到完成状态。

    创建网关后，即可在门户中查看虚拟网络，从而查看分配给网关的 IP 地址。 网关显示为连接的设备。 可以选择连接的设备（虚拟网关），查看详细信息。
6. 在 Azure Stack 部署上重复这些步骤。

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>在 Azure 和 Azure Stack 中创建本地网关

本地网络网关通常是指本地位置。 请为站点提供一个名称供 Azure 或 Azure Stack 引用，然后指定本地 VPN 设备的 IP 地址，以便创建一个连接来连接到该设备。 此外还可指定 IP 地址前缀，以便通过 VPN 网关将其路由到 VPN 设备。 指定的地址前缀是位于本地网络的前缀。 如果之后本地网络发生了更改，或需要更改 VPN 设备的公共 IP 地址，可轻松更新这些值。

1. 在门户中选择“+创建资源”。
2. 在搜索框中输入“本地网关”，然后按 **Enter** 进行搜索。 这会返回一个结果列表。 单击“本地网关”，然后选择“创建”按钮，打开“创建本地网关”页。
3. 在“创建本地网关”页中，指定本地网关的值，详见示例值以及下面介绍的其他值。
    - **IP 地址**：这是需要 Azure 或 Azure Stack 连接到的 VPN 设备的公共 IP 地址。 指定有效的公共 IP 地址。 IP 地址不能位于 NAT 后面，并且必须可让 Azure 访问。 如果目前没有 IP 地址，可以使用示例中显示的值，但是需要返回并将占位符 IP 地址替换为 VPN 设备的公共 IP 地址。 否则，Azure 不能连接。
    -  指的是此本地网络所代表的网络的地址范围。 可以添加多个地址空间范围。 请确保此处所指定的范围没有与要连接到的其他网络的范围相重叠。 Azure 会将指定的地址范围路由到本地 VPN 设备 IP 地址。 如果需要连接到本地站点，请在此处使用自己的值，而不是示例中显示的值。
    - **配置 BGP 设置**：仅在配置 BGP 时使用。 否则，不选择此项。
    - **订阅**：确保显示的是正确订阅。
    - **资源组**：选择要使用的资源组。 可以创建新的资源组或选择已创建的资源组。
    - **位置**：选择将在其中创建此对象的位置。 可选择 VNet 所在的位置，但这不是必须的。
4. 将值指定完以后，选择页面底部的“创建”按钮即可创建本地网关。
5. 在 Azure Stack 部署上重复这些步骤。

## <a name="configure-your-connection"></a>配置连接

通过站点到站点连接连接到本地网络需要 VPN 设备。 在此步骤中，请配置 VPN 设备（称为“连接”）。 配置连接时，需要以下项：
    - 共享密钥。 此共享密钥就是在创建站点到站点 VPN 连接时指定的共享密钥。 在示例中，我们使用基本的共享密钥。 建议生成更复杂的可用密钥。
    - 虚拟网关的“公共 IP 地址”。 可以通过 Azure 门户、PowerShell 或 CLI 查看公共 IP 地址。 若要使用 Azure 门户查找 VPN 网关的公共 IP 地址，请导航到“虚拟网关”，然后选择网关的名称。
在虚拟网关和本地 VPN 设备之间创建站点到站点 VPN 连接。
1. 在门户中选择“+创建资源”。
2. 在搜索框中输入“连接”，然后按 **Enter** 进行搜索。 这会返回一个结果列表。 单击“连接”，然后选择“创建”按钮以打开“创建连接”页面。
3. 在“创建连接”页上，配置连接的值。
     - 基本信息：
        1. **连接类型**：选择“站点到站点(IPSec)”。
        2. **资源组**：（选择测试资源组）
     - 设置：
        1. **虚拟网关**：选择此前创建的虚拟网关。
        2. **本地网关**：选择此前创建的本地网关。 
        3. **连接名称**：此项会自动填充两个网关的值。 
        4. **共享密钥**：此处的值必须与用于本地 VPN 设备的值匹配。 此示例使用“abc123”，但可以（而且应该）使用更复杂的。 重要的是，此处指定的值必须与配置 VPN 设备时指定的值相同。
    - 剩下的“订阅”、“资源组”和“位置”值是固定的。
4. 单击“确定”以创建连接。 屏幕上会闪烁“正在创建连接”。
5. 可在虚拟网关的“连接”页中查看连接。** “状态”会从“未知”转换为“正在连接”，再转换为“成功”。

