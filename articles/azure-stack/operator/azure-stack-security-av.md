---
title: 在 Azure Stack上更新 Windows Defender 防病毒
description: 有关防病毒功能如何在 Azure Stack 上保持最新的详细信息
services: azure-stack
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.topic: article
origin.date: 01/14/2019
ms.date: 02/18/2019
ms.author: v-jay
ms.reviewer: fiseraci
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: e8aebafbec3ff3317c9da0a94954450ebfbd6d04
ms.sourcegitcommit: 05aa4e4870839a3145c1a3835b88cf5279ea9b32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2019
ms.locfileid: "64529866"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>在 Azure Stack上更新 Windows Defender 防病毒

[Windows Defender 防病毒](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)是一种反恶意软件解决方案，可提供安全性和病毒防护功能。 每个 Azure Stack 基础结构组件（Hyper-V 主机和虚拟机）均受到 Windows Defender 防病毒的保护。 为了获得最新的保护，Windows Defender 防病毒定义、引擎和平台需要定期更新。 如何应用更新取决于配置。

## <a name="connected-scenario"></a>联网场景

为了获得反恶意软件定义和引擎更新，Azure Stack [更新资源提供程序](azure-stack-updates.md#the-update-resource-provider)每天都会多次下载反恶意软件定义和引擎更新。 每个 Azure Stack 基础结构组件都会从更新资源提供程序获取更新并自动应用更新。

为了获得反恶意软件平台更新，请应用[每月的 Azure Stack 更新](azure-stack-apply-updates.md)。 每月的 Azure Stack 更新包括该月的 Windows Defender 防病毒平台更新。

## <a name="disconnected-scenario"></a>离线场景

 应用[每月的 Azure Stack 更新](azure-stack-apply-updates.md)。 每月的 Azure Stack 更新包括该月的 Windows Defender 防病毒定义、引擎和平台更新。

## <a name="next-steps"></a>后续步骤

[详细了解 Azure Stack 安全性](azure-stack-security-foundations.md)
