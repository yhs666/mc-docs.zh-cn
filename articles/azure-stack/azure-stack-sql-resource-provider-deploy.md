---
title: 在 Azure Stack 中使用 SQL 数据库 | Microsoft Docs
description: 了解如何在 Azure Stack 中部署 SQL 数据库即服务，并通过便捷的步骤部署 SQL Server 资源提供程序适配器。
services: azure-stack
documentationCenter: ''
author: JeffGoldner
manager: bradleyb
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/09/2018
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: 5b868b7d29ec2dd71810083ec8cfe4ca40fee4d4
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="use-sql-databases-on-azure-stack"></a>在 Azure Stack 中使用 SQL 数据库

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用 SQL Server 资源提供程序适配器可以公开 [Azure Stack](azure-stack-poc.md) 的 SQL 数据库即服务。 安装资源提供程序并将它连接到一个或多个 SQL Server 实例之后，你和用户可以创建：
- 适用于云本地应用程序的数据库。
- 基于 SQL 的网站。
- 基于 SQL 的工作负荷。
无需每次预配托管 SQL Server 的虚拟机 (VM)。

资源提供程序并不支持 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)的所有数据库管理功能。 例如，不会提供弹性数据库池以及自动提升和降低数据库性能的功能。 但是，资源提供程序支持类似的创建、读取、更新和删除 (CRUD) 操作。 API 与 SQL 数据库不兼容。

## <a name="sql-resource-provider-adapter-architecture"></a>SQL 资源提供程序适配器体系结构
资源提供程序由三个组件构成：

- **SQL 资源提供程序适配器 VM**，即运行提供程序服务的 Windows 虚拟机。
- **资源提供程序本身**，处理预配请求并公开数据库资源。
- **托管 SQL 服务器的服务器**，为称作宿主服务器的数据库提供容量。

必须创建一个或多个 SQL Server 实例和/或提供对外部 SQL Server 实例的访问权限。

## <a name="deploy-the-resource-provider"></a>部署资源提供程序

1. 如果尚未这样做，请注册开发工具包，并通过 Marketplace 管理下载 Windows Server 2016 Datacenter Core 映像。 必须使用 Windows Server 2016 Core 映像。 也可以使用脚本创建 [Windows Server 2016 映像](/azure-stack/azure-stack-add-default-image)。 （请务必选择“Core”选项）。 不再需要 .NET 3.5 运行时。

2. 登录到可访问特权终结点 VM 的主机。

    - 在 Azure Stack 开发工具包安装中，登录到物理主机。

    - 在多节点系统上，该主机必须是可访问特权终结点的系统。
    
    >[!NOTE]
    > 运行脚本的系统必须是装有最新版 .NET 运行时的 Windows 10 或 Windows Server 2016 系统。 否则安装会失败。 Azure Stack SDK 主机满足此条件。


3. 下载 SQL 资源提供程序二进制文件。 然后运行自解压程序，将内容解压缩到临时目录。

    >[!NOTE] 
    > 资源提供程序内部版本对应于 Azure Stack 内部版本。 请务必下载适用于运行中 Azure Stack 版本的正确二进制文件。

    | Azure Stack 内部版本 | SQL 资源提供程序安装程序 |
    | --- | --- |
    |1.0.180102.3、1.0.180103.2 或 1.0.180106.1（多节点） | [SQL RP 版本 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [SQL RP 版本 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [SQL RP 版本 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. 从特权终结点检索 Azure Stack 根证书。 对于 Azure Stack SDK，将在此过程中创建自签名证书。 对于多节点，必须提供相应的证书。

   若要提供自己的证书，请将 .pfx 文件放在 **DependencyFilesLocalPath** 中，如下所示：

    - \*.dbadapter.\<区域\>.\<外部 FQDN\> 的通配符证书，或采用 sqladapter.dbadapter.\<区域\>.\<外部 FQDN\> 公用名的单个站点证书。

    - 此证书必须受信任。 也就是说，信任链必须存在，且不需要中间证书。

    - DependencyFilesLocalPath 中只存在单个证书文件。

    - 文件名不能包含任何特殊字符。


5. 打开权限提升的（管理）**新** PowerShell 控制台，并切换到解压缩后的文件所在的目录。 使用新窗口可以避免因系统上已加载的错误 PowerShell 模块可能造成的问题。

6. [安装 Azure PowerShell 版本 1.2.11](azure-stack-powershell-install.md)。

7. 运行 DeploySqlProvider.ps1 脚本以执行以下步骤：

    - 将证书和其他项目上传到 Azure Stack 上的存储帐户。
    - 发布库包，以便通过库部署 SQL 数据库。
    - 发布用于部署宿主服务器的库包。
    - 使用步骤 1 中创建的 Windows Server 2016 映像部署 VM，然后安装资源提供程序。
    - 注册映射到资源提供程序 VM 的本地 DNS 记录。
    - 将资源提供程序注册到本地 Azure 资源管理器（用户和管理员）。

> [!NOTE]
> 如果安装花费的时间超过 90 分钟，可能表示失败。 如果发生失败，屏幕和日志中会显示失败消息，但会从失败的步骤重试部署。 系统如果不符合建议的内存和 vCPU 规格，可能无法部署 SQL 资源提供程序。
>

下面是可从 PowerShell 命令提示符运行的示例命令。 （请务必根据需要更改帐户信息和密码。）

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.partner.onmschina.cn"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 参数
可以在命令行中指定这些参数。 如果未指定参数或任何参数验证失败，系统会提示提供所需的参数。

| 参数名称 | 说明 | 注释或默认值 |
| --- | --- | --- |
| **CloudAdminCredential** | 访问特权终结点时所需的云管理员凭据。 | _必需_ |
| **AzCredential** | Azure Stack 服务管理员帐户的凭据。 使用部署 Azure Stack 时所用的相同凭据。 | _必需_ |
| **VMLocalCredential** | SQL 资源提供程序 VM 的本地管理员帐户的凭据。 | _必需_ |
| **PrivilegedEndpoint** | 特权终结点的 IP 地址或 DNS 名称。 |  _必需_ |
| **DependencyFilesLocalPath** | 同样必须将证书 .pfx 文件放在此目录中。 | _可选_（对于多节点部署是_必需_的） |
| **DefaultSSLCertificatePassword** | .pfx 证书的密码。 | _必需_ |
| **MaxRetryCount** | 操作失败时，想要重试每个操作的次数。| 2 |
| **RetryDuration** | 每两次重试的超时间隔（秒）。 | 120 |
| **卸载** | 删除资源提供程序和所有关联的资源（请参阅下面的注释）。 | 否 |
| **DebugMode** | 防止在失败时自动清除。 | 否 |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>使用 Azure Stack 门户验证部署

> [!NOTE]
>  在安装脚本完成运行之后，必须刷新门户才能看到管理边栏选项卡。


1. 以服务管理员身份登录到管理门户。

2. 验证部署是否成功。 转到“资源组”。 然后选择“system.\<位置\>.sqladapter”资源组。 验证所有四个部署是否成功。

      ![验证 SQL 资源提供程序的部署](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>更新 SQL 资源提供程序适配器（仅限多节点，1710 和更高版本）
每当更新 Azure Stack 内部版本时，都会发布一个新的 SQL 资源提供程序适配器。 现有适配器可继续运行。 但是，我们建议在更新 Azure Stack 后，尽快更新到最新的内部版本。 

更新过程类似于前面所述的安装过程。 使用最新的资源提供程序代码创建新 VM。 此外，请将设置迁移到此新实例，包括数据库和宿主服务器信息。 还可以迁移所需的 DNS 记录。

结合前面所述的相同参数使用 UpdateSQLProvider.ps1 脚本。 在此处也必须提供证书。

> [!NOTE]
> 只有多节点系统才支持此更新过程。

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.partner.onmschina.cn"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 参数
可以在命令行中指定这些参数。 如果未指定参数或任何参数验证失败，系统会提示提供所需的参数。

| 参数名称 | 说明 | 注释或默认值 |
| --- | --- | --- |
| **CloudAdminCredential** | 访问特权终结点时所需的云管理员凭据。 | _必需_ |
| **AzCredential** | Azure Stack 服务管理员帐户的凭据。 使用部署 Azure Stack 时所用的相同凭据。 | _必需_ |
| **VMLocalCredential** | SQL 资源提供程序 VM 的本地管理员帐户的凭据。 | _必需_ |
| **PrivilegedEndpoint** | 特权终结点的 IP 地址或 DNS 名称。 |  _必需_ |
| **DependencyFilesLocalPath** | 同样必须将证书 .pfx 文件放在此目录中。 | _可选_（对于多节点部署是_必需_的） |
| **DefaultSSLCertificatePassword** | .pfx 证书的密码。 | （必需） |
| **MaxRetryCount** | 操作失败时，想要重试每个操作的次数。| 2 |
| **RetryDuration** |每两次重试的超时间隔（秒）。 | 120 |
| **卸载** | 删除资源提供程序和所有关联的资源（请参阅下面的注释）。 | 否 |
| **DebugMode** | 防止在失败时自动清除。 | 否 |



## <a name="remove-the-sql-resource-provider-adapter"></a>删除 SQL 资源提供程序适配器

若要删除资源提供程序，必须先删除所有依赖项。

1. 确保已保留针对此 SQL 资源提供程序适配器版本下载的原始部署包。

2. 必须从资源提供程序中删除所有用户数据库。 （删除用户数据库不会删除数据。）此任务应由用户自己执行。

3. 管理员必须从 SQL 资源提供程序适配器中删除宿主服务器。

4. 管理员必须删除引用 SQL 资源提供程序适配器的所有计划。

5. 管理员必须删除与 SQL 资源提供程序适配器关联的所有 SKU 和配额。

6. 使用以下元素重新运行部署脚本：
    - -Uninstall 参数
    - Azure 资源管理器终结点
    - DirectoryTenantID
    - 服务管理员帐户的凭据


## <a name="next-steps"></a>后续步骤

[添加宿主服务器](azure-stack-sql-resource-provider-hosting-servers.md)和[创建数据库](azure-stack-sql-resource-provider-databases.md)。

试用其他 [PaaS 服务](azure-stack-tools-paas-services.md)，例如 [MySQL Server 资源提供程序](azure-stack-mysql-resource-provider-deploy.md)和[应用服务资源提供程序](azure-stack-app-service-overview.md)。

