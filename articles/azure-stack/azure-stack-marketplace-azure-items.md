---
title: 适用于 Azure Stack 的 Azure Marketplace 项 | Microsoft Docs
description: 这些 Azure Marketplace 项可以用在 Azure Stack 中。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/14/2018
ms.date: 05/24/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: 2d74192ab74cb4e6da9fb9ac478a877af263c11f
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
---
# <a name="azure-marketplace-items-available-for-azure-stack"></a>适用于 Azure Stack 的 Azure Marketplace 项

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*


## <a name="virtual-machine-extensions"></a>虚拟机扩展
只要有所用虚拟机 (VM) 扩展的更新，就应下载它们。 随产品一起提供的扩展不会在普通的修补和更新过程中更新，因此请经常查看更新。 其他扩展只能通过 Marketplace 管理获取。

|  | 项名称 | 说明 | 发布者 | OS 类型 |
| --- | --- | --- | --- | --- |
|![SQL IaaS 扩展](./media/azure-stack-marketplace-azure-items/cse.png) | [ SQL IaaS 扩展 ](/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)| <b>下载此扩展以部署任何“Windows 上的 SQL Server”Marketplace项 - 此扩展是必需的。</b> | Microsoft | Windows |
|![自定义脚本扩展](./media/azure-stack-marketplace-azure-items/cse.png) | [ 自定义脚本扩展 ](/virtual-machines/windows/extensions-customscript)| <b>请下载此更新，此更新针对用于 Windows 的自定义脚本扩展的内置版本。</b> | Microsoft | Windows |
|![PowerShell DSC 扩展](./media/azure-stack-marketplace-azure-items/dsc.png) | [ PowerShell DSC 扩展 ](/virtual-machines/windows/extensions-dsc-overview)| <b>请将此更新下载到 PowerShell DSC 扩展的内置版本。更新为支持 TLS v1.2。</b> | Microsoft | Windows |
| ![Microsoft 反恶意软件扩展](./media/azure-stack-marketplace-azure-items/cse.png) | [ Microsoft 反恶意软件扩展 ](/security/azure-security-antimalware)| 适用于 Azure 的 Microsoft 反恶意软件是一个针对应用程序和租户环境所提供的单一代理解决方案，可在在后台运行而无需人工干预。 | Microsoft | Windows |
|![自定义脚本扩展](./media/azure-stack-marketplace-azure-items/cse.png) | [ 自定义脚本扩展 ](/virtual-machines/windows/extensions-customscript)| <b>请将此更新下载到 Linux 的自定义脚本扩展的内置版本。此扩展有多个版本，应下载 1.5.2.1 和 2.0.x。</b> | Microsoft | Linux |
| ![Docker 扩展](./media/azure-stack-marketplace-azure-items/dockerextension.png) | [Docker](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.docker-arm) | 适用于 Linux 虚拟机的 Docker 扩展。 | Microsoft | Linux |
| ![适用于 Linux 的 VM 访问权限](./media/azure-stack-marketplace-azure-items/cse.png) | [ 适用于 Linux 的 VM 访问权限 ](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/)| <b>请下载此更新，此更新针对适用于 Linux 的 VM 访问权限扩展的内置版本。如果计划使用 Debian Linux VM，此更新很重要。</b> | Microsoft | Linux |
| ![适用于 Linux 的 CloudLink SecureVM 扩展](./media/azure-stack-marketplace-azure-items/cloudlink.png) | [ 适用于 Linux 的 CloudLink SecureVM 扩展 ](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/cloudlink.cloudlink-securevm)  | 轻松可靠地控制、监视和加密 VM。 | Dell EMC | Linux |
| ![适用于 Windows 的 CloudLink SecureVM 扩展](./media/azure-stack-marketplace-azure-items/cloudlink.png) | [ 适用于 Windows 的 CloudLink SecureVM 扩展 ](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/cloudlink.cloudlink-securevm)  | 轻松可靠地控制、监视和加密 VM。 | Dell EMC | Windows |

## <a name="microsoft-virtual-machine-images-and-solution-templates"></a>Microsoft 虚拟机映像和解决方案模板

Azure Stack 支持下述 Azure Marketplace 虚拟机和解决方案模板。 请根据说明单独下载任何依赖项。 SQL Server 和 Machine Learning Server 之类的应用程序需要适当的许可，除非已标记为“免费”或“试用”。

|  | 项名称 | 说明 | 发布者 |
| --- | --- | --- | --- |
| ![Windows Server](./media/azure-stack-marketplace-azure-items/windowsserver.png) | [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer) | 企业级解决方案，部署简单，经济高效，以应用程序和用户为中心。 这些映像会定期更新，发布最新修补程序。 <b>重要信息：必须删除 2018 年 1 月 18 日之前下载的映像并将其替换为最新版本。</b> | Microsoft |
| ![远程桌面服务](./media/azure-stack-marketplace-azure-items/remotedesktopservicesdeployment.png) | [远程桌面服务 (RDS) 部署](https://azuremarketplace.microsoft.com/marketplace/apps/rds.remote-desktop-services-basic-deployment) | 创建基本的远程桌面服务 (RDS) 部署。 <b>下载最新的 Windows Server 2016 Datacenter 映像。</b> | Microsoft |
| ![SharePoint Server 2013 试用版](./media/azure-stack-marketplace-azure-items/sharepoint.png) | [SharePoint Server 2013 试用版](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SharePointServer2013Trial) | Windows Server 2012 Datacenter 和 Visual Studio 2017 社区版上的 Microsoft SharePoint Server 2013 试用版。 | Microsoft |
| ![SharePoint Server 2016 试用版](./media/azure-stack-marketplace-azure-items/sharepoint.png) | [SharePoint Server 2016 试用版](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SharePointServer2016Trial) | Windows Server 2016 Datacenter 上的 Microsoft SharePoint Server 2016 试用版。 | Microsoft |
| ![Windows Server 2012 R2 上的 SQL Server 2014 SP2](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2012 R2 上的 SQL Server 2014 SP2](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQL2014SP2-WS2012R2) | SQL Server 2014 Service Pack 2。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Standard](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Standard](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQL2016SP1-WS2016) | 适用于任务关键型智能应用程序的数据库平台。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Developer](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Developer](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016) | SQL Server 2016 SP1 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Express](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Express](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016) | SQL Server 2016 SP1 的免费 Express 版本。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Enterprise](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQL2016SP1-WS2016) | 适用于任务关键型智能应用程序的数据库平台。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Web](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Web](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQL2016SP1-WS2016) | 适用于任务关键型智能应用程序的数据库平台。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2017 Standard](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Standard](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQLServer2017StandardonWindowsServer2016) | 适用于任务关键型智能应用程序的数据库平台。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2017 Developer](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Developer](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016) | SQL Server 2016 SP1 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2017 Express](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Express](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) | SQL Server 2016 SP1 的免费 Express 版本。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2017 Enterprise](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQLServer2017EnterpriseWindowsServer2016) | 适用于任务关键型智能应用程序的数据库平台。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![](./media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Web](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQLServer2017WebonWindowsServer2016) | 适用于任务关键型智能应用程序的数据库平台。 **必需下载项：** SQL IaaS 扩展。 | Microsoft |
| ![Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Standard](./media/azure-stack-marketplace-azure-items/sql.png) | [Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Standard](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS) | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + Canonical |
| ![Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Developer](./media/azure-stack-marketplace-azure-items/sql.png) | [Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Developer](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) | SQL Server 2016 SP1 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 | Microsoft + Canonical |
| ![Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Express](./media/azure-stack-marketplace-azure-items/sql.png) | [Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Express](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) | SQL Server 2016 SP1 的免费 Express 版本。 | Microsoft + Canonical |
| ![Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Enterprise](./media/azure-stack-marketplace-azure-items/sql.png) | Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Enterprise | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + Canonical |
| ![Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Web](./media/azure-stack-marketplace-azure-items/sql.png) | [Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Web](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SQLServer2017WebonUbuntuServer1604LTS) | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + Canonical |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Standard](./media/azure-stack-marketplace-azure-items/sql.png) | SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Standard | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + SUSE |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Developer](./media/azure-stack-marketplace-azure-items/sql.png) | SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Developer | SQL Server 2016 SP1 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 | Microsoft + SUSE |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Express](./media/azure-stack-marketplace-azure-items/sql.png) | [SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Express](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) | SQL Server 2016 SP1 的免费 Express 版本。 | Microsoft + SUSE |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Enterprise](./media/azure-stack-marketplace-azure-items/sql.png) | SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Enterprise | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + SUSE |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Web](./media/azure-stack-marketplace-azure-items/sql.png) | SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Web| 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + SUSE |
| ![Windows Server 2016 上的 Microsoft Machine Learning Server 9.3.0](./media/azure-stack-marketplace-azure-items/microsoft.png) | [Windows Server 2016 上的 Microsoft Machine Learning Server 9.3.0](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.MicrosoftMachineLearningServer930onWindowsServer2016) | Windows Server 2016 上的 Microsoft Machine Learning Server 9.3.0。 | Microsoft |
| ![Ubuntu 16.04 上的 Microsoft Machine Learning Server 9.3.0](./media/azure-stack-marketplace-azure-items/microsoft.png) | [Ubuntu 16.04 上的 Microsoft Machine Learning Server 9.3.0](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.MicrosoftMachineLearningServer930onUbuntu1604) | Ubuntu 16.04 上的 Microsoft Machine Learning Server 9.3.0。 | Microsoft + Canonical |
| ![CentOS Linux 7.2 上的 Microsoft Machine Learning Server 9.3.0](./media/azure-stack-marketplace-azure-items/microsoft.png) | [CentOS Linux 7.2 上的 Microsoft Machine Learning Server 9.3.0](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.MicrosoftMachineLearningServer930onCentOSLinux72) | CentOS Linux 7.2 上的 Microsoft Machine Learning Server 9.3.0。 | Microsoft + Rogue Wave |


## <a name="linux-distributions"></a>Linux 发行版
|  | 项名称 | 说明 | 发布者 |
| --- | --- | --- | --- |
| ![Ubuntu Server](./media/azure-stack-marketplace-azure-items/ubuntu.png) | [Ubuntu Server](https://azuremarketplace.microsoft.com/marketplace/apps/Canonical.UbuntuServer) | Ubuntu Server 是全球流行的 Linux 云环境。 | Canonical |
| ![SLES 12 SP3 (BYOS)](./media/azure-stack-marketplace-azure-items/suse.png) | SLES 12 SP3 (BYOS) | SUSE Linux Enterprise Server 12 SP3。 | SUSE |

## <a name="third-party-byol-free-and-trial-images-and-solution-templates"></a>第三方 BYOL 版、免费版和试用版映像以及解决方案模板

|  | 项名称 | 说明 | 发布者 |
| --- | --- | --- | --- |
| ![SUSE Manager 3.0 Proxy (BYOS)](./media/azure-stack-marketplace-azure-items/suse.png) | SUSE Manager 3.0 Proxy (BYOS) | 同类最佳开源基础结构管理。 | SUSE |


<!-- Update_Description: update metedata properties -->