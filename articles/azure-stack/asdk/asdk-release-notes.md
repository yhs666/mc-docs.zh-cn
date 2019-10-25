---
title: ASDK 发行说明 | Microsoft Docs
description: Azure Stack 开发工具包 (ASDK) 的改进、修复和已知问题。
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
origin.date: 08/30/2019
ms.date: 10/21/2019
ms.author: v-jay
ms.reviewer: misainat
ms.lastreviewed: 08/30/2019
ms.openlocfilehash: 2e0299a221ccba51cfac686abc1192baa08ad700
ms.sourcegitcommit: 713bd1d1b476cec5ed3a9a5615cfdb126bc585f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578509"
---
# <a name="asdk-release-notes"></a>ASDK 发行说明

本文介绍了 Azure Stack 开发工具包 (ASDK) 中的更改、修复和已知问题。 如果不确定所运行的版本，请[使用门户进行查看](../operator/azure-stack-updates.md)。

请订阅 [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS 源](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)，随时了解 ASDK 的新增功能。

::: moniker range="azs-1908"
## <a name="build-11908020"></a>内部版本 1.1908.0.20

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1908.md#whats-new)。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

<!-- - For a list of Azure Stack issues fixed in this release, see [this section](../operator/azure-stack-release-notes-1908.md#fixes) of the Azure Stack release notes. -->
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1908.md)。
- 请注意，可用的 Azure Stack 修补程序不适用于 ASDK。
::: moniker-end

::: moniker range="azs-1907"
## <a name="build-11907020"></a>内部版本 1.1907.0.20

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1907.md#whats-in-this-update)。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 使用某些市场映像创建 VM 资源时，可能无法完成部署。 作为一种解决方法，可以单击“摘要”  页中的“下载模板和参数”  链接，然后单击“模板”  边栏选项卡中的“部署”  按钮。
- 如需此版本中已修复的 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1907.md#fixes)。
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1907.md)。
- 请注意，[发布的 Azure Stack 修补程序](../operator/azure-stack-release-notes-1907.md#hotfixes)不适用于 Azure Stack ASDK。
::: moniker-end

::: moniker range="azs-1906"
## <a name="build-11906030"></a>内部版本 1.1906.0.30

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1906.md#whats-in-this-update)。

### <a name="changes"></a>更改

- 添加了 **AzS-SRNG01** 支持环 VM，用于托管 Azure Stack 的日志收集服务。 有关详细信息，请参阅[虚拟机角色](asdk-architecture.md)。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 使用某些市场映像创建 VM 资源时，可能无法完成部署。 作为一种解决方法，可以单击“摘要”  页中的“下载模板和参数”  链接，然后单击“模板”  边栏选项卡中的“部署”  按钮。
- 如需此版本中已修复的 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1906.md#fixes)。
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1906.md)。
- 请注意，[发布的 Azure Stack 修补程序](../operator/azure-stack-release-notes-1906.md#hotfixes)不适用于 Azure Stack ASDK。
::: moniker-end

::: moniker range="azs-1905"
## <a name="build-11905040"></a>内部版本 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1905.md#whats-in-this-update)。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 修复了必须编辑 **RegisterWithAzure.psm1** PowerShell 脚本才能成功[注册 ASDK](asdk-register.md) 的问题。
- 如需此版本中已修复的其他 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](../operator/azure-stack-release-notes-1905.md#fixes)。
- 如需已知问题的列表，请参阅[此文](../operator/azure-stack-release-notes-known-issues-1905.md)。
- 请注意，[发布的 Azure Stack 修补程序](../operator/azure-stack-release-notes-1905.md#hotfixes)不适用于 Azure Stack ASDK。
::: moniker-end
