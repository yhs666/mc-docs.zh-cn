---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 09/19/2019
ms.date: 11/11/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 4fecf4a27b5b7ee369437bf2fddf9c31414000a2
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655490"
---
<!--Verified successfully-->
本文包含适用于 Azure 虚拟机的安全建议。 请遵循这些建议来履行我们责任分担模型中所述的安全义务。 这些建议还有助于改善 Web 应用解决方案的整体安全性。 若要详细了解 Azure 采取哪些措施来履行服务提供商责任，请参阅[云计算的分担责任](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)。

在 Azure 安全中心可以自动实施本文所述的某些建议。 在保护 Azure 中的资源方面，Azure 安全中心是第一道防线。 它定期分析 Azure 资源的安全状态，以识别潜在的安全漏洞。 然后，它会建议如何解决漏洞。 有关详细信息，请参阅 [Azure 安全中心的安全建议](../articles/security-center/security-center-recommendations.md)。

<!--Not Available on [What is Azure Security Center?](../articles/security-center/security-center-intro.md)-->

## <a name="recommendations"></a>建议

| Category | 建议 | 注释 | 安全中心 |
|-|-|----|--|
| 常规 | 生成自定义 VM 映像时，请应用最新的更新。 | 在创建映像之前，为操作系统以及要包含在映像中的所有应用程序安装最新更新。  | - |
| 常规 | 备份 VM。 | [Azure 备份](../articles/backup/backup-overview.md)可帮助保护应用程序数据，其运行开销极低。 应用程序错误可能会损坏数据，人为错误可能会将 bug 引入应用程序。 Azure 备份可以保护运行 Windows 和 Linux 的 VM。 | - |
| 常规 | 使用多个 VM 来提高复原能力和可用性。 | 如果 VM 运行的应用程序必须高度可用，请使用多个 VM 或[可用性集](../articles/virtual-machines/windows/manage-availability.md)。 | - |
| 常规 | 采用业务连续性和灾难恢复 (BCDR) 策略。 | 使用 Azure Site Recovery 可以从支持业务连续性的不同选项中进行选择。 它支持不同的复制和故障转移方案。 有关详细信息，请参阅[关于 Site Recovery](../articles/site-recovery/site-recovery-overview.md)。 | - |
| 标识和访问管理 | 集中进行 VM 身份验证。 | 可以使用 [Azure Active Directory 身份验证](../articles/active-directory/develop/authentication-scenarios.md)集中进行 Windows 和 Linux VM 的身份验证。 | - |
| 数据安全性 | 加密操作系统磁盘。 | [Azure 磁盘加密](../articles/security/azure-security-disk-encryption-overview.md)可帮助你加密 Windows 和 Linux IaaS VM 磁盘。 在没有所需密钥的情况下，无法读取已加密磁盘的内容。 磁盘加密可以防范有人未经授权访问存储的数据，否则他们可能会复制磁盘。|  |
| 数据安全性 | 加密数据磁盘。 | [Azure 磁盘加密](../articles/security/azure-security-disk-encryption-overview.md)可帮助你加密 Windows 和 Linux IaaS VM 磁盘。 在没有所需密钥的情况下，无法读取已加密磁盘的内容。 磁盘加密可以防范有人未经授权访问存储的数据，否则他们可能会复制磁盘。| -  |
| 数据安全性 | 限制安装的软件。 | 将安装的软件限制为成功应用解决方案所需的软件。 此准则原则有助于减小解决方案的受攻击面。 | - |
| 数据安全性 | 使用防病毒软件或反恶意软件。 | 在 Azure 中，可以使用安全供应商（例如 Microsoft、Symantec、Trend Micro 和 Kaspersky）提供的反恶意软件。 这些软件可帮助保护 VM 免受恶意文件、广告程序和其他威胁的侵害。 可以根据应用程序工作负荷部署 Microsoft Antimalware。 使用默认的基本安全性或高级自定义配置。 有关详细信息，请参阅[适用于 Azure 云服务和虚拟机的 Microsoft Antimalware](../articles/security/azure-security-antimalware.md)。 | - |
| 数据安全性 | 安全存储密钥和机密。 | 为应用程序所有者提供安全的集中管理选项来简化机密和密钥的管理。 这种管理可以减少意外泄密或透露的风险。 | - |
| 网络 | 限制对管理端口的访问。 | 攻击者可能会利用猜出的常用密码和已知的未修补漏洞，扫描公有云 IP 范围中的开放管理端口，然后试图发起“轻而易举”的攻击。 可以使用[实时 (JIT) VM 访问](../articles/security-center/security-center-just-in-time.md)来锁定发往 Azure VM 的入站流量，降低遭受攻击的可能性，同时在需要时提供与 VM 的连接。 | - |
| 网络 | 限制网络访问。 | 可以通过网络安全组限制网络访问并控制公开的终结点数。 有关详细信息，请参阅[创建、更改或删除网络安全组](../articles/virtual-network/manage-network-security-group.md)。 | - |
| 监视 | 监视 VM。 | 可以使用用于 VM 的 Azure Monitor 来监视 Azure VM 和虚拟机规模集的状态。 VM 性能问题可能会导致服务中断，从而违反可用性安全原则。 | - |

<!--Not Available on Line 24 +1 |  [Update Management](../articles/automation/automation-update-management.md)-->
<!--Not Available on Line 24 +1 |  [Yes](../articles/security-center/security-center-apply-system-updates.md)-->

<!--Not Available on Line 36 +1 (../articles/azure-monitor/insights/vminsights-overview.md)-->
<!--MOONCAKE: Not Available on  Line 29 [Yes](../articles/security-center/security-center-apply-disk-encryption.md)-->
<!--MOONCAKE: LINE 33 CORRECT ON Microsoft Antimalware -->

<!--MOONCAKE: Line 33  Not Available on  Azure Key Vault can securely store your keys in hardware security modules (HSMs) that are certified to FIPS 140-2 Level 2. If you need to use FIPs 140.2 Level 3 to store your keys and secrets, you can use [Azure Dedicated HSM](../articles/dedicated-hsm/overview.md)-->

<!--Not Available on ## Next steps-->
<!--Not Available on [Secure-development documentation](../articles/security/fundamentals/abstract-develop-secure-apps.md)-->

<!--Update_Description: new articles on include virtual machines security recommendations -->
<!--New.date: 11/04/2019-->