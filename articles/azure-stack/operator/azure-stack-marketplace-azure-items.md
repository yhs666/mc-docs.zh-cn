---
title: 适用于 Azure Stack 的 Azure 市场项 | Microsoft Docs
description: 这些 Azure 市场项可以用在 Azure Stack 中。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/09/2019
ms.date: 09/16/2019
ms.author: v-jay
ms.reviewer: ihcherie
ms.lastreviewed: 08/09/2019
ms.openlocfilehash: 34bf49cef98fe67da03ba6d8358a13fdc3cc2c6b
ms.sourcegitcommit: 843028f54c4d75eba720ac8874562ab2250d5f4d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70856971"
---
# <a name="azure-marketplace-items-available-for-azure-stack"></a>适用于 Azure Stack 的 Azure 市场项

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包。*

## <a name="virtual-machine-extensions"></a>虚拟机扩展

只要有所用虚拟机 (VM) 扩展的更新，就应下载它们。 随产品一起提供的扩展不会在普通的修补和更新过程中更新，因此请经常查看更新。 其他扩展只能通过市场管理获取。

|  | 项名称 | 说明 | 发布者 | OS 类型 |
| --- | --- | --- | --- | --- |
| ![SQL IaaS 扩展](media/azure-stack-marketplace-azure-items/cse.png) | [SQL IaaS 扩展](/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)| **下载此扩展以部署任何“Windows 上的 SQL Server”市场项 - 此扩展是必需的。** | Microsoft | Windows |
| ![自定义脚本扩展](media/azure-stack-marketplace-azure-items/cse.png) | [自定义脚本扩展](/virtual-machines/windows/extensions-customscript)| **请下载此更新，此更新针对用于 Windows 的自定义脚本扩展的内置版本。** | Microsoft | Windows |
| ![PowerShell DSC 扩展](media/azure-stack-marketplace-azure-items/dsc.png) | [PowerShell DSC 扩展](/virtual-machines/windows/extensions-dsc-overview)| **请将此更新下载到 PowerShell DSC 扩展的内置版本。更新为支持 TLS v1.2。** | Microsoft | Windows |
| ![Microsoft 反恶意软件扩展](media/azure-stack-marketplace-azure-items/cse.png) | [Microsoft 反恶意软件扩展](/security/azure-security-antimalware)| 适用于 Azure 的 Microsoft 反恶意软件是一个针对应用程序和租户环境所提供的单一代理解决方案，可在后台运行而无需人工干预。 **请将此更新下载到 Antimalware 扩展的内置版本。** | Microsoft | Windows |
| ![Azure 诊断扩展](media/azure-stack-marketplace-azure-items/cse.png) | [Azure 诊断扩展](/virtual-machines/extensions/diagnostics-windows)| Azure 诊断是 Azure 中可对部署的应用程序启用诊断数据收集的功能。 **请下载此更新，此更新针对用于 Windows 的诊断扩展的内置版本。** | Microsoft | Windows |
| ![“Azure Monitor 更新和配置管理”扩展](media/azure-stack-marketplace-azure-items/cse.png) | [“Azure Monitor 更新和配置管理”扩展](/virtual-machines/extensions/oms-windows)| “Azure Monitor 更新和配置管理”扩展将与 Log Analytics 一起使用，以提供虚拟机监视功能。 **请下载此更新，此更新针对用于 Windows 的 Monitoring Agent 扩展的内置版本。** | Microsoft | Windows |
| ![自定义脚本扩展](media/azure-stack-marketplace-azure-items/cse.png) | - [自定义脚本扩展（版本 1，已弃用）](/virtual-machines/extensions/custom-script-linuxostc)</b> - [自定义脚本扩展（版本 2）](/virtual-machines/extensions/custom-script-linux) |**请将此更新下载到 Linux 的自定义脚本扩展的内置版本。此扩展有多个版本，应下载 1.5.2.1 和 2.0.x。** | Microsoft | Linux |
| ![适用于 Linux 的 VM 访问权限](media/azure-stack-marketplace-azure-items/cse.png) | [适用于 Linux 的 VM 访问权限](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/)| **请下载此更新，此更新针对适用于 Linux 的 VM 访问权限扩展的内置版本。如果计划使用 Debian Linux VM，此更新很重要。** | Microsoft | Linux |

## <a name="virtual-machine-images-and-solution-templates"></a>虚拟机映像和解决方案模板

Azure Stack 支持下述 Azure 市场虚拟机和解决方案模板。 请根据说明单独下载任何依赖项。 SQL Server 和 Machine Learning Server 之类的应用程序需要适当的许可，除非已标记为“免费”或“试用”。

|  | 项名称 | 说明 | 发布者 |
| --- | --- | --- | --- |
| ![Windows Server](media/azure-stack-marketplace-azure-items/windowsserver.png) | [Windows Server](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.WindowsServer?tab=Overview) | 企业级解决方案，部署简单，经济高效，以应用程序和用户为中心。 这些映像会定期更新，发布最新修补程序。 **重要信息：必须删除 2018 年 1 月 18 日之前下载的映像并将其替换为最新版本。** | Microsoft |
| ![Windows Server 2012 R2 上的 SQL Server 2014 SP2](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2012 R2 上的 SQL Server 2014 SP2](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2014sp2-ws2012r2?tab=Overview) | SQL Server 2014 Service Pack 2。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![免费许可证：Windows Server 2016 上的 SQL Server 2016 SP1 Developer](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：Windows Server 2016 上的 SQL Server 2016 SP1 Developer](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp1-ws2016?tab=Overview) | SQL Server 2016 SP1 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![免费许可证：Windows Server 2016 上的 SQL Server 2016 SP1 Express](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：Windows Server 2016 上的 SQL Server 2016 SP1 Express](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp1-ws2016?tab=PlansAndPrice) | SQL Server 2016 SP1 的免费 Express 版本。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Standard](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Standard](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp1-ws2016?tab=PlansAndPrice) | 适用于任务关键型智能应用程序的数据库平台。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP1 Enterprise](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP1 Enterprise](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp1-ws2016?tab=PlansAndPrice) | 适用于任务关键型智能应用程序的数据库平台。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![免费许可证：Windows Server 2016 上的 SQL Server 2016 SP2 Developer](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：Windows Server 2016 上的 SQL Server 2016 SP2 Developer](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp2-ws2016?tab=PlansAndPrice) | SQL Server 2016 SP2 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![免费许可证：Windows Server 2016 上的 SQL Server 2016 SP2 Express](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：Windows Server 2016 上的 SQL Server 2016 SP2 Express](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp2-ws2016?tab=PlansAndPrice) | SQL Server 2016 SP2 的免费 Express 版本。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP2 Standard](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP2 Standard](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp2-ws2016?tab=PlansAndPrice) | 适用于任务关键型智能应用程序的数据库平台。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2016 SP2 Enterprise](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2016 SP2 Enterprise](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2016sp2-ws2016?tab=PlansAndPrice) | 适用于任务关键型智能应用程序的数据库平台。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![免费许可证：Windows Server 2016 上的 SQL Server 2017 Developer](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：Windows Server 2016 上的 SQL Server 2017 Developer](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2017-ws2016?tab=Overview) | SQL Server 2017 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![免费许可证：Windows Server 2016 上的 SQL Server 2017 Express](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：Windows Server 2016 上的 SQL Server 2017 Express](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2017-ws2016?tab=Overview) | SQL Server 2017 的免费 Express 版本。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2017 Standard](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Standard](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2017-ws2016?tab=PlansAndPrice) | 适用于任务关键型智能应用程序的数据库平台。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Windows Server 2016 上的 SQL Server 2017 Enterprise](media/azure-stack-marketplace-azure-items/sql.png) | [Windows Server 2016 上的 SQL Server 2017 Enterprise](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2017-ws2016?tab=PlansAndPrice) | 适用于任务关键型智能应用程序的数据库平台。 **所需的下载：** SQL IaaS 扩展。 | Microsoft |
| ![Ubuntu Server 16.04 LTS 上的 SQL Server 2017](media/azure-stack-marketplace-azure-items/sql.png) | [Ubuntu Server 16.04 LTS 上的 SQL Server 2017 Enterprise](https://market.azure.cn/zh-cn/marketplace/apps/microsoftsqlserver.sql2017-ubuntu1604?tab=Overview) | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + Canonical |
| ![免费许可证：SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Developer](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Developer](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.SQLServer2017Ubuntu?tab=Overview) | SQL Server 2017 的免费开发人员版，适用于事务处理、数据仓库、商业智能和分析型工作负荷。 | Microsoft + SUSE |
| ![免费许可证：SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Express](media/azure-stack-marketplace-azure-items/sql.png) | [免费许可证：SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Express](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.SQLServer2017Ubuntu?tab=Overview) | SQL Server 2017 的免费 Express 版本。 | Microsoft + SUSE |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Enterprise](media/azure-stack-marketplace-azure-items/sql.png) | [SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Enterprise](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.SQLServer2017Ubuntu?tab=Overview) | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + SUSE |
| ![SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Web](media/azure-stack-marketplace-azure-items/sql.png) | [SUSE Linux Enterprise Server (SLES) 12 SP2 上的 SQL Server 2017 Web](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.SQLServer2017Ubuntu?tab=Overview) | 适用于任务关键型智能应用程序的数据库平台。 | Microsoft + SUSE |
| ![Windows Server 2016 上的 Microsoft Machine Learning Server 9.3.0](media/azure-stack-marketplace-azure-items/microsoft.png) | [Windows Server 2016 上的 Microsoft Machine Learning Server 9.3.0](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.MicrosoftMachineLearningServer930onWindowsServer2016?tab=Overview) | Windows Server 2016 上的 Microsoft Machine Learning Server 9.3.0。 | Microsoft |
| ![Ubuntu 16.04 上的 Microsoft Machine Learning Server 9.3.0](media/azure-stack-marketplace-azure-items/microsoft.png) | [Ubuntu 16.04 上的 Microsoft Machine Learning Server 9.3.0](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.MicrosoftMachineLearningServer930onUbuntu1604?tab=Overview) | Ubuntu 16.04 上的 Microsoft Machine Learning Server 9.3.0。 | Microsoft + Canonical |
| ![CentOS Linux 7.2 上的 Microsoft Machine Learning Server 9.3.0](media/azure-stack-marketplace-azure-items/microsoft.png) | [CentOS Linux 7.2 上的 Microsoft Machine Learning Server 9.3.0](https://market.azure.cn/zh-cn/marketplace/apps/Microsoft.MicrosoftMachineLearningServer930onCentOSLinux72?tab=Overview) | CentOS Linux 7.2 上的 Microsoft Machine Learning Server 9.3.0。 | Microsoft + Rogue Wave |

## <a name="linux-distributions"></a>Linux 发行版

|  | 项名称 | 说明 | 发布者 |
| --- | --- | --- | --- |
| ![Clear Linux OS](media/azure-stack-marketplace-azure-items/clearlinux.png) | [Clear Linux OS](https://market.azure.cn/zh-cn/marketplace/apps/CoreOS.CoreOS?tab=Overview) | 一个针对 Intel 体系结构优化的参考性 Linux 发行版 | Clear Linux Project |
| ![CoreOS 提供的 Container Linux](media/azure-stack-marketplace-azure-items/coreos.png) | [CoreOS 提供的 Container Linux](https://market.azure.cn/zh-cn/marketplace/apps/CoreOS.CoreOS?tab=Overview) | Container Linux 是一种新式的最小型 Linux 发行版，可以方便地运行容器、管理群集以及无缝地更新服务器 - 所有组件都启用了仓库规模的计算。 | CoreOS |
| ![Ubuntu Server](media/azure-stack-marketplace-azure-items/ubuntu.png) | [Ubuntu Server](https://market.azure.cn/zh-cn/marketplace/apps/Canonical.UbuntuServer?tab=Overview) | Ubuntu Server 是全球流行的 Linux 云环境。 | Canonical |
| ![Debian 8 "Jessie"](media/azure-stack-marketplace-azure-items/debian8.png) | [Debian 8“Jessie”](https://market.azure.cn/zh-cn/marketplace/apps/credativ.Debian?tab=Overview) | Debian GNU/Linux 是最流行的 Linux 分发版之一。 | credativ |
| ![基于 CentOS 的 6.8](media/azure-stack-marketplace-azure-items/roguewave.png) | [基于 CentOS 的 6.8](https://market.azure.cn/zh-cn/marketplace/apps/RogueWave.CentOSbased?tab=Overview) | 此 Linux 发行版基于 CentOS ，由 Rogue Wave Software 提供。 | Rogue Wave Software（之前为 OpenLogic）  |
| ![基于 CentOS 的 6.10](media/azure-stack-marketplace-azure-items/roguewave.png) | [基于 CentOS 的 6.10](https://market.azure.cn/zh-cn/marketplace/apps/RogueWave.CentOSbased?tab=Overview) | 此 Linux 发行版基于 CentOS ，由 Rogue Wave Software 提供。 | Rogue Wave Software（之前为 OpenLogic）  |
| ![基于 CentOS 的 7.3](media/azure-stack-marketplace-azure-items/roguewave.png) | [基于 CentOS 的 7.3](https://market.azure.cn/zh-cn/marketplace/apps/RogueWave.CentOSbased?tab=Overview) | 此 Linux 发行版基于 CentOS ，由 Rogue Wave Software 提供。 | Rogue Wave Software（之前为 OpenLogic） |
| ![基于 CentOS 的 7.5](media/azure-stack-marketplace-azure-items/roguewave.png) | [基于 CentOS 的 7.5](https://market.azure.cn/zh-cn/marketplace/apps/RogueWave.CentOSbased?tab=Overview) | 此 Linux 发行版基于 CentOS ，由 Rogue Wave Software 提供。 | Rogue Wave Software（之前为 OpenLogic） |
| ![基于 CentOS 的 7.5-LVM](media/azure-stack-marketplace-azure-items/roguewave.png) | [基于 CentOS 的 7.5-LVM](https://market.azure.cn/zh-cn/marketplace/apps/RogueWave.CentOSbased?tab=PlansAndPrice) | 此 Linux 发行版基于 CentOS ，由 Rogue Wave Software 提供。 | Rogue Wave Software（之前为 OpenLogic） |
| ![SLES 11 SP4 (BYOS)](media/azure-stack-marketplace-azure-items/suse.png) | [SLES 11 SP4 (BYOS)](https://market.azure.cn/zh-cn/marketplace/apps/suse.sles-byos?tab=Overview) | SUSE Linux Enterprise Server 11 SP4。 | SUSE |
| ![SLES 12 SP3 (BYOS)](media/azure-stack-marketplace-azure-items/suse.png) | [SLES 12 SP4 (BYOS)](https://market.azure.cn/zh-cn/marketplace/apps/suse.sles-byos?tab=Overview) | SUSE Linux Enterprise Server 12 SP4。 | SUSE |
| ![SLES 15 (BYOS)](media/azure-stack-marketplace-azure-items/suse.png) | [SLES 15 (BYOS)](https://market.azure.cn/zh-cn/marketplace/apps/suse.sles-byos?tab=Overview) | SUSE Linux Enterprise Server 15。 | SUSE |

## <a name="third-party-byol-free-trial-images-and-solution-templates"></a>第三方 BYOL 版、免费版和试用版映像以及解决方案模板

|  | 项名称 | 说明 | 发布者 |
| --- | --- | --- | --- |
| ![A10 vThunder ADC](media/azure-stack-marketplace-azure-items/a10.png) | [A10 vThunder ADC](https://market.azure.cn/zh-cn/marketplace/apps/a10networks-cn.a10-thunder-adc-411-p2?tab=PlansAndPrice) | 适用于 Azure 的 A10 Networks vThunder 应用交付控制器专为实现高性能、灵活性和易于部署的应用程序交付和服务器负载均衡而构建，并经过优化以在 Azure 云中本机运行。 | A10 Networks |
| ![Service Fabric 群集](media/azure-stack-marketplace-azure-items/servicefrabric.png) | [Service Fabric 群集](https://azuremarketplace.microsoft.com/marketplace/apps/bitnami.erpnext) | 此解决方案在虚拟机规模集上部署作为独立群集运行的 Service Fabric。 <br>**此解决方案模板还需要下载 Windows Server 2016 Datacenter**| Microsoft |
| ![SUSE Manager 3.1 Proxy (BYOS)](media/azure-stack-marketplace-azure-items/suse.png) | [SUSE Manager 3.1 Proxy (BYOS)](https://market.azure.cn/zh-cn/marketplace/apps/suse.suse-manager-proxy-byos?tab=Overview) | 同类最佳开源基础结构管理。 | SUSE |
