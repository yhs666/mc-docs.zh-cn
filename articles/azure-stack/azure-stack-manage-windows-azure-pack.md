---
title: 从 Azure Stack 管理 Azure Pack 虚拟机 | Microsoft Docs
description: 了解如何从 Azure Stack 中的用户门户管理 Azure Pack (WAP) VM。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 213c2792-d404-4b44-8340-235adf3f8f0b
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/28/2018
ms.date: 03/26/2018
ms.author: v-junlch
ms.openlocfilehash: c5eb0bfd716841e46939cf61b189b16e4886652c
ms.sourcegitcommit: 6d7f98c83372c978ac4030d3935c9829d6415bf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-azure-pack-virtual-machines-from-azure-stack"></a>从 Azure Stack 管理 Azure Pack 虚拟机

*适用于：Azure Stack 开发工具包*

在 Azure Stack 开发工具包中，可以从 Azure Stack 用户门户访问 Windows Azure Pack 上运行的租户虚拟机。 用户可以使用 Azure Stack 门户来管理其现有的 IaaS 虚拟机和虚拟网络。 可以通过底层 Service Provider Foundation (SPF) 和 Virtual Machine Manager (VMM) 组件在 Azure Pack 上使用这些资源。 具体而言，用户可以：

- 浏览资源
- 检查配置值
- 停止或启动虚拟机
- 通过远程桌面协议 (RDP) 连接到虚拟机
- 创建和管理检查点
- 删除虚拟机和虚拟网络

此功能由适用于 Azure Stack 的 Azure Pack 连接器（预览版）提供。 本文介绍如何配置适用于单节点 Azure Stack 环境的连接器。

对于此预览版连接器，请注意以下事项：
- 只能在测试环境中使用 Azure Pack 连接器（适用于 Azure Stack 和 Azure Pack），而不能在生产部署中使用。
- 必须安装 Azure Pack 更新汇总 (UR) 9.1 或更高版本，以及 System Center SPF 和 VMM UR 9 或更高版本。
- VMM 和 SPF 组件可以是 System Center 2012 R2 或 System Center 2016。
- 必须在 Azure Stack 和 Azure Pack 上执行配置步骤。
- 本文中的说明适用于非云平台系统 (CPS) 环境。
- 若要查看已知问题，请参阅 [Azure Stack 故障排除](azure-stack-troubleshooting.md)。


## <a name="architecture"></a>体系结构
下图显示了 Azure Pack 连接器的主要组件。

![Azure Pack 连接器组件](./media/azure-stack-manage-wap/image1.png)

请注意以下详细信息：
- Azure Stack 用户门户从两个云（Azure Stack 和 Azure Pack）访问资源信息。
- 用户必须在这两个环境中具有有效的帐户。
- Azure Stack 用户门户必须能够通过网络访问 Azure Pack 上运行的组件。
- 在示意图的 **WAP** 部分，可以看到新的 Azure Pack 连接器模块（WAP 扩展和连接器），以及包含 SPF 和 VMM 组件的现有 Azure Pack 租户 API。


## <a name="identity-management"></a>身份管理
Azure Pack 租户 API 必须信任 Azure Stack 安全令牌服务 (STS)。

当用户通过 Azure Stack 门户执行目标为 Azure Pack 资源的操作时，门户将使用 Azure Pack 租户 API。 因此，提供的用户身份验证令牌必须来自受信任的 STS。 参阅下图：

![Azure Pack 连接器身份验证示意图](./media/azure-stack-manage-wap/image2.png)

在开发工具包环境中，Azure Pack 和 Azure Stack 具有独立的标识提供者。 从 Azure Stack 门户访问这两个环境的用户在这两个标识提供者中的用户主体名称 (UPN) 必须相同。 例如，帐户 *azurestackadmin@azurestack.local* 在 Azure Pack 的 STS 中也应存在。 如果 AD FS 未设置为支持出站信任关系，则需要从 Azure Pack 组件（租户 API）建立对 AD FS 的 Azure Stack 实例的信任。

## <a name="prerequisites"></a>先决条件

### <a name="download-the-azure-pack-connector"></a>下载 Azure Pack 连接器
在 [Microsoft 下载中心](https://aka.ms/wapconnectorazurestackdlc)下载 .exe 文件，并将其提取到本地计算机。 稍后，要将内容复制到可以访问 Azure Pack 环境的计算机。

### <a name="deployment-option-requirement"></a>部署选项要求
若要与 Azure Pack 集成，可以使用 AD FS 选项或 Azure Active Directory 选项来部署 Azure Stack。

### <a name="connectivity-requirements"></a>连接要求
1. 确保能够在可以访问 Azure Stack 用户门户的计算机上通过 Web 浏览器访问 Azure Pack 租户门户。
2. Azure Stack 上的 AzS-WASP01 虚拟机必须能够连接到 Azure Pack 租户门户计算机。 使用 Ping.exe 验证网络连接。 
3. 必须拥有新连接器服务的有效证书。 这些 SSL 证书必须由受信任的证书颁发机构 (CA) 颁发。 不能使用自签名证书。 Azure Stack（具体而言，是 AzS-WASP01 VM）和可由其他任何租户用来访问 Azure Stack 用户门户的计算机都必须信任该 SSL 证书。
 
    >[!NOTE]
    由于 AzS-WASP01 运行包含服务器核心安装选项的 Windows Server，因此可以使用命令行工具（例如 Certutil.ext）来导入证书。 例如，可将 .cer 文件复制到 AzS-WASP01 上的 c:\temp，然后运行命令 **certutil -addstore "CA" "c:\temp\certname.cer"**。

4. 若要通过 Azure Stack 门户来与 Azure Pack 租户虚拟机建立 RDP 连接，Azure Pack 环境必须允许将远程桌面流量发往租户 VM。
5. 若要在 Azure Stack 租户虚拟机与 Azure Pack 租户虚拟机之间建立连接，这些虚拟机的外部 IP 地址必须可跨两个环境路由。 这种连接也可以涉及到关联 DNS 服务器，以解析环境之间的虚拟机名称。

### <a name="iis-requirements"></a>IIS 要求
托管 Azure Pack 租户门户的计算机（可以是一个物理主机、一个虚拟机或多个虚拟机）上必须安装 URL 重写 IIS 扩展。 如果尚未安装，可从[此处](https://www.iis.net/downloads/microsoft/url-rewrite)下载该扩展。 如果有多个虚拟机托管租户门户，请在每个虚拟机上安装该扩展。

## <a name="configure-azure-stack"></a>配置 Azure Stack
配置 Azure Pack 连接器之前，必须先在 Azure Stack 用户门户中启用多云模式。 此模式可让用户选择用于访问资源的云。

若要启用多云模式，必须在部署 Azure Stack 之后运行 Add-AzurePackConnector.ps1 脚本。 下表描述了脚本参数。


|  参数 | 说明 | 示例 |   
| -------- | ------------- | ------- |  
| AzurePackClouds | Azure Pack 连接器的 URI。 这些 URI 应该对应于 Azure Pack 租户门户。 | @{CloudName = "AzurePack1"; CloudEndpoint = "https://waptenantportal1:40005"},@{CloudName = "AzurePack2"; CloudEndpoint = "https://waptenantportal2:40005"}<br><br>  （默认情况下，端口值为 40005。） |  
| AzureStackCloudName | 表示本地 Azure Stack 云的标签。| "AzureStack" |
| DisableMultiCloud | 用于禁用多云模式的开关。| 不适用 |
| | |

可在部署之后紧接着运行 Add-AzurePackConnector.ps1 脚本，或者以后再运行。 若要在部署之后紧接着运行该脚本，请使用与完成 Azure Stack 部署相同的 Windows PowerShell 会话。 否则，可以使用管理员身份（以 azurestackadmin 帐户登录）打开新的 Windows PowerShell 会话。

1. 结合以下命令（包含环境特定的值）运行 Add-AzurePackConnector.ps1 脚本。 请注意，使用 Add-AzurePackConnector 脚本可以添加多个 Azure Pack 连接器终结点。
 
    ```powershell
        $cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local", `
        (ConvertTo-SecureString -String "<password>" -AsPlainText -Force))
        $session = New-PSSession -ComputerName 'azs-ercs01' `
        -Credential $cred `
        -ConfigurationName PrivilegedEndpoint `
        -Authentication Credssp

        # Enable Multicloud
        Invoke-Command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
        @{CloudName = "AzurePack_1"; CloudEndpoint = "https://waptenantportal1:40005"},`
        @{CloudName = "AzurePack_2"; CloudEndpoint = "https://waptenantportal2:40005" } `
        -AzureStackCloudName "AzureStack" }

    ```
    > [!NOTE]
    > 当前内部版本中存在一个问题，即 Add-AzurePackConnector 脚本在结束后，会长时间保留在轮询循环中（几分钟），直到结束。 看到消息“ VERBOSE: 步骤‘配置 Azure Pack 连接器’状态:‘成功’”后，可以停止脚本，或等到它自行停止。 由于配置已成功，因此可以忽略此问题。

2. 请针对指定的每个 Azure Pack 环境，记下此脚本生成的输出文件（每个环境各有一个）。 这些文件位于：\\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput。 这些文件包含配置目标 Azure Pack 环境所需的信息。 在稍后的说明中，需要将此文件作为参数传递给脚本。 此文件包含以下信息：

    - **AzurePackConnectorEndpoint**：包含 Azure Pack 连接器服务的终结点。
    - **AuthenticationIdentityProviderPartner**：包含以下值对：
        - Azure Pack 租户 API 需要信任的身份验证令牌签名证书，用于接受来自 Azure Stack 门户扩展的调用。

        - 与签名证书关联的“领域”。 例如：https://adfs.local.azurestack.global.external/adfs/c1d72562-534e-4aa5-92aa-d65df289a107/。

3. 浏览到包含输出文件的文件夹 (\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput)，并将文件复制到本地计算机。 文件如下所示：AzurePack-06-27-15-50.txt。

4. 测试配置。

    a. 打开浏览器并登录到 Azure Stack 用户门户 (https://portal.local.azurestack.external/)。
    
    b. 以租户身份登录并加载门户后，将会看到有关无法从 Azure Pack 云提取订阅或扩展的错误消息。 单击“确定”关闭这些消息。 （在配置 Azure Pack 后，这些错误消息就会消失。）

    c. 注意门户左上角的“云”下拉列表。

    ![Azure Stack 用户界面中的云选择器](./media/azure-stack-manage-wap/image3.png)

## <a name="configure-azure-pack"></a>配置 Azure Pack
需要手动配置 Azure Pack（只适用于此预览版连接器）。

>[!IMPORTANT]
对于此预览版，请只在测试环境中使用 Azure Pack 连接器，而不要在生产部署中使用。

1.  在 Azure Pack 租户门户虚拟机上安装连接器 MSI 文件，并安装证书。 （如果有多个租户门户虚拟机，则必须在每个虚拟机上完成此步骤。）

    a. 在文件资源管理器中，将前面下载的 **WAPConnector** 文件夹复制到租户门户虚拟机上名为 **c:\temp** 的文件夹中。

    b. 与租户门户虚拟机建立控制台或 RDP 连接。

    c. 将目录切换到 **c:\temp\wapconnector\setup\scripts**，并运行 **Install-Connector.ps1** 脚本来安装三个 MSI 文件。 无需指定任何参数。

     ```powershell
    cd C:\temp\wapconnector\setup\scripts\

    .\Install-Connector.ps1
    ```
     d.单击“验证存储凭据”以验证存储帐户。 将目录切换到 **c:\inetpub**，验证是否已安装这三个新站点：

       - MgmtSvc-Connector

       - MgmtSvc-ConnectorExtension

       - MgmtSvc-ConnectorController

    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，并单击“确定”。 在相同的 **c:\temp\wapconnector\setup\scripts** 文件夹中，运行 **Configure-Certificates.ps1** 脚本来安装证书。 默认情况下，该脚本将使用 Azure Pack 中的租户门户站点可用的相同证书。 请确保这是有效证书（受 Azure Stack AzS-WASP01 虚拟机和任何访问 Azure Stack 门户的客户端计算机的信任）。 否则无法通信。 （或者，也可以使用 -Thumbprint 参数以参数的形式显式传递证书指纹。）

     ```powershell
        cd C:\temp\wapconnector\setup\scripts\

        .\Configure-Certificates.ps1
    ```

    f.单击“保存”以保存设置。 若要完成这三个服务的配置，请运行 **Configure-WapConnector.ps1** 脚本来更新 Web.config 文件参数。

    |  参数 | 说明 | 示例 |   
    | -------- | ------------- | ------- |  
    | TenantPortalFQDN | Azure Pack 租户门户 FQDN。 | tenant.contoso.com | 
    | TenantAPIFQDN | Azure Pack 租户 API FQDN。 | tenantapi.contoso.com  |
    | AzureStackPortalFQDN | Azure Stack 用户门户 FQDN。 | portal.local.azurestack.external |
    | | |
    
     ```powershell
    .\Configure-WapConnector.ps1 -TenantPortalFQDN "tenant.contoso.com" `
         -TenantAPIFQDN "tenantapi.contoso.com" `
         -AzureStackPortalFQDN "portal.local.azurestack.external"
    ```
    g. 如果有多个租户门户虚拟机，请针对每个虚拟机重复步骤 1。

2. 在每个 Azure Pack 租户 API 虚拟机上安装新的租户 API MSI。

    a. 如果正在使用负载均衡器，可能需要将虚拟机标记为脱机。

    b. 在文件资源管理器中，将 **WAPConnector** 文件夹复制到每台租户 API 计算机上名为 **c:\temp** 的文件夹中。

    c. 将前面保存的 AzurePackConnectorOutput.txt 文件复制到 **c:\temp\WAPConnector**。

    d.单击“验证存储凭据”以验证存储帐户。 与文件复制到的租户 API VM 建立控制台或 RDP 连接。
    
    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，并单击“确定”。 将目录切换到 **c:\temp\wapconnector\setup\scripts**，并运行 **Update-TenantAPI.ps1**。 此新版 WAP 租户 API 包含更改，不仅可以启用与当前 STS 的信任关系，而且还能启用与 Azure Stack 中的 AD FS 实例的信任关系。

     ```powershell
    cd C:\temp\wapconnector\setup\packages\

    .\Update-TenantAPI.ps1
    ```

    f.单击“保存”以保存设置。  在每个运行租户 API 的虚拟机上重复步骤 2。
3. **只在一个**租户 API VM 上，运行 Configure-TrustAzureStack.ps1 脚本，以添加与租户 API 和 Azure Stack 上 AD FS 实例之间的信任关系。 必须使用对 Microsoft.MgmtSvc.Store 数据库拥有管理员访问权限的帐户。 此脚本采用以下参数：

    | 参数 | 说明 | 示例 |
    | --------- | ------------| ------- |
   | SqlServer | 包含 Microsoft.MgmtSvc.Store 数据库的 SQL Server 名称。 此参数是必需的。 | SQLServer | 
   | DataFile | 此输出文件是前面在配置 Azure Stack 多云模式期间生成的。 此参数是必需的。 | AzurePack-06-27-15-50.txt | 
   | PromptForSqlCredential | 指示脚本应以交互方式提示要连接到 SQL Server 实例时使用的 SQL 身份验证凭据。 给定的凭据必须拥有足够的权限，可卸载数据库、架构以及删除用户登录名。 如果未提供任何值，则脚本假设当前的用户上下文拥有访问权限。 | 无需指定任何值。 |
   |  |  |

    如果不知道要使用的 SQL 服务器，可以执行发现。 连接到租户 API 计算机，使用 Unprotect-MgmtSvc 命令取消保护租户 API Web.config 文件，并在连接字符串中找到服务器名称。 请记得再次运行 Protect-MgmtSvc 以保护租户 API Web.config 文件。

  ```powershell
  cd C:\temp\wapconnector\setup\scripts\

 .\Configure-TrustAzureStack.ps1 -SqlServer "SQLServer" `
       -DataFile "C:\temp\wapconnector\AzurePackConnectorOutput.txt"
  ```

## <a name="example"></a>示例
以下示例演示在单节点 Azure Stack 配置中的完整 Azure Pack 连接器部署，以及两种 Azure Pack 快速安装。 （每种快速安装位于单台计算机上，示例名称为 *wapcomputer1* 和 *wapcomputer2*。）

```powershell
# Run the following script on the Azure Stack host
$cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local",`
     (ConvertTo-SecureString -String "p@ssw0rd" -AsPlainText -Force))
$session = New-PSSession -ComputerName 'azs-ercs01' -Credential $cred `
     -ConfigurationName PrivilegedEndpoint -Authentication Credssp
 
# Enable Multicloud
invoke-command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
     @{CloudName = "AzurePack_1"; CloudEndpoint = "https://wapcomputer1.contoso.com:40005"},`
     @{CloudName = "AzurePack_2"; CloudEndpoint = "https://wapcomputer2.contoso.com:40005"}`
     -AzureStackCloudName "AzureStack" }  

```
从 [Microsoft 下载中心](https://aka.ms/wapconnectorazurestackdlc)下载并提取 .exe 文件，将 WAPConnector 文件夹复制到 Azure Pack 计算机上的 **c:\temp** 文件夹中。 将上述脚本中生成的输出文件（位于 \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput）复制到 **c:\temp\WAPConnector** 文件夹中。 （文件如下所示：AzurePack-06-27-15-50.txt。）然后运行以下脚本（对每个 Azure Pack 实例各运行一次）：

 ```powershell
# Install Connector components
cd C:\temp\WAPConnector\Setup\Scripts
.\Install-Connector.ps1

# Configure Certificates for the new Connector services
.\Configure-Certificates.ps1

# Configure the Connector services
.\Configure-WapConnector.ps1 -TenantPortalFQDN "wapcomputer1.contoso.com" `
     -TenantAPIFQDN "wapcomputer1.contoso.com" `
     -AzureStackPortalFQDN "portal.local.azurestack.external"

# Install the updated TenantAPI
.\Update-TenantAPI.ps1

# Establish trust with the Azure Stack AD FS
.\Configure-TrustAzureStack.ps1 -SqlServer "wapcomputer1" `
     -DataFile "C:\temp\wapconnector\AzurePack-06-27-15-50.txt" 

```
## <a name="troubleshooting-tips"></a>故障排除提示
1. 确保 Azure Stack 与 Azure Pack 之间建立了网络连接。 在任何访问 Azure Stack 门户的租户计算机与运行新连接器服务的 Azure Pack 租户门户虚拟机之间，都应该建立这种连接。
2. 确保所有指定的 FQDN 正确。
3. 确保 Azure Stack（具体而言，是 AzS-WASP01 VM）和可由其他任何租户用来访问 Azure Stack 用户门户的计算机信任新连接器服务中使用的 SSL 证书。
4. 有关已知问题，请参阅 [Azure Stack 故障排除](azure-stack-troubleshooting.md)。


## <a name="next-steps"></a>后续步骤
[在 Azure Stack 中使用管理员门户和用户门户](azure-stack-manage-portals.md)

<!-- Update_Description: update metedata properties -->