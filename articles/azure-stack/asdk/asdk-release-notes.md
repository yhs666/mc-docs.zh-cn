---
title: Azure Stack 开发工具包发行说明 | Microsoft Docs
description: Azure Stack 开发工具包的改进、修复和已知问题。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/28/2019
ms.date: 07/29/2019
ms.author: v-jay
ms.reviewer: misainat
ms.lastreviewed: 06/28/2019
ms.openlocfilehash: 50ba7682d802341d9b9f0f5a6a968e38f273f830
ms.sourcegitcommit: 4d34571d65d908124039b734ddc51091122fa2bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68513503"
---
# <a name="asdk-release-notes"></a>ASDK 发行说明

本文介绍了 Azure Stack 开发工具包 (ASDK) 中的更改、修复和已知问题。 如果不确定所运行的版本，可以[使用门户检查版本](../operator/azure-stack-updates.md#determine-the-current-version)。

请订阅 [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS 源](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)，随时了解 ASDK 的新增功能。

## <a name="build-11906030"></a>内部版本 1.1906.0.30

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1906.md#whats-in-this-update)。

### <a name="changes"></a>更改

- 添加了 **AzS-SRNG01** 支持环 VM，用于托管 Azure Stack 的日志收集服务。 有关详细信息，请参阅[虚拟机角色](asdk-architecture.md)。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 使用某些市场映像创建虚拟机资源时，可能无法完成部署。 作为一种解决方法，可以单击“摘要”  页中的“下载模板和参数”  链接，然后单击“模板”  边栏选项卡中的“部署”  按钮。 
- 如需此版本中已修复的 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1906.md#fixes)。
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1906.md)。
- 请注意，[发布的 Azure Stack 修补程序](../operator/azure-stack-release-notes-1906.md#hotfixes)不适用于 Azure Stack ASDK。

## <a name="build-11905040"></a>内部版本 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1905.md#whats-in-this-update)。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 修复了必须编辑 **RegisterWithAzure.psm1** PowerShell 脚本才能成功[注册 ASDK](asdk-register.md) 的问题。
- 如需此版本中已修复的其他 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1905.md#fixes)。
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1905.md)。
- 请注意，[发布的 Azure Stack 修补程序](../operator/azure-stack-release-notes-1905.md#hotfixes)不适用于 Azure Stack ASDK。

## <a name="build-11904036"></a>内部版本 1.1904.0.36

<!-- ### Changes -->

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1904.md#whats-in-this-update)。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 由于运行注册脚本时服务主体超时，若要成功[注册 ASDK](asdk-register.md)，必须编辑 **RegisterWithAzure.psm1** PowerShell 脚本。 请执行以下操作：

  1. 在 ASDK 主计算机上，在具有提升权限的编辑器中打开文件 **C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1**。
  2. 在第 1249 行的末尾添加 `-TimeoutInSeconds 1800` 参数。 这是必需的，因为在运行注册脚本时服务主体超时。 第 1249 行现在应如下所示：

     ```powershell
      $servicePrincipal = Invoke-Command -Session $PSSession -ScriptBlock { New-AzureBridgeServicePrincipal -RefreshToken $using:RefreshToken -AzureEnvironment $using:AzureEnvironmentName -TenantId $using:TenantId -TimeoutInSeconds 1800 }
      ```

- 修复了在[此处（版本 1902 中）](#known-issues)确定的 VPN 连接问题。

- 如需此版本中已修复的其他 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1904.md#fixes)。
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1904.md)。
- 请注意，[发布的 Azure Stack 修补程序](../operator/azure-stack-release-notes-1904.md#hotfixes)不适用于 Azure Stack ASDK。

## <a name="build-1903"></a>内部版本 1903

1903 有效负载不包括 ASDK 发行版。

### <a name="known-issues"></a>已知问题

- 使用[此文](asdk-connect.md)中的步骤从另一主机建立到 ASDK 的 VPN 连接时，会出现问题。 尝试从 VPN 客户端连接到 ASDK 环境时，会看到错误“用户名或密码不正确”。即使你确定使用的帐户和键入的密码是正确的，也会出现这种情况。  此问题不是出在凭据，而是出在对特定身份验证协议（在 ASDK 上用于 VPN 连接的协议）进行的更改。 若要解决此问题，请执行以下步骤：

   首先，请更改在 ASDK 服务器端使用的身份验证协议：

   1. 通过 RDP 登录到 ASDK 主机。
   2. 打开提升的 PowerShell 会话，使用在部署时提供的密码以 AzureStack\AzureStackAdmin 身份登录。
   3. 运行以下命令：

      ```powershell
      netsh nps set np name = "Connections to Microsoft Routing and Remote Access server" profileid = "0x100a" profiledata = "1A000000000000000000000000000000" profileid = "0x1009" profiledata = "0x5"
      restart-service remoteaccess -force
      ```

   接下来，修改客户端连接脚本。 若要这样做，最简单的方法是直接更改 C:\AzureStack-Tools-master\connect\azurestack.connect.psm1 脚本模块：

   1. 修改 **Add-AzsVpnConnection** cmdlet，将 `AuthenticationMethod` 参数从 `MsChapv2` 更改为 `EAP`：

      ```powershell
      $connection = Add-VpnConnection -Name $ConnectionName -ServerAddress $ServerAddress -TunnelType L2tp -EncryptionLevel Required -AuthenticationMethod Eap -L2tpPsk $PlainPassword -Force -RememberCredential -PassThru -SplitTunneling
      ```

   2. 将 **Connect-AzsVpn** cmdlet 从使用 `rasdial @ConnectionName $User $PlainPassword` 更改为使用 `rasphone`，因为 EAP 要求交互式登录：

      ```powershell
      rasphone $ConnectionName
      ```

   3. 保存所做的更改，然后重新导入 **azurestack.connect.psm1** 模块。
   4. 请按[此文](asdk-connect.md#set-up-vpn-connectivity)说明执行操作。
   5. 通过 VPN 连接到 ASDK 时，请这样连接：先导航到 Windows 的“网络和 Internet 设置”  ，然后转到“VPN”，而不是  从任务栏进行连接，这样可确保系统会提示你输入凭据。

- 我们已识别到以下问题：发往内部负载均衡器 (ILB) 的超过 1450 字节的数据包将被丢弃。 该问题的原因是主机上的 MTU 设置过小，无法容纳遍历角色的 VXLAN 封装数据包，自版本 1901 开始，该角色已移到主机。 在你可能遇到的至少两种情况中，我们已发现此问题会自行显现：

  - SQL 查询发往内部负载均衡器 (ILB) 后面的 SQL Always On，并且超过 660 字节。
  - 如果尝试启用多个主机，Kubernetes 部署将失败。  

  在同一虚拟网络但不同子网中的 VM 与 ILB 之间进行通信时，将发生此问题。 在 ASDK 主机上权限提升的命令提示符下运行以下命令即可解决此问题：

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```
