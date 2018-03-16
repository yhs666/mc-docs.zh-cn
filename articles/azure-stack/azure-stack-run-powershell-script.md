---
title: "部署 Azure Stack 开发工具包 | Microsoft Docs"
description: "了解如何准备 Azure Stack 开发工具包，然后运行 PowerShell 脚本对其进行部署。"
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 12/15/2017
ms.date: 03/04/2018
ms.author: v-junlch
ms.reviewer: wfayed
ms.openlocfilehash: fa86c1dc61139fdc9eea44bc99c6427a9c6a7de6
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="deploy-the-azure-stack-development-kit"></a>部署 Azure Stack 开发工具包

*适用于：Azure Stack 开发工具包*

若要部署 [Azure Stack 开发工具包](azure-stack-poc.md)，必须完成以下步骤：

1. [下载部署包](https://azure.microsoft.com/overview/azure-stack/try/?v=try)以获取 Cloudbuilder.vhdx。
2. 准备 cloudbuilder.vhdx，对计算机（开发工具包主机）进行配置，以便在其上安装开发工具包。 此步骤后，开发工具包主机会启动到 Cloudbuilder.vhdx。
3. 在开发工具包主机上部署开发工具包。

> [!NOTE]
> 为了获得最佳结果，即使你想要使用断开连接的 Azure Stack 环境，也最好是在连接到 Internet 的情况下进行部署。 这样就可以在部署时激活开发工具包安装版随附的 Windows Server 2016 评估版。

## <a name="download-and-extract-the-development-kit"></a>下载并提取开发工具包
1. 开始下载之前，请确保计算机满足以下先决条件：

    - 计算机必须有至少 60 GB 的可用磁盘空间，这些空间位于除操作系统磁盘外的四个独立且相同的逻辑硬盘驱动器上。
    - 必须已安装 [.NET Framework 4.6（或更高版本）](https://aka.ms/r6mkiy)。

2. [转到“入门”页](https://azure.microsoft.com/overview/azure-stack/try/?v=try)，以便在其中下载 Azure Stack 开发工具包，提供自己的详细信息，然后单击“提交”。
3. 下载并运行先决条件检查器脚本：[用于 Azure Stack 开发工具包的部署检查器](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409)。 此独立脚本完成由 Azure Stack 开发工具包的安装程序执行的先决条件检查。 在下载更大的用于 Azure Stack 开发工具包的程序包之前，可以通过它来确认硬件和软件要求是否已得到满足。
4. 在“下载软件”下单击“Azure Stack 开发工具包”。

    > [!NOTE]
    > ASDK 下载项 (AzureStackDevelopmentKit.exe) 本身大约为 10GB。 如果选择也下载 Windows Server 2016 评估版 ISO 文件，则下载大小增至约 17GB。 在 ASDK 安装完成后，可以使用该 ISO 文件创建 Windows Server 2016 虚拟机映像并将其添加到 Azure Stack Marketplace。 请注意，此 Windows Server 2016 评估映像只能与 ASDK 配合使用，并且受 ASDK 许可条款约束。

5. 下载完成后，请单击“运行”，启动 ASDK 自解压缩程序 (AzureStackDevelopmentKit.exe)。
6. 查看并接受自解压缩程序向导的“许可协议”页中显示的许可协议，然后单击“下一步”。
7. 查看自解压缩程序向导的“重要说明”页上显示的隐私声明信息，然后单击“下一步”。
8. 在自解压缩程序向导的“选择目标位置”页上选择要将 Azure Stack 安装程序文件提取到其中的位置，然后单击“下一步”。 默认位置为：当前文件夹\Azure Stack Development Kit。 
9. 查看自解压缩程序向导的“准备提取”页上的目标位置摘要，然后单击“提取”以提取 CloudBuilder.vhdx（约 25GB）和 ThirdPartyLicenses.rtf 文件。 此过程将需要一段时间才能完成。
10. 将 CloudBuilder.vhdx 文件复制或移动到 ASDK 主机上的 C:\ 驱动器的根目录 (C:\CloudBuilder.vhdx)。

> [!NOTE]
> 提取这些文件以后，即可通过删除 .EXE 和 .BIN 文件来恢复硬盘空间。 也可备份这些文件，这样当你需要重新部署 ASDK 时，就不需再次下载这些文件。

## <a name="deploy-the-asdk-using-a-guided-experience"></a>通过引导式体验部署 ASDK
ASDK 可以使用图形用户界面 (GUI) 进行部署，该界面可以通过下载并运行 asdk-installer.ps1 PowerShell 脚本活得。

> [!NOTE]
> Azure Stack 开发工具包的安装程序用户界面是一种基于 WCF 和 PowerShell 的开源脚本。

### <a name="prepare-the-development-kit-host-using-a-guided-user-experience"></a>通过引导式用户体验准备开发工具包主机
必须先准备 ASDK 环境，然后才能在主机上安装 ASDK。
1. 以本地管理员身份登录到 ASDK 主机。
2. 确保已将 CloudBuilder.vhdx 文件移动到 C:\ 驱动器的根目录 (C:\CloudBuilder.vhdx)。
3. 运行以下脚本，将开发工具包安装程序文件 (asdk-installer.ps1) 从 [Azure Stack GitHub 工具存储库](https://github.com/Azure/AzureStack-Tools)下载到开发工具包主机上的 **C:\AzureStack_Installer** 文件夹：

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
    $LocalPath = 'C:\AzureStack_Installer'
    # Create folder
    New-Item $LocalPath -Type directory
    # Download file
    Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
    ```

4. 从提升了权限的 PowerShell 控制台启动 **C:\AzureStack_Installer\asdk-installer.ps1** 脚本，然后单击“准备环境”。
5. 在安装程序的“选择 Cloudbuilder vhdx”页上浏览到 **cloudbuilder.vhdx** 文件并将其选中，该文件是在前述步骤中下载并提取的。 如果需要向开发工具包主机添加其他驱动程序，则也可选择在此页上启用“添加驱动程序”复选框。  
6. 在“可选设置”页上，提供开发工具包主机 的本地管理员帐户。 

    > [!IMPORTANT]
    > 如果不提供这些凭据，则需在计算机因设置开发工具包而重启后直接访问或通过 KVM 访问主机。

7. 也可在“可选设置”页上提供这些可选设置：
    - **Computername**：此选项设置开发工具包主机的名称。 名称必须符合 FQDN 要求，且长度不得超过 15 个字符。 默认值是由 Windows 生成的随机计算机名称。
    - **时区**：设置开发工具包主机的时区。 默认为“(UTC-8:00)太平洋时间(美国和加拿大)”。
    - **静态 IP 配置**：将部署设置为使用静态 IP 地址。 否则，当安装程序重启到 cloudbuilder.vhx 中时，会使用 DHCP 来配置网络接口。
11. 单击“下一步”。
12. 如果上一步选择了一个静态 IP 配置，则现在必须执行以下步骤：
    - 选择网络适配器。 确保可以连接到该适配器，然后单击“下一步”。
    - 确保 **IP 地址**、**网关**和 **DNS** 值正确，然后单击“下一步”。
13. 单击“下一步”，启动准备过程。
14. 但准备过程指示“已完成”时，单击“下一步”。
15. 单击“立即重启”，启动到 cloudbuilder.vhdx 中，然后继续部署过程。


### <a name="deploy-the-development-kit-using-a-guided-experience"></a>通过引导式体验部署开发工具包
准备 ASDK 主机以后，即可使用以下步骤将 ASDK 部署到 CloudBuilder.vhdx 映像中。 
1. 在主机成功启动到 CloudBuilder.vhdx 映像中以后，使用在前述步骤中指定的管理员凭据登录。 
2. 打开提升了权限的 PowerShell 控制台，运行 **\AzureStack_Installer\asdk-installer.ps1** 脚本（现在可能位于 CloudBuilder.vhdx 映像的另一驱动器上）。 单击“安装”。
3. 在“类型”下拉框中，选择“Azure 云”或“AD FS”。
    - **Azure 云**：将 Azure Active Directory (Azure AD) 配置为标识提供者。 若要使用此选项，需要 Internet 连接、采用 *domainname*.partner.onmschina.cn 格式的 Azure AD 目录租户的完整名称或 Azure AD 验证的自定义域名，以及指定目录的全局管理员凭据。 
    - **AD FS**：默认的标记目录服务将用作标识提供者。 登录时使用的默认帐户是 azurestackadmin@azurestack.local，要使用的密码是在设置过程中提供的。
4. 在“本地管理员密码”下的“密码”框中，键入本地管理员密码（必须与当前配置的本地管理员密码相符），然后单击“下一步”。
5. 选择用于开发工具包的网络适配器，然后单击“下一步”。
6. 为 BGPNAT01 虚拟机选择 DHCP 或静态网络配置。
    - **DHCP**（默认）：虚拟机 DHCP 服务器获取 IP 网络配置。
    - **静态**：仅当 DHCP 无法为 Azure Stack 分配可访问 Internet 的有效 IP 地址时，才使用此选项。 静态 IP 地址必须在 CIDR 格式中使用子网掩码长度来指定（例如，10.0.0.5/24）。
7. 也可设置以下值：
    - **VLAN ID**：设置 VLAN ID。 仅当主机和 AzS-BGPNAT01 必须通过配置 VLAN ID 来访问物理网络（和 Internet）时，才使用此选项。 
    - **DNS 转发器**：在 Azure Stack 部署过程中会创建 DNS 服务器。 若要允许解决方案中的计算机解析标记外部的名称，请提供现有的基础结构 DNS 服务器。 标记内 DNS 服务器将未知的名称解析请求转发至此服务器。
    - **时间服务器**：此必填字段设置可供开发工具包使用的时间服务器。 必须以有效的时间服务器 IP 地址的形式提供此参数。 服务器名称不受支持。
  
    > [!TIP]
    > 若要查找时间服务器 IP 地址，请访问 [pool.ntp.org](http:\\pool.ntp.org) 或 ping time.windows.com。 
  
8. 单击“下一步”。 
9. 在“验证网络接口卡属性”页上会看到一个进度栏。 
    - 如果其中显示“无法下载更新”，则请按页面中的说明操作。
    - 当显示“已完成”时，请单击“下一步”。
10. 在“摘要”页上单击“部署”。 也可在这里复制将要用来安装开发工具包的 PowerShell 安装程序命令。
11. 如果使用的是 Azure AD 部署，系统会在安装开始后数分钟提示输入 Azure AD 全局管理员帐户凭据。
12. 部署过程可能需要数小时，在此期间系统会自动重启一次。 如果部署成功，PowerShell 控制台会显示“完成操作‘部署’”。 如果部署失败，可以从头[重新部署](azure-stack-redeploy.md)，也可以在同一个提升了权限的 PowerShell 窗口中使用以下 PowerShell 命令，从最后一个成功步骤重启部署：

    ```powershell
    cd C:\CloudDeployment\Setup
    .\InstallAzureStackPOC.ps1 -Rerun
    ```

    > [!IMPORTANT]
    > 若要监视部署进度，则请在开发工具包主机在安装过程中重启时，以 azurestack\AzureStackAdmin 身份登录。 如果在计算机加入域后以本地管理员身份登录，则看不到部署进度。 请勿重新运行部署，而应以 azurestack\AzureStackAdmin 身份登录，验证其是否正在运行。
    

## <a name="deploy-the-asdk-using-powershell"></a>使用 PowerShell 部署 ASDK
前面的步骤介绍了如何通过引导式用户体验部署 ASDK。 也可按照此部分的步骤，使用 PowerShell 将 ASDK 部署在开发工具包主机上。

### <a name="configure-the-asdk-host-computer-to-boot-from-cloudbuildervhdx"></a>将 ASDK 主机配置为从 CloudBuilder.vhdx 启动
以下命令会将 ASDK 主机配置为从下载后提取的 Azure Stack 虚拟硬盘 (CloudBuilder.vhdx) 启动。 完成这些步骤后，请重启 ASDK 主机。

1. 以管理员身份启动命令提示符。
2. 运行 `bcdedit /copy {current} /d "Azure Stack"`
3. 复制 (CTRL+C) 返回的 CLSID 值，包括必需的 {}。 该值称为 {CLSID}，需要在余下的步骤中粘贴进去（CTRL+V 或右键单击）。
4. 运行 `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` 
5. 运行 `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` 
6. 运行 `bcdedit /set {CLSID} detecthal on` 
7. 运行 `bcdedit /default {CLSID}`
8. 若要验证启动设置，请运行 `bcdedit`。 
9. 确保已将 CloudBuilder.vhdx 文件移动到 C:\ 驱动器的根目录 (C:\CloudBuilder.vhdx)，然后重启开发工具包主机。 ASDK 主机在重启后会默认为从 CloudBuilder.vhdx VM 启动。 

### <a name="prepare-the-development-kit-host-using-powershell"></a>使用 PowerShell 准备开发工具包主机 
在开发工具包主机成功启动到 CloudBuilder.vhdx 映像中以后，即可打开提升了权限的 PowerShell 控制台并运行此部分的命令，以便在开发工具包主机上部署 ASDK。

> [!IMPORTANT] 
> ASDK 安装支持使用一个网络接口卡 (NIC) 进行网络连接。 如果有多个 NIC，请确保只启用一个（所有其他 NIC 均禁用），然后再运行部署脚本。

可以将 Azure AD 或 AD FS 作为标识提供者来部署 Azure Stack。 Azure Stack、资源提供程序和其他应用程序使用这二者的方式是相同的。 若要详细了解使用 AD FS 时 Azure Stack 中支持的功能，请参阅[主要功能和概念](azure-stack-key-features.md)一文。

> [!TIP]
> 如果不提供任何安装参数（参见下面的 InstallAzureStackPOC.ps1 可选参数和示例），系统会提示输入必需的参数。

### <a name="deploy-azure-stack-using-azure-ad"></a>使用 Azure AD 部署 Azure Stack 
若要**使用 Azure AD 作为标识提供者**来部署 Azure Stack，必须通过直接方式或透明代理方式建立 Internet 连接。 运行以下 PowerShell 命令，以便使用 Azure AD 部署开发工具包：

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

  进行 ASDK 安装后数分钟，系统会提示输入 Azure AD 凭据。 必须提供 Azure AD 租户的全局管理员凭据。 

### <a name="deploy-azure-stack-using-ad-fs"></a>使用 AD FS 部署 Azure Stack 
若要**使用 AD FS 作为标识提供者**来部署开发工具包，请运行以下 PowerShell 命令（只需添加 -UseADFS 参数）： 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

在 AD FS 部署中，默认的标记目录服务用作标识提供者。 登录时使用的默认帐户是 azurestackadmin@azurestack.local，而密码则会设置为在 PowerShell 安装命令中提供的密码。

部署过程可能需要数小时，在此期间系统会自动重启一次。 如果部署成功，PowerShell 控制台会显示“完成操作‘部署’”。 如果部署失败，可以尝试使用 -rerun 参数再次运行脚本。 也可从头开始[重新部署 ASDK](azure-stack-redeploy.md)。
> [!IMPORTANT]
> 若要在 ASDK 主机重启后监视部署进度，必须以 AzureStack\AzureStackAdmin 身份登录。 如果在主机重启（并加入 azurestack.local 域）后以本地管理员身份登录，将看不到部署进度。 请勿重新运行部署，而应以 azurestack 身份登录，验证其是否正在运行。
>

#### <a name="azure-ad-deployment-script-examples"></a>Azure AD 部署脚本示例
可以编写整个 Azure AD 部署的脚本。 下面是一些加注的示例，其中包括一些可选参数。

如果 Azure AD 标识只与**一个** Azure AD 目录相关联，请执行以下代码：
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

如果 Azure AD 标识与**多个** Azure AD 目录相关联，请执行以下代码：
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.partner.onmschina.cn 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.partner.onmschina.cn or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

如果环境**没有**启用 DHCP，则必须在上述某个选项中包括下述额外的参数（已提供用法示例）： 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>ASDK InstallAzureStackPOC.ps1 可选参数
|参数|必需/可选|说明|
|-----|-----|-----|
|AdminPassword|必须|在所有虚拟机（在开发工具包部署过程中创建）上设置本地管理员帐户和所有其他的用户帐户。 此密码必须与当前主机上的本地管理员密码匹配。|
|InfraAzureDirectoryTenantName|必须|设置租户目录。 使用此参数指定一个具体的目录，使 AAD 帐户有权在其中管理多个目录。 AAD Directory 租户的 .partner.onmschina.cn 格式的完整名称，或者 Azure AD 验证的自定义域名。|
|TimeServer|必须|使用此参数指定具体的时间服务器。 必须以有效的时间服务器 IP 地址的形式提供此参数。 服务器名称不受支持。|
|InfraAzureDirectoryTenantAdminCredential|可选|设置 Azure Active Directory 用户名和密码。 这些 Azure 凭据必须是组织 ID。|
|InfraAzureEnvironment|可选|选择 Azure 环境，以便将此 Azure Stack 部署注册到其中。 选项包括“公共 Azure”、“Azure - 中国”、“Azure - 美国政府”。|
|DNSForwarder|可选|在 Azure Stack 部署过程中会创建 DNS 服务器。 若要允许解决方案中的计算机解析标记外部的名称，请提供现有的基础结构 DNS 服务器。 标记内 DNS 服务器将未知的名称解析请求转发至此服务器。|
|NatIPv4Address|DHCP NAT 支持所需|为 MAS-BGPNAT01 设置静态 IP 地址。 仅当 DHCP 无法分配可以访问 Internet 的有效 IP 地址时，才使用此参数。|
|NatIPv4Subnet|DHCP NAT 支持所需|IP 子网前缀，适用于经 NAT 的 DHCP 支持。 仅当 DHCP 无法分配可以访问 Internet 的有效 IP 地址时，才使用此参数。|
|PublicVlanId|可选|设置 VLAN ID。 仅当主机和 MAS-BGPNAT01 必须通过配置 VLAN ID 来访问物理网络（和 Internet）时，才使用此参数。 例如，.\InstallAzureStackPOC.ps1 -Verbose -PublicVLan 305|
|Rerun|可选|使用此标志重新运行部署。 将使用所有以前的输入。 不支持重新输入以前提供的数据，因为已生成多个唯一的值并将其用于部署。|

## <a name="activate-the-administrator-and-tenant-portals"></a>激活管理员门户和租户门户
在完成使用 Azure AD 的部署以后，必须激活 Azure Stack 管理员门户和租户门户。 对于目录的所有用户来说，此激活是指同意为 Azure Stack 门户和 Azure 资源管理器提供正确的权限（已在同意页上列出）。

- 对于管理员门户，请导航到 https://adminportal.local.azurestack.external/guest/signup，阅读相关信息，然后单击“接受”。 接受后即可添加服务管理员，但这些管理员不能也是目录租户管理员。

- 对于租户门户，请导航到 https://portal.local.azurestack.external/guest/signup，阅读相关信息，然后单击“接受”。 接受后，目录中的用户即可登录到租户门户。 

> [!NOTE] 
> 如果门户未激活，则只有目录管理员可以登录并使用门户。 其他用户在登录时会看到一个错误，指出管理员尚未授予其他用户权限。 如果管理员原本不属于 Azure Stack 注册到的目录，则必须将 Azure Stack 目录追加到激活 URL。 例如，如果 Azure Stack 注册到 fabrikam.partner.onmschina.cn，而管理员用户为 admin@contoso.com，则请导航到 https://portal.local.azurestack.external/guest/signup/fabrikam.partner.onmschina.cn 来激活门户。 

## <a name="reset-the-password-expiration-policy"></a>重置密码过期策略 
若要确保开发工具包主机的密码不在评估期结束之前过期，请在部署 ASDK 之后执行以下步骤。

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>若要通过 Powershell 更改密码过期策略，请执行以下步骤：
在提升权限的 Powershell 控制台中，运行以下命令：

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>若要手动更改密码过期策略，请执行以下步骤：
1. 在开发工具包主机上打开“组策略管理”，然后导航到“组策略管理” - “林: azurestack.local” - “域” - “azurestack.local”。
2. 右键单击“默认域策略”，然后单击“编辑”。
3. 在组策略管理编辑器中，导航到“计算机配置” - “策略” - “Windows 设置” - “安全设置” - “帐户策略” - “密码策略”。
4. 在右窗格中，双击“密码最长期限”。
5. 在“密码最长期限属性”对话框中，将“密码有效天数”值更改为 180，然后单击“确定”。


## <a name="next-steps"></a>后续步骤

[连接到 Azure Stack](azure-stack-connect-azure-stack.md)

[设置适用于 Azure Stack 环境的 PowerShell](azure-stack-powershell-configure-quickstart.md)

[将 Azure Stack 注册到 Azure 订阅](azure-stack-register.md)




