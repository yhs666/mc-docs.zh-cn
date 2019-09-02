---
title: 外围网络示例 - 使用由防火墙、UDR 和 NSG 构成的外围网络来保护网络 | Azure
description: 构建包含防火墙、用户定义的路由 (UDR) 和网络安全组 (NSG) 的外围网络（也称为 DMZ）。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 02/01/2016
ms.date: 06/10/2019
ms.author: v-yeche
ms.openlocfilehash: 3f22d28ce3e84886a0909b5027efb01a23364df7
ms.sourcegitcommit: df1b896faaa87af1d7b1f06f1c04d036d5259cc2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66250449"
---
# <a name="example-3-build-a-perimeter-network-to-protect-networks-with-a-firewall-udr-and-nsgs"></a>示例 3：构建外围网络以使用防火墙、UDR 和 NSG 保护网络

<!--Not Available on [Return to the Security Boundary Best Practices Page][HOME]-->

本示例创建一个外围网络（也称为 DMZ、外围安全区域和屏蔽子网）。 本示例将实施防火墙、四个 Windows 服务器、用户定义的路由 (UDR)、IP 转发和网络安全组 (NSG)。 本文将会演练每个相关命令，让你更加深入地了解每个步骤。 “流量方案”部分还深入介绍了流量如何流经外围网络的各个防御层。 最后的“参考”部分包含所有相关代码，并说明如何构建此环境来测试和试验各种方案。

![包含 NVA、NSG 和 UDR 的双向外围网络][1]

## <a name="environment-setup"></a>环境设置

此示例使用包含以下组件的订阅：

* 三个云服务：SecSvc001、FrontEnd001 和 BackEnd001
* 包含以下三个子网的虚拟网络 (CorpNetwork)：SecNet、FrontEnd 和 BackEnd
* 一个网络虚拟设备：与 SecNet 子网连接的防火墙
* 一个代表应用程序 Web 服务器的 Windows 服务器：IIS01
* 两个代表应用程序后端服务器的 Windows 服务器：AppVM01、AppVM02
* 一个代表 DNS 服务器的 Windows 服务器：DNS01

[参考部分](#references)包含可用于构建本文所述的大多数环境的 PowerShell 脚本。 本文不会另外提供有关构建虚拟机 (VM) 和虚拟网络的详细说明。

构建环境的方法如下：

1. 保存[参考部分](#references)中包含的网络配置 XML 文件。 需要使用与给定方案匹配的名称、位置和 IP 地址更新此文件。
1. 更新整个脚本中的用户变量，使之与特定的环境（例如订阅、服务名称等）相匹配。
1. 在 PowerShell 中执行脚本。

> [!NOTE]
> PowerShell 脚本中指定的区域必须与网络配置 XML 文件中指定的区域相匹配。

成功运行该脚本后，执行以下步骤：

1. 设置防火墙规则。 请参阅[防火墙规则](#firewall-rules)部分。
1. （可选）使用“参考”部分中的两个脚本在 Web 服务器和应用服务器上设置一个 Web 应用程序，用于测试此外围网络配置。

## <a name="user-defined-routing"></a>用户定义的路由

默认情况下，以下系统路由定义为：

    Effective routes :
     Address Prefix    Next hop type    Next hop IP address Status   Source
     --------------    -------------    ------------------- ------   ------
     {10.0.0.0/16}     VNETLocal                            Active   Default
     {0.0.0.0/0}       internet                             Active   Default
     {10.0.0.0/8}      Null                                 Active   Default
     {100.64.0.0/10}   Null                                 Active   Default
     {172.16.0.0/12}   Null                                 Active   Default
     {192.168.0.0/16}  Null                                 Active   Default

VNETLocal 始终是该特定虚拟网络的已定义地址前缀。 例如，根据每个特定虚拟网络的定义方式，不同的虚拟网络有不同前缀。 其余系统路由是静态的，其默认值如上所示。

在优先级方面，将通过最长前缀匹配 (LPM) 方法处理路由。 因此，表中最具体的路由将应用到给定的目标地址。

因此，发往本地网络 (10.0.0.0/16) 上的 DNS01 (10.0.2.4) 的流量将由于 10.0.0.0/16 路由而通过虚拟网络路由。  对于 10.0.2.4，10.0.0.0/16 路由是最具体的路由。 即使 10.0.0.0/8 和 0.0.0.0/0 可能适用，也仍会应用此规则。 但是，这些路由较不具体，因此不影响此流量。 发往 10.0.2.4 的流量使用本地虚拟网络作为下一跃点，因而会路由到目标。

例如，发往 10.1.1.1 的流量不会应用 10.0.0.0/16 路由。 10.0.0.0/8 系统是最具体的路由，因此会丢弃流量或将其“黑洞化”，因为下一跃点为 Null。

如果目标不适用于任何 Null 前缀或 VNETLocal 前缀，则流量将遵循最不具体的路由 (0.0.0.0/0)。 它将路由到用作下一跃点的 Internet，并路由出 Azure 的 Internet 边缘。

如果路由表中有两个相同的前缀，则优先顺序基于路由的 source 属性：

1. VirtualAppliance：手动添加到路由表中的用户定义的路由。
1. VPNGateway：由动态网络协议添加的动态路由（与混合网络配合使用时为 BGP）。 这些路由可不断变化，因为动态协议会自动反映对等网络中的更改。
1. 默认值：上述路由表中所示的系统路由、本地虚拟网络和静态条目。

> [!NOTE]
> 现在可以将用户定义的路由 (UDR) 与 ExpressRoute 网关和 VPN 网关配合使用，强制将出站和入站跨界流量路由到网络虚拟设备 (NVA)。

### <a name="create-local-routes"></a>创建本地路由

本示例使用分别用于前端和后端子网的两个路由表。 已在每个表中加载了适用于给定子网的静态路由。 对于本示例而言，每个数据表包含三个路由：

1. 未定义下一跃点的本地子网流量。 此路由允许本地子网流量绕过防火墙。
2. 下一跃点定义为防火墙的虚拟网络流量。 此路由替代允许直接路由本地虚拟网络流量的默认规则。
3. 将下一跃点定义为防火墙的所有其余流量 (0/0)。

路由表在创建后会绑定到其子网。 前端子网路由表应如下所示：

    Effective routes :
     Address Prefix    Next hop type       Next hop IP address  Status   Source
     --------------    ------------------  -------------------  ------   ------
     {10.0.1.0/24}     VNETLocal                                Active
     {10.0.0.0/16}     VirtualAppliance    10.0.0.4             Active
     {0.0.0.0/0}       VirtualAppliance    10.0.0.4             Active

本示例使用以下命令来构建路由表、添加用户定义的路由，然后将路由表绑定到子网。 以 `$` 开头的项（例如 `$BESubnet`）是本文“参考”部分提供的脚本中的用户定义的变量。

1. 首先创建基础路由表。 以下代码片段为后端子网创建表。 完整的脚本还会为前端子网创建对应的表。

    ```powershell
    New-AzureRouteTable -Name $BERouteTableName `
       -Location $DeploymentLocation `
       -Label "Route table for $BESubnet subnet"
    ```

1. 创建路由表后，可以添加特定的用户定义的路由。 以下代码片段指定通过虚拟设备路由所有流量 (0.0.0.0/0)。 前面在脚本中创建虚拟设备时，已使用变量 `$VMIP[0]` 传入了分配的 IP 地址。 完整的脚本还会在前端表中创建对应的规则。

    ```powershell
    Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
    ```

1. 上述路由条目会替代默认的“0.0.0.0/0”路由，但默认的 10.0.0.0/16 规则仍允许虚拟网络中的流量直接路由到目标，而不是路由到网络虚拟设备。 若要纠正此行为，需要添加以下规则：

    ```powershell
    Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
    ```

1. 此时必须做出选择。 上述两个路由将所有流量路由到防火墙进行评估，包括单个子网中的流量。 这可能是所需的行为。 否则，可以允许子网中的流量直接在本地路由而不需要防火墙的介入。 添加第三个特定的规则，用于直接路由指向本地子网的任何地址 (NextHopType = VNETLocal)。

    ```powershell
    Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
           -NextHopType VNETLocal
    ```

1. 最后，在创建路由表并填充用户定义的路由后，需要将该表绑定到子网。 以下代码片段绑定后端子网的表。 完整的脚本还会将前端路由表绑定到前端子网。

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
       -SubnetName $BESubnet `
       -RouteTableName $BERouteTableName
    ```

## <a name="ip-forwarding"></a>IP 转发

IP 转发是 UDR 的随附功能。 这是虚拟设备上的一项设置，使虚拟设备能够接收不是要传送到该设备的流量，再将流量转发到其最终目标。

例如，如果来自 AppVM01 的流量对 DNS01 服务器发出请求，UDR 会将流量路由到防火墙。 在启用 IP 转发后，目标为 DNS01 (10.0.2.4) 的流量被防火墙设备 (10.0.0.4) 所接受，然后转发到其最终目标 (10.0.2.4)。 如果防火墙上未启用 IP 转发，则即使路由表将防火墙用作下一跃点，流量也不会被设备所接受。

> [!IMPORTANT]
> 请记得同时启用 IP 转发和用户定义的路由。

可以在创建 VM 时使用一条命令启用 IP 转发。 调用代表防火墙虚拟设备的 VM 实例，并启用 IP 转发。 请注意，以 `$` 开头的任何红色项（例如 `$VMName[0]`）是本文档的“参考”部分提供的脚本中的用户定义的变量。 方括号中的零 `[0]` 代表 VM 阵列中的第一个 VM。 为使示例脚本无须修改即可运行，第一个 VM (VM 0) 必须是防火墙。 在完整的脚本中，相关的代码片段与末尾处的 UDR 命令分组在一起。

```powershell
Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
    Set-AzureIPForwarding -Enable
```

## <a name="network-security-groups"></a>网络安全组

本示例将构建一个网络安全组 (NSG)，并在其中加载单个规则。 然后，本示例仅将此 NSG 绑定到前端和后端子网（而不绑定到 SecNet）。 加载到 NSG 的规则如下所示：

* 拒绝从 Internet 到整个虚拟网络（所有子网）的任何流量（所有端口）。

尽管本示例使用了 NSG，但它的主要用途是作为防止人为配置错误的第二道防线。 需要阻止从 Internet 传送到前端或后端子网的所有入站流量。 流量只应流经 SecNet 子网前往防火墙，然后，应该只有适当的流量才会路由到前端或后端子网。 此外，UDR 规则会将抵达前端或后端子网的流量路由到防火墙。 防火墙将此流量视为非对称流，并丢弃出站流量。

有三个安全层会保护前端和后端子网：

1. FrontEnd001 和 BackEnd001 云服务上没有开放的终结点
1. NSG 拒绝来自 Internet 的流量
1. 防火墙丢弃非对称流量

对于本示例中的 NSG，值得注意的是它只包含一个规则，如下所示。 此规则会拒绝发往整个虚拟网络（包括安全子网）的 Internet 流量。

```powershell
Get-AzureNetworkSecurityGroup -Name $NSGName | `
    Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
    from the internet" `
    -Type Inbound -Priority 100 -Action Deny `
    -SourceAddressPrefix INTERNET -SourcePortRange '*' `
    -DestinationAddressPrefix VIRTUAL_NETWORK `
    -DestinationPortRange '*' `
    -Protocol *
```

由于 NSG 只绑定到前端和后端子网，因此不会对安全子网的入站流量应用此规则。 因此，流量会流向安全子网。

```powershell
Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $FESubnet -VirtualNetworkName $VNetName

Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $BESubnet -VirtualNetworkName $VNetName
```

## <a name="firewall-rules"></a>防火墙规则

需要在防火墙上创建转发规则。 由于防火墙会阻止或转发所有入站、出站和 虚拟网络 内部流量，因此需要多个防火墙规则。 此外，防火墙必须处理发往安全服务公共 IP 地址的所有入站流量（在不同的端口上）。 为了避免今后出现重复性工作，请遵循最佳做法：先绘制逻辑流程图，然后设置子网和防火墙规则。 下图是此示例中防火墙规则的逻辑视图：

![防火墙规则的逻辑视图][2]

> [!NOTE]
> 管理端口根据网络虚拟设备的不同而异。 本示例演示了使用端口 22、801 和 807 的 Barracuda NextGen 防火墙。 请查阅设备供应商的文档，找到用于管理设备的确切端口。

### <a name="logical-rule-description"></a>逻辑规则描述

以上逻辑图未显示安全子网，因为防火墙是该子网上的唯一资源。 此图显示的是防火墙规则以及这些规则在逻辑上是如何允许或拒绝流量的流动，而不是实际的路由路径。 此外，针对远程桌面协议 (RDP) 流量选择的外部端口是较高范围的端口 (8014 - 8026)，并且选择时遵循了本地 IP 地址的最后两个八位字节以方便阅读。 例如，本地服务器地址 10.0.1.4 与外部端口 8014 相关联。 不过可以使用任何更高范围的且不冲突的端口。

本示例需要以下类型的规则：

* 针对入站流量的外部规则：
    1. 防火墙管理规则：允许流量传递到网络虚拟设备的管理端口。
    2. 针对每个 Windows 服务器的 RDP 规则：允许通过 RDP 管理单个服务器。  根据网络虚拟设备的功能，可将这些规则捆绑为一个规则。
    3. 应用程序流量规则：第一个规则对应于前端 Web 流量，第二个规则对应于后端流量（例如从 Web 服务器流向数据层）。 这些规则的配置取决于网络体系结构和流量流。

        * 第一个规则允许实际的应用程序流量抵达应用程序服务器。 与安全性、管理等相关的规则不同，应用程序规则允许外部用户或服务访问应用程序。 本示例在端口 80 上有单个 Web 服务器，允许单个防火墙应用程序规则将发往外部 IP 地址的流量路由到 Web 服务器内部 IP 地址。 重定向的流量会话由 NAT 重新映射到内部服务器。
        * 第二个应用程序流量规则是后端规则，允许 Web 服务器使用任何端口将流量路由到 AppVM01 服务器而不是 AppVM02 服务器。

* 针对虚拟网络内部流量的内部规则：
    1. 出站到 Internet 规则：允许来自任何网络的流量传递到选定的网络。 此规则通常是防火墙上的默认规则，但处于禁用状态。 对于本示例，需启用此规则。
    2. DNS 规则：仅允许 DNS（端口 53）流量传递到 DNS 服务器。 在此环境中，阻止了大部分由前端流往后端的流量。 此规则专门允许来自任何本地子网的 DNS。
    3. 子网到子网规则：允许后端子网上的任何服务器连接到前端子网上的任何服务器，但不允许反向连接。

* 针对不符合上述任一条件的流量的防故障规则：
    1. 拒绝所有流量规则：始终为最终规则（就优先级而言）。 如果流量流不符合上述任何规则，此规则会阻止它。 它是默认的规则。 它通常已激活，因此不需要修改。

> [!TIP]
> 为了简化本示例，允许保留第二个应用程序流量规则中的任何端口。 在真实情况下，应使用特定的端口和地址范围来减小此规则的受攻击面。

> [!IMPORTANT]
> 创建规则后，请务必检查每个规则的优先级，以确保根据需要允许或拒绝流量。 对于本示例，规则按优先顺序排列。 如果规则顺序错误，则很容易被锁定在防火墙外面。 请务必将防火墙管理规则设置为绝对最高优先级。

### <a name="rule-prerequisites"></a>规则先决条件

运行防火墙的虚拟机需要公共终结点。 为使防火墙能够处理流量，必须打开这些公共终结点。 本示例包含三种类型的流量：

1. 用于控制防火墙和防火墙规则的管理流量
1. 用于控制 Windows 服务器的 RDP 流量
1. 应用程序流量

流量类型显示在以上防火墙规则逻辑图的上半部分。

> [!IMPORTANT]
> 请记住，所有流量都会通过防火墙传送。  若要通过远程桌面访问 IIS01 服务器，需要在端口 8014 上连接到防火墙，并允许防火墙在内部将 RDP 请求路由到 IIS01 RDP 端口。 由于（在门户中）没有直接通往 IIS01 的 RDP 路径，因此 Azure 门户的“连接”按钮不起作用。  所有来自 Internet 的连接将连往安全服务和端口（例如 secscv001.chinacloudapp.cn:xxxx）。 请参考上图中外部端口与内部 IP 和端口的映射。

可以在创建 VM 时或构建后打开终结点。 示例脚本和以下代码片段在创建 VM 后打开终结点。

请注意，以 `$` 开头的项（例如 `$VMName[$i]`）是本文“参考”部分提供的脚本中的用户定义的变量。 `[$i]` 代表 VM 阵列中特定 VM 的阵列编号。

```powershell
Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
    -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
    Update-AzureVM
```

尽管此处因为使用了变量而未明确显示，但你应该只打开安全云服务上的终结点。 此预防措施有助于确保防火墙处理所有入站流量，不管这些流量是路由的、通过 NAT 重新映射的还是丢弃的。

在电脑上安装管理客户端用于管理防火墙并创建所需的配置。 有关如何管理防火墙或其他 NVA 的详细信息，请参阅供应商的文档。 本部分的余下内容以及“创建防火墙规则”部分将介绍如何配置防火墙。  请使用供应商的管理客户端，而不要使用 Azure 门户或 PowerShell。

转到 [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin) 来下载管理客户端并了解如何连接到 Barracuda 防火墙。

在登录到防火墙之后、创建防火墙规则之前定义网络对象和服务对象。 这两个必备对象类可方便你创建规则。

对于本示例，请为以下每一项各定义一个命名网络对象（共三个）：

* 前端子网
* 后端子网
* DNS 服务器的 IP 地址

若要从 Barracuda NG Admin 客户端仪表板创建命名网络：

1. 转到**配置选项卡**。
1. 在“操作配置”部分选择“规则集”。  
1. 在“防火墙对象”菜单中选择“网络”。  
1. 在“编辑网络”菜单中选择“新建”。  
1. 通过添加名称和前缀来创建网络对象：

    ![创建前端网络对象][3]

上述步骤将为前端子网创建一个命名网络。 它还为后端子网创建类似的对象。 现在可以更轻松地在防火墙规则中按名称引用子网。

对于 DNS 服务器对象：

![创建 DNS 服务器对象][4]

本文稍后会在 DNS 规则中使用这一个 IP 地址引用。

另一个必备对象包括服务对象，这些对象代表每台服务器的 RDP 连接端口。 现有的 RDP 服务对象已绑定到默认 RDP 端口 3389。 因此，可以创建新的服务以允许来自外部端口 (8014-8026) 的流量。 还可将新端口添加到现有的 RDP 服务。 但为了便于演示，可为每台服务器创建一个规则。 若要从 Barracuda NG Admin 客户端仪表板为服务器创建新的 RDP 规则：

1. 转到**配置选项卡**。
1. 在“操作配置”部分选择“规则集”。  
1. 在“防火墙对象”菜单中选择“服务”。  
1. 向下滚动服务列表，然后选择“RDP”。 
1. 右键单击并选择“复制”，然后右键单击并选择“粘贴”。
1. 现已创建一个可编辑的 RDP-Copy1 服务对象。 右键单击“RDP-Copy1”并选择“编辑”。  
1. 此时会弹出如下所示的“编辑服务对象”窗口： 

    ![默认 RDP 规则的副本][5]

1. 编辑代表特定服务器 RDP 服务的值。 对于 AppVM01，应该修改默认的 RDP 规则以反映图 8 中所用的新服务**名称**、**说明**和外部 RDP 端口。 请注意，端口已从 RDP 默认值 3389 更改为要用于此特定服务器的外部端口。 例如，AppVM01 的外部端口为 8025。 修改后的服务规则如下所示：

    ![AppVM01 规则][6]

重复此过程以创建剩余服务器的 RDP 服务：AppVM02、DNS01 和 IIS01。 这些服务可让下一部分中的规则创建操作变得更简单明了。

> [!NOTE]
> 防火墙不需要 RDP 服务，因为防火墙 VM 是基于 Linux 的映像，因此将在端口 22 上使用 SSH 来管理 VM，而不是使用 RDP。 此外，允许使用端口 22 和另外两个端口进行管理连接。 请参阅下一部分中的“防火墙管理规则”。 

### <a name="firewall-rules-creation"></a>创建防火墙规则

本示例使用三种类型的防火墙规则，它们各有不同的图标：

应用程序重定向规则：![“应用程序重定向”图标][7]

目标 NAT 规则：![“目标 NAT”图标][8]

传递规则：![“传递”图标][9]

可以在 Barracuda 网站中找到有关这些规则的详细信息。

若要创建以下规则或验证现有的默认规则：

1. 在 Barracuda NG Admin 客户端仪表板中，转到“配置”选项卡。 
1. 在“操作配置”部分选择“规则集”。  
1. “主要规则”网格会显示此防火墙中的现有活动规则和已停用的规则。  选择右上角的绿色 **+** 即可创建新规则。 如果防火墙“禁止”更改，则你会看到标有“锁定”的按钮，且无法创建或编辑规则。  选择“锁定”按钮可解除锁定规则集并可进行编辑。  右键单击要编辑的规则，然后选择“编辑规则”。 

创建或修改任何规则后，请将其推送到防火墙并激活。 否则，规则更改不会生效。 [规则激活](#rule-activation)中描述了推送和激活过程。

完成本示例所需的每个规则的具体说明如下：

* **防火墙管理规则**：此应用重定向规则允许流量传递到网络虚拟设备的管理端口，在本示例中为 Barracuda NextGen 防火墙。 管理端口是 801 和 807，以及 22（可选）。 外部和内部端口相同（无端口转换）。 此规则名为 SETUP-MGMT-ACCESS。 它是默认的规则，默认已在 Barracuda NextGen 防火墙版本 6.1 中启用。

    ![防火墙管理规则][10]

    > [!TIP]
    > 此规则中的源地址空间是“任意”。  如果管理 IP 地址范围已知，则减小此范围也会减小管理端口的受攻击面。

* **RDP 规则**：这些目标 NAT 规则允许通过 RDP 管理单个服务器。 这些规则的重要字段包括：
    * 源。 若要允许来自任意位置的 RDP，请在“源”字段中使用“任意”引用值。 
    * Service。 使用前面创建的 RDP 服务对象：**AppVM01 RDP**。 外部端口将重定向到服务器的本地 IP 地址和默认的 RDP 端口 3386。 此特定规则用于对 AppVM01 进行 RDP 访问。
    * 目标。 使用防火墙上的本地端口：“DCHP 1 本地 IP”，如果使用静态 IP，则为“eth0”。   如果网络设备有多个本地接口，序号（eth0、eth1 等）可能不同。 防火墙使用此端口发送数据，它可能与接收端口相同。 实际的路由目标在“目标列表”字段中列出。 
    * 重定向。 告知虚拟设备最终要将此流量重定向到何处。 最简单的重定向是在“目标列表”字段中放置 IP。 还可以指定端口，NAT 会重新路由端口和 IP 地址。 如果不指定端口，则虚拟设备会使用入站请求中的目标端口。

        ![防火墙 RDP 规则][11]

        创建四个 RDP 规则：

        | 规则名称 | 服务器 | 服务 | 目标列表 |
        | --- | --- | --- | --- |
        | RDP-to-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
        | RDP-to-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
        | RDP-to-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
        | RDP-to-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

    > [!TIP]
    > 缩小“源”和“服务”字段的范围可减小受攻击面。 使用可让功能正常运行的最受限范围。

* **应用程序流量规则**：有两个应用程序流量规则。 一个规则对应于前端 Web 流量。 另一个规则对应于后端流量（例如从 Web 服务器流向数据层）。 这些规则取决于网络体系结构和流量流。

    * 针对 Web 流量的前端规则：

        ![防火墙 Web 规则][12]

        此目标 NAT 规则允许实际的应用程序流量抵达应用程序服务器。 与安全性、管理等相关的规则不同，应用程序规则允许外部用户或服务访问应用程序。 本示例在端口 80 上有单个 Web 服务器，允许单个防火墙应用程序规则将发往外部 IP 地址的流量路由到 Web 服务器内部 IP 地址。 重定向的流量会话由 NAT 重新映射到内部服务器。

        > [!NOTE]
        > “目标列表”字段中未分配端口。  因此入站端口 80（对于所选服务为 443）将用于 Web 服务器的重定向。 如果 Web 服务器正在侦听不同的端口（例如端口 8080），则可将“目标列表”字段更新为 10.0.1.4:8080 以同时允许端口重定向。

    * 后端规则允许 Web 服务器通过“任意”服务来与 AppVM01 服务器（而不是 AppVM02）通信： 

        ![防火墙 AppVM01 规则][13]

        此传递规则允许前端子网上的任何 IIS 服务器在任何端口上使用任何协议连接到 AppVM01 (10.0.2.5)，使 Web 应用程序能够访问数据。

        在此屏幕截图中，“目标”字段使用 `<explicit-dest>` 来表示目标为 10.0.2.5。  可按屏幕截图中所示显式指定 IP 地址。 还可以按 DNS 服务器的先决条件中所示，使用命名的网络对象。 防火墙管理员可以选择要使用的方法。 若要将 10.0.2.5 添加为显式目标，请双击 `<explicit-dest>` 下的第一个空白行，并在打开的对话框中输入地址。

        使用此传递规则时不需要 NAT，因为它会处理内部流量。 可将“连接方法”设置为 `No SNAT`。 

        > [!NOTE]
        > 此规则中的“源”网络是前端子网上的任何资源（如果只有一个）。 如果体系结构指定了已知特定数目的 Web 服务器，则你可以将“网络对象”资源创建为更特定于这些确切的 IP 地址，而不是整个前端子网。

        > [!TIP]
        > 此规则使用“任意”服务让你更轻松地设置和使用示例应用程序。  还允许在单个规则中使用 ICMPv4 (ping)。 但是，为了减小跨越此边界的受攻击面，我们建议将端口和协议服务限制为尽可能小的范围，只能应用程序能够正常运行即可。

        > [!TIP]
        > 尽管此示例规则使用 `<explicit-dest>` 引用，但在整个防火墙配置中应使用一致的方法。 我们建议使用命名的网络对象，以方便阅读和支持。 此处显示 `<explicit-dest>` 只是为了显示替代的引用方法。 我们一般并不建议这样做，尤其是对于复杂的配置。

* **出站到 Internet 规则**：此传递规则允许来自任何“源”网络的流量传递到选定的“目标”网络。 通常，Barracuda NextGen 防火墙默认会“打开”此规则，但它处于禁用状态。 右键单击此规则可以访问“激活规则”命令。  按屏幕截图中所示修改此规则，以将后端和前端子网的网络对象添加到此规则的“源”属性。 在本文的先决条件部分，你已创建这些网络对象。

    ![防火墙出站规则][14]

* **DNS 规则**：此传递规则仅允许 DNS（端口 53）流量传递到 DNS 服务器。 在此环境中，阻止了大部分由前端流往后端的流量，而此规则专门允许 DNS 流量。

    ![防火墙 DNS 规则][15]

    > [!NOTE]
    > “连接方法”已设置为 `No SNAT`，因为此规则用于内部 IP 地址之间的流量，不需要通过 NAT 重新路由。 

* **子网到子网规则**：此默认传递规则已激活并经过修改，允许后端子网上的任何服务器连接到前端子网上的任何服务器。 此规则仅涵盖内部流量，因此可将“连接方法”设置为 `No SNAT`。 

    ![防火墙 VNet 内部规则][16]

    > [!NOTE]
    > 此处未选中“双向”复选框，因此只会单向应用此规则。  可以发起从后端子网到前端网络的连接，但是反之不然。 如果选中该复选框，则此规则会启用双向流量，而我们在逻辑图中指定不需要这样做。

* **拒绝所有流量规则**：就优先级而言，此规则始终应为最终规则。 如果流量流与上述任何规则都不匹配，则此规则会导致丢弃这些流量。 通常，此规则默认已激活，因此无需修改。

    ![防火墙拒绝规则][17]

> [!IMPORTANT]
> 创建上述所有规则后，请检查每个规则的优先级，以确保可根据需要允许或拒绝流量。 对于本示例，规则的列出顺序是它们在 Barracuda 管理客户端的转发规则主网格中显示的顺序。

## <a name="rule-activation"></a>规则激活

根据逻辑图的规范修改规则集后，需要将规则集上传到防火墙并激活。

![防火墙规则激活][18]

在管理客户端窗口的右上角，选择“发送更改”将修改后的规则发送到防火墙。  选择“激活”。 

激活防火墙规则集后，此示例环境即已构建完成。

## <a name="traffic-scenarios"></a>流量方案

> [!IMPORTANT]
> 请记住，所有流量都会通过防火墙传送。  若要通过远程桌面访问 IIS01 服务器，需要在端口 8014 上连接到防火墙，并允许防火墙在内部将 RDP 请求路由到 IIS01 RDP 端口。 由于（在门户中）没有直接通往 IIS01 的 RDP 路径，因此 Azure 门户的“连接”按钮不起作用。  所有来自 Internet 的连接将连往安全服务和端口（例如 secscv001.chinacloudapp.cn:xxxx）。 请参考上图中外部端口与内部 IP 和端口的映射。

对于这些方案，应准备好以下防火墙规则：

1. 防火墙管理 (FW Mgmt)
2. 通过 RDP 访问 IIS01
3. 通过 RDP 访问 DNS01
4. 通过 RDP 访问 AppVM01
5. 通过 RDP 访问 AppVM02
6. 到 Web 的应用流量
7. 到 AppVM01 的应用流量
8. 出站到 Internet
9. 前端到 DNS01
10. 子网内部流量（仅限后端到前端）
11. 全部拒绝

实际的防火墙规则集很可能还包含其他规则，而不仅仅是本示例所述的规则。 而其他这些规则的优先级编号也可能不同。 应参考此列表和关联的编号来了解这些规则彼此之间的相对优先级。 例如，在实际的防火墙上，“通过 RDP 访问 IIS01”规则编号可能是 5，但只要它低于“防火墙管理”规则并高于“通过 RDP 访问 DNS01”规则，就符合此列表的目的。 此列表还有助于简化以下方案的说明。 例如“防火墙规则 9 (DNS)”。 请注意，当流量方案与 RDP 无关时，四个 RDP 规则统称为“RDP 规则”。

另请记住，已创建了网络安全组 (NSG) 用于前端和后端子网上的入站 Internet 流量。

### <a name="allowed-internet-to-web-server"></a>（允许）从 Internet 访问 Web 服务器

1. Internet 用户从 SecSvc001.CloudApp.Net（面向 Internet 的云服务）请求 HTTP 页面。
1. 云服务通过端口 80 上的开放终结点将流量传递到 10.0.0.4:80 上的防火墙接口。
1. 未对安全子网分配 NSG，因此系统 NSG 规则允许流量发往防火墙。
1. 流量抵达防火墙的内部 IP 地址 (10.0.1.4)。
1. 防火墙执行规则处理：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 - 5（RDP 规则）不适用。 转到下一规则。
    1. 防火墙规则 6（应用：Web）适用。 允许流量。 防火墙通过 NAT 将流量重新路由到 10.0.1.4 (IIS01)。
1. 前端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 防火墙通过 NAT 重新路由此流量，因此源地址现在是防火墙。 由于防火墙位于安全子网中，并对前端子网 NSG 显示为本地流量，因此允许此流量。 转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量，因此允许此流量。 停止 NSG 规则处理。
1. IIS01 正在侦听 Web 流量。 它会接收此请求并开始处理请求。
1. IIS01 尝试与后端子网中的 AppVM01 发起 FTP 会话。
1. 前端子网上的 UDR 路由使防火墙成为下一跃点。
1. 前端子网上没有出站规则，因此允许流量。
1. 防火墙开始处理规则：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 - 5（RDP 规则）不适用。 转到下一规则。
    1. 防火墙规则 6（应用：Web）不适用。 转到下一规则。
    1. 防火墙规则 7（应用：后端）适用。 允许流量。 防火墙将流量转发到 10.0.2.5 (AppVM01)。
1. 后端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量。 允许流量。 停止 NSG 规则处理。
1. AppVM01 接收请求，发起会话并做出响应。
1. 后端子网上的 UDR 路由使防火墙成为下一跃点。
1. 后端子网上没有出站 NSG 规则，因此允许响应。
1. 由于这会返回已建立的会话上的流量，防火墙将响应传回给 Web 服务器 (IIS01)。
1. 前端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量，因此允许此流量。 停止 NSG 规则处理。
1. IIS 服务器接收响应，并完成 AppVM01 的事务。 然后，服务器完成构建 HTTP 响应，并将其发送到请求方。
1. 前端子网上没有出站 NSG 规则，因此允许响应。
1. HTTP 响应抵达防火墙。 由于这是对已建立的 NAT 会话的响应，因此防火墙会接受它。
1. 防火墙将响应重定向回到 Internet 用户。
1. 前端子网上没有出站 NSG 规则或 UDR 跃点，因此允许响应。 Internet 用户接收请求的网页。

### <a name="allowed-internet-rdp-to-back-end"></a>（允许）通过 RDP 从 Internet 访问后端

1. Internet 上的服务器管理员请求通过 SecSvc001.CloudApp.Net:8025 对 AppVM01 发起 RDP 会话。 8025 是用户针对防火墙规则 4 (RDP AppVM01) 分配的端口号。
1. 云服务通过端口 8025 上的开放终结点将流量传递到 10.0.0.4:8025 上的防火墙接口。
1. 未对安全子网分配 NSG，因此系统 NSG 规则允许流量发往防火墙。
1. 防火墙执行规则处理：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 (RDP IIS) 不适用。 转到下一规则。
    1. 防火墙规则 3 (RDP DNS01) 不适用。 转到下一规则。
    1. 防火墙规则 4 (RDP AppVM01) 适用，因此允许流量。 防火墙通过 NAT 将流量重新路由到 10.0.2.5:3386（AppVM01 上的 RDP 端口）。
1. 后端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 防火墙通过 NAT 重新路由此流量，因此源地址现在是位于安全子网上的防火墙。 它被后端子网 NSG 视为本地流量，因此受到允许。 转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量，因此允许此流量。 停止 NSG 规则处理。
1. AppVM01 侦听 RDP 流量并做出响应。
1. 没有出站 NSG 规则，因此会应用默认规则。 允许返回流量。
1. UDR 将出站流量路由到用作下一跃点的防火墙。
1. 由于这会返回已建立的会话上的流量，防火墙将响应传回给 Internet 用户。
1. 已启用 RDP 会话。
1. AppVM01 提示输入用户名和密码。

### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>（允许）在 DNS 服务器上执行 Web 服务器 DNS 查找

1. Web 服务器 IIS01 请求 http\:\/\/www.data.gov 上的数据源，但需要解析地址。
1. 虚拟网络的网络配置将 DNS01（后端子网上的 10.0.2.4）列为主要 DNS 服务器。 IIS01 将 DNS 请求发送到 DNS01。
1. UDR 将出站流量路由到用作下一跃点的防火墙。
1. 没有出站 NSG 规则绑定到前端子网。 允许流量。
1. 防火墙执行规则处理：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 - 5（RDP 规则）不适用。 转到下一规则。
    1. 防火墙规则 6 和 7（应用规则）不适用。 转到下一规则。
    1. 防火墙规则 8（到 Internet）不适用。 转到下一规则。
    1. 防火墙规则 9 (DNS) 适用。 允许流量。 防火墙将流量转发到 10.0.2.4 (DNS01)。
1. 后端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量。 允许流量。 停止 NSG 规则处理。
1. DNS 服务器接收请求。
1. DNS 服务器没有缓存的地址，请求 Internet 上的根 DNS 服务器。
1. UDR 将出站流量路由到用作下一跃点的防火墙。
1. 后端子网上没有出站 NSG 规则，因此允许流量。
1. 防火墙执行规则处理：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 - 5（RDP 规则）不适用。 转到下一规则。
    1. 防火墙规则 6 和 7（应用规则）不适用。 转到下一规则。
    1. 防火墙规则 8（到Internet）适用。 允许流量。 通过 SNAT 将会话重新路由到 Internet 上的根 DNS 服务器。
1. Internet DNS 服务器做出响应。 此会话是从防火墙发起的，因此响应被防火墙接受。
1. 此会话已建立，因此防火墙会将响应转发到发起端服务器 DNS01。
1. 后端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量，因此允许此流量。 停止 NSG 规则处理。
1. DNS 服务器接收并缓存响应，并将初始请求响应给 IIS01。
1. 后端子网上的 UDR 路由使防火墙成为下一跃点。
1. 后端子网上没有出站 NSG 规则，因此允许流量。
1. 此会话已在防火墙上建立，因此防火墙会将响应重新路由回到 IIS 服务器。
1. 前端子网执行入站规则处理：
    1. 后端子网到前端子网的入站流量没有 NSG 规则，因此不会应用任何 NSG 规则。
    1. 默认的系统规则允许子网间的流量。 允许流量。
1. IIS01 从 DNS01 接收响应。

### <a name="allowed-back-end-server-to-front-end-server"></a>（允许）后端服务器到前端服务器

1. 通过 RDP 登录 AppVM02 的管理员直接通过 Windows 文件资源管理器从 IIS01 服务器请求文件。
1. 后端子网上的 UDR 路由使防火墙成为下一跃点。
1. 后端子网上没有出站 NSG 规则，因此允许响应。
1. 防火墙执行规则处理：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 - 5（RDP 规则）不适用。 转到下一规则。
    1. 防火墙规则 6 和 7（应用规则）不适用。 转到下一规则。
    1. 防火墙规则 8（到 Internet）不适用。 转到下一规则。
    1. 防火墙规则 9 (DNS) 不适用。 转到下一规则。
    1. 防火墙规则 10（子网内部）适用。 允许流量。 防火墙将流量传递到 10.0.1.4 (IIS01)。
1. 前端子网开始处理入站规则：
    1. NSG 规则 1（阻止 Internet）不适用，转到下一规则。
    1. 默认 NSG 规则允许子网到子网的流量，因此允许流量。 停止 NSG 规则处理。
1. 假设有适当的身份验证和授权，IIS01 将接受请求和响应。
1. 前端子网上的 UDR 路由使防火墙成为下一跃点。
1. 前端子网上没有出站 NSG 规则，因此允许响应。
1. 此会话已在防火墙上存在，因此允许此响应。 防火墙将响应返回给 AppVM02。
1. 后端子网执行入站规则处理：
    1. NSG 规则 1（阻止 Internet）不适用。 转到下一规则。
    2. 默认 NSG 规则允许子网到子网的流量，因此允许流量。 停止 NSG 规则处理。
1. AppVM02 接收响应。

### <a name="denied-internet-direct-to-web-server"></a>（拒绝）从 Internet 直接访问 Web 服务器

1. Internet 用户尝试通过 FrontEnd001.CloudApp.Net 服务访问 IIS01 Web 服务器。
1. 没有为 HTTP 流量打开终结点，因此，此流量不会通过云服务。 流量不会抵达服务器。
1. 如果出于某种原因而打开了终结点，前端子网上的 NSG（阻止 Internet）将阻止此流量。
1. 最终，前端子网 UDR 路由将任何来自 IIS01 的出站流量发送到用作下一跃点的防火墙。 防火墙将此流量视为非对称流量，并丢弃出站响应。

>因此，Internet 与 IIS01 之间至少有三个独立防御层。 云服务可以防止未经授权或不当的访问。

### <a name="denied-internet-to-back-end-server"></a>（拒绝）通过 Internet 访问后端服务器

1. Internet 用户尝试通过 BackEnd001.CloudApp.Net 服务访问 AppVM01 上的文件。
2. 由于没有为文件共享打开终结点，因此，此请求不会通过云服务。 流量不会抵达服务器。
3. 如果出于某种原因而打开了终结点，NSG（阻止 Internet）将阻止此流量。
4. 最终，UDR 路由将任何来自 AppVM01 的出站流量发送到用作下一跃点的防火墙。 防火墙将此流量视为非对称流量，并丢弃出站响应。

> 因此，Internet 与 AppVM01 之间至少有三个独立防御层。 云服务可以防止未经授权或不当的访问。

### <a name="denied-front-end-server-to-back-end-server"></a>（拒绝）前端服务器到后端服务器

1. IIS01 遭到入侵，正在运行恶意代码以尝试扫描后端子网服务器。
1. 前端子网 UDR 路由将任何来自 IIS01 的出站流量发送到用作下一跃点的防火墙。 遭到入侵的 VM 无法改变此路由。
1. 防火墙处理流量。 如果请求是针对 AppVM01 发出的，或者是为了执行 DNS 查找而对 DNS 服务器发出的，则防火墙可能会允许该流量，因为应用了防火墙规则 7 和 9。 其他所有流量将被防火墙规则 11（全部拒绝）阻止。
1. 如果在防火墙上启用了高级威胁检测，则可以阻止包含已知签名或模式（带有高级威胁规则标志）的流量。 即使是对于本文所述的基本转发规则所允许的流量，也可以采用此措施。 本文档不会介绍高级威胁检测。 请参阅供应商文档来了解特定网络设备的高级威胁功能。

### <a name="denied-internet-dns-lookup-on-dns-server"></a>（拒绝）在 DNS 服务器上执行 Internet DNS 查找

1. Internet 用户尝试通过 BackEnd001.CloudApp.Net 服务查找 DNS01 上的内部 DNS 记录。
1. 由于没有为 DNS 流量打开终结点，因此，此流量不会通过云服务。 流量不会抵达服务器。
1. 如果出于某种原因而打开了终结点，前端子网上的 NSG 规则（阻止 Internet）将阻止此流量。
1. 最终，后端子网 UDR 路由将任何来自 DNS01 的出站流量发送到用作下一跃点的防火墙。 防火墙将此流量视为非对称流量，并丢弃出站响应。

> 因此，Internet 与 DNS01 之间至少有三个独立防御层。 云服务可以防止未经授权或不当的访问。

#### <a name="denied-internet-to-sql-access-through-firewall"></a>（拒绝）从 Internet 通过防火墙访问 SQL

1. Internet 用户从 SecSvc001.CloudApp.Net（面向 Internet 的云服务）请求 SQL 数据。
1. 没有为 SQL 打开终结点，因此，此流量不会通过云服务。 流量不会抵达防火墙。
1. 如果出于某种原因而打开 SQL 终结点，防火墙会执行规则处理：
    1. 防火墙规则 1 (FW Mgmt) 不适用。 转到下一规则。
    1. 防火墙规则 2 - 5（RDP 规则）不适用。 转到下一规则。
    1. 防火墙规则 6 和 7（应用程序规则）不适用。 转到下一规则。
    1. 防火墙规则 8（到 Internet）不适用。 转到下一规则。
    1. 防火墙规则 9 (DNS) 不适用。 转到下一规则。
    1. 防火墙规则 10（子网内部）不适用。 转到下一规则。
    1. 防火墙规则 11（全部拒绝）适用。 阻止流量。 停止规则处理。

## <a name="references"></a>参考

本部分包含以下项：

* 完整脚本。 请将它保存在 PowerShell 脚本文件中。
* 网络配置。请将它保存到名为 NetworkConf2.xml 的文件中。

如有需要，请修改文件中用户定义的变量。 运行该脚本，并遵照本文前面列出的防火墙规则设置说明操作。

### <a name="full-script"></a>完整脚本

设置用户定义的变量后，请运行此脚本，以便：

1. 连接到 Azure 订阅
1. 新建存储帐户
1. 根据网络配置文件中的定义创建新的虚拟网络和三个子网
1. 构建五个虚拟机：一个防火墙和四个 Windows Server VM
1. 配置 UDR：
    1. 创建两个新的路由表
    1. 将路由添加到表
    1. 将表绑定到相应的子网
1. 在 NVA 上启用 IP 转发
1. 配置 NSG：
    1. 创建 NSG
    1. 添加规则
    1. 将 NSG 绑定到相应的子网

在已连接到 Internet 的电脑或服务器本地运行此 PowerShell 脚本。

> [!IMPORTANT]
> 运行此脚本时，PowerShell 中可能会弹出警告或其他参考性消息。 只需关注红色的错误消息。

```powershell
    <#
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwarding from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "China North"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Variables or  #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount -Environment AzureChinaCloud
        Set-AzureSubscription -SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) {
            Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription -SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] -InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  -SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM -ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] -InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  -SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM -ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Associate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the FrontEnd and BackEnd subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install BackEnd resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
```

### <a name="network-config-file"></a>网络配置文件

使用更新的位置保存此 XML 文件。 更改上述完整脚本中的 `$NetworkConfigFile` 变量，以链接到保存的网络配置文件。

```xml
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="China North">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>
```

## <a name="next-steps"></a>后续步骤

可以安装一个示例应用程序来帮助演练本外围网络示例。

> [!div class="nextstepaction"]
> [示例应用程序脚本](./virtual-networks-sample-app.md)

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "使用 NVA、NSG 和 UDR 的双向外围网络"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "防火墙规则的逻辑视图"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "创建前端网络对象"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "创建 DNS 服务器对象"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "默认 RDP 规则的副本"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 规则"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "应用程序重定向图标"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "目标 NAT 图标"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "传递图标"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "防火墙管理规则"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "防火墙 RDP 规则"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "防火墙 Web 规则"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "防火墙 AppVM01 规则"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "防火墙出站规则"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "防火墙 DNS 规则"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "防火墙 VNet 内部规则"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "防火墙拒绝规则"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "防火墙规则激活"

<!--Link References-->
<!--Not Available on [HOME]: ../best-practices-network-security.md-->

<!-- Update_Description: wording update, update link -->