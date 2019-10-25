---
title: Azure Stack 1908 发行说明 | Microsoft Docs
description: 了解 Azure Stack 集成系统的 1908 更新，包括新增功能、已知问题和更新下载位置。
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
origin.date: 09/05/2019
ms.date: 10/21/2019
ms.author: v-jay
ms.reviewer: prchint
ms.lastreviewed: 08/30/2019
monikerRange: azs-1908
ms.openlocfilehash: de660c5fff720a69e8c502147156975855aaa2ac
ms.sourcegitcommit: 713bd1d1b476cec5ed3a9a5615cfdb126bc585f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578578"
---
# <a name="azure-stack-1908-update"></a>Azure Stack 1908 更新

*适用于：Azure Stack 集成系统*

本文介绍 1908 更新包的内容。 此更新包括新的改进，以及此 Azure Stack 版本的修复。

> [!IMPORTANT]  
> 此更新包仅适用于 Azure Stack 集成系统。 请勿将此更新包应用于 Azure Stack 开发工具包。

## <a name="previous-release-notes"></a>以前的发行说明

从版本 1908 开始，以前的发行说明版本不再会显示在左侧的目录中。 若要访问以前的发行说明版本，请选择另一篇文章（例如 [Azure Stack 概述](azure-stack-overview.md)），然后在左侧目录顶部的版本选择器中选择 1905、1906、1907 或 1908。 有关以前的发行说明版本，请参阅[已存档的发行说明](#archived-release-notes)部分。

## <a name="build-reference"></a>内部版本参考

Azure Stack 1908 更新内部版本号为 **1.1908.0.20**。

### <a name="update-type"></a>更新类型

对于 1908，运行 Azure Stack 的底层操作系统已更新为 Windows Server 2019。 这可以实现核心基础增强，并在不久的将来将更多功能引入 Azure Stack。

Azure Stack 1908 更新内部版本类型为“完整”。  因此，1908 更新的运行时间比快速更新（例如 1906 和 1907）更久。 完整更新的确切运行时间通常取决于 Azure Stack 实例包含的节点数目、租户工作负荷在系统上使用的容量、系统的网络连接（如果已连接到 Internet），以及系统的硬件配置。 在我们的内部测试中，1908 更新的预期运行时间如下：4 个节点 - 42 小时，8 个节点 - 50 小时，12 个节点 - 60 小时，16 个节点 - 70 小时。 更新运行时间超过预期值并不常见，因此，除了更新失败之外，无需要求 Azure Stack 操作员执行操作。

有关更新内部版本类型的详细信息，请参阅[在 Azure Stack 中管理更新](azure-stack-updates.md)。

- 确切的更新运行时间通常取决于租户工作负荷在系统上使用的容量、系统网络连接（如果已连接到 Internet），以及系统的硬件配置。
- 运行时间超过预期并不常见，因此，除了更新失败之外，无需要求 Azure Stack 操作员执行操作。
- 此运行时近似值特定于 1908 更新，不应与其他 Azure Stack 更新进行比较。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新增功能

<!-- What's new, also net new experiences and features. -->

- 对于 1908，请注意，运行 Azure Stack 的底层操作系统已更新为 Windows Server 2019。 这可以实现核心基础增强，并在不久的将来将更多功能引入 Azure Stack。
- Azure Stack 基础结构的所有组件现在都以 FIPS 140-2 模式运行。
- Azure Stack 操作员现在可以删除门户用户数据。 有关详细信息，请参阅[从 Azure Stack 中清除门户用户数据](azure-stack-portal-clear.md)。

### <a name="improvements"></a>改进

<!-- Changes and product improvements with tangible customer-facing value. -->
- Azure Stack 的静态数据加密已得到改进，可将机密持久保存到物理节点的硬件受信任平台模块 (TPM)。

### <a name="changes"></a>更改

- 硬件提供商将在 Azure Stack 版本 1908 的同一发布时间发布 OEM 扩展包 2.1 或更高版本。 必须安装 OEM 扩展包 2.1 或更高版本才能使用 Azure Stack 版本 1908。 有关如何下载 OEM 扩展包 2.1 或更高版本的详细信息，请与系统的硬件提供商联系，并参阅 [OEM 更新](azure-stack-update-oem.md#oem-contact-information)一文。  

### <a name="fixes"></a>修复项

- 修复了与将来的 Azure Stack OEM 更新兼容的问题，以及使用客户用户映像进行 VM 部署的问题。 此问题是在 1907 中发现的，已在修补程序 [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) 中予以修复  
- 修复了 OEM 固件更新的问题，并更正了 Fabric Ring Health 的 Test-AzureStack 中的诊断错误。 此问题是在 1907 中发现的，已在修补程序 [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) 中予以修复
- 修复了 OEM 固件更新过程的问题。 此问题是在 1907 中发现的，已在修补程序 [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) 中予以修复


<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates"></a>安全更新

有关此 Azure Stack 更新中的安全更新的信息，请参阅 [Azure Stack 安全更新](azure-stack-release-notes-security-updates.md)。

## <a name="update-planning"></a>更新规划

应用更新之前，请务必查看以下信息：

- [已知问题](azure-stack-release-notes-known-issues-1908.md)
- [安全更新](azure-stack-release-notes-security-updates.md)
- [应用更新之前和之后的活动清单](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>下载更新

可从 [Azure Stack 下载页](https://aka.ms/azurestackupdatedownload)下载 Azure Stack 1908 更新包。

## <a name="hotfixes"></a>修补程序

Azure Stack 定期发布修补程序。 将 Azure Stack 更新到 1908 之前，请务必先安装 1907 的最新 Azure Stack 修补程序。

Azure Stack 修补程序仅适用于 Azure Stack 集成系统；请勿尝试在 ASDK 上安装修补程序。

### <a name="prerequisites-before-applying-the-1908-update"></a>先决条件：应用 1908 更新之前

必须在包含以下修补程序的版本 1907 中应用 Azure Stack 版本 1908：

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修补程序 1.1907.12.44](https://support.microsoft.com/help/4517473)

Azure Stack 1908 更新需要系统硬件提供商提供的 **Azure Stack OEM 2.1 或更高版本**。 OEM 更新包括 Azure Stack 系统硬件的驱动程序和固件更新。 有关应用 OEM 更新的详细信息，请参阅[应用 Azure Stack 原始设备制造商更新](azure-stack-update-oem.md)

### <a name="after-successfully-applying-the-1908-update"></a>成功应用 1908 更新之后

安装此更新之后，请安装所有适用的修补程序。 有关详细信息，请参阅我们的[服务策略](azure-stack-servicing-policy.md)。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- 1908 没有可用的修补程序。

## <a name="automatic-update-notifications"></a>自动更新通知

其系统可从基础结构网络访问 Internet 的客户在操作员门户中会看到“有可用的更新”消息。  无法访问 Internet 的系统可以下载并导入包含相应 .xml 的 .zip 文件。

> [!TIP]  
> 订阅下述 *RSS* 或 *Atom* 源，了解 Azure Stack 修补程序的最新信息：
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>已存档的发行说明

可查看 [TechNet 库中 Azure Stack 发行说明的早期版本](https://aka.ms/azsarchivedrelnotes)。 提供这些已存档的发行说明仅供参考，并不意味着支持这些版本。 有关 Azure Stack 支持的信息, 请参阅 [Azure Stack 服务策略](azure-stack-servicing-policy.md)。 如需进一步的帮助，请联系 Microsoft 客户支持服务。

## <a name="next-steps"></a>后续步骤

- 有关 Azure Stack 中更新管理的概述，请参阅[在 Azure Stack 中管理更新的概述](azure-stack-updates.md)。  
- 有关如何在 Azure Stack 中应用更新的详细信息，请参阅[在 Azure Stack 中应用更新](azure-stack-apply-updates.md)。
- 若要查看 Azure Stack 集成系统的服务策略，以及必须如何做才能使系统保持在受支持的状态，请参阅 [Azure Stack 服务策略](azure-stack-servicing-policy.md)。  
- 若要使用特权终结点 (PEP) 来监视和恢复更新，请参阅[使用特权终结点监视 Azure Stack 中的更新](azure-stack-monitor-update.md)。
