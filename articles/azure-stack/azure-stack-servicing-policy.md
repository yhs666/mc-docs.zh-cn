---
title: Azure Stack 服务策略 | Microsoft Docs
description: 了解 Azure Stack 服务策略，以及如何使集成系统保持在受支持的状态。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/03/2018
ms.date: 04/08/2018
ms.author: v-junlch
ms.openlocfilehash: 8f96d56ac422ed12ea99b87f30bedc521a49f6af
ms.sourcegitcommit: ffb8b1527965bb93e96f3e325facb1570312db82
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2018
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack 服务策略
本文介绍 Azure Stack 集成系统的服务策略，以及必须如何做才能使系统保持在受支持的状态。 

## <a name="update-package-types"></a>更新包类型

集成系统有两种类型的的更新包：Microsoft 软件更新，以及特定于原始设备制造商 (OEM) 硬件供应商的更新，例如驱动程序和固件。 这些更新是以单独的 Azure Stack 更新包提供，而且独立管理。

- **Microsoft 软件更新**。 Microsoft 会负责 Microsoft 软件更新包的端到端服务生命周期。 这些包可以包括最新的 Windows Server 安全更新、非安全更新和 Azure Stack 功能更新。 可以直接从 Microsoft 下载这些更新包。
- **OEM 硬件供应商提供的更新**。 Azure Stack 硬件合作伙伴负责硬件相关固件和驱动程序更新包的端到端服务生命周期（包括指导）。 此外，对于硬件生命周期主机上的所有软件和硬件，Azure Stack 硬件合作伙伴拥有并维护指导。 OEM 硬件供应商在自己的下载站点上托管这些更新包。

## <a name="update-package-release-cadence"></a>更新包发布频率

Microsoft 预期每月发布软件更新包。 但是，可能一个月内发布多个更新或没有任何更新。 OEM 硬件供应商会根据需要发布更新。

Microsoft 更新包具有以下命名约定，可帮助你轻松识别发布日期：

*MajorProductVersion.MinorProductVersion.YYMMDD.BuildNumber*

例如，2017 年 6 月 15 日发布的 Microsoft 软件更新是版本“1.0.170615.1”。

## <a name="keep-your-system-under-support"></a>保持系统受支持
必须不断更新 Azure Stack 部署才能持续获得支持。 针对延迟更新的策略是，若要确保 Azure Stack 始终获得支持，必须运行最近发布的更新版本，或者运行以前发布的两个主要更新版本之一。  修补程序不属于主要更新版本。  如果缺少至少两个更新，Azure Stack 云会被视为不合规，必须至少更新到最低的受支持版本才能获得支持。 

例如，如果最新发布的更新版本为 1805，在此之前的两个更新包为版本 1804 和 1803，则 1803 和 1804 仍受支持， 但 1802 不受支持。 即使最近一到两个月没有发布任何版本，此策略也有效。 例如，如果最新版本为 1805，但没有版本 1804，则此前的两个更新包（1803 和 1802）仍可获得支持。

Microsoft 软件更新包是非累积性的，其先决条件是需要前一个更新包。 如果决定延后一个或多个更新，则要使用最新版本，请考虑整体运行时。 


## <a name="next-steps"></a>后续步骤

- [在 Azure Stack 中管理更新](azure-stack-updates.md)

<!-- Update_Description: wording update -->

