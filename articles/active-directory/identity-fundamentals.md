---
title: Azure 标识管理基础知识 | Microsoft Docs
description: 基于云的标识现已成为控制和洞察用户如何以及何时访问企业应用程序与数据的最佳方式。
keywords: ''
author: jeffgilb
manager: femila
origin.date: 07/05/2017
ms.topic: article
ms.prod: ''
ms.service: azure
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 39cf0277a0eb27ccaed95cadc6d9251b145a4865
ms.sourcegitcommit: ba39acbdf4f7c9829d1b0595f4f7abbedaa7de7d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2018
ms.locfileid: "29993172"
---
# <a name="fundamentals-of-azure-identity-management"></a>Azure 标识管理基础知识

随着越来越多的公司数字资源驻留在企业网络外部的云中和设备上，采用优越的基于云的标识和访问管理解决方案变得很有必要。 基于云的标识现已成为控制和洞察用户如何以及何时访问企业应用程序与数据的最佳方式。

十多年来，Microsoft 一直在保护基于云的标识的安全，而现在则通过 [Azure Active Directory (AD)](active-directory-editions.md) 提供相同的保护系统。 有了 Azure AD，企业管理员就可以轻松地确认用户和管理员的责任，提供比以前更好的安全性和监管。

Azure AD Premium 是基于云的标识和访问管理解决方案，提供的高级保护功能支持对所有应用使用一个安全标识、支持标识保护（通过 [Microsoft Intelligence Security Graph](https://www.microsoft.com/en-us/security/intelligence) 增强），以及支持 Privileged Identity Management。 Azure AD Premium 不只是另一个监视或报告工具，它还能实时保护用户的标识，允许创建基于风险的自适应访问策略来保护组织的数据。

Microsoft 不仅提供各处通用的标识，而且提供一套可以在组织中实现 IT 自动化、确保 IT 安全性以及进行 IT 管理的工具。 即使在出现云计算以后，也仍然需要管理和控制各种 IT 任务，例如通过呼叫支持人员来重置用户密码、进行用户组管理，以及提出应用程序请求。 让情况更为复杂的是，员工现在会将其个人设备带到工作中，并且会使用随时可用的 SaaS 应用程序。 这就使得跨公司数据中心和公有云平台维持对应用程序的控制极具挑战性。


## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>通过自助服务和单一登录体验提高工作效率，降低支持人员成本

如果只需记住单一用户名和密码且所有设备的体验是一致的，则员工的工作效率更高。 另外，如果能够执行自助任务，例如，如果能够[重置遗忘的密码](active-directory-passwords.md)或请求访问某个应用程序而不需等待支持人员的帮助，则还可节省时间。

Azure AD [将本地 Active Directory 扩展](connect/active-directory-aadconnect.md)到云，让用户不仅能够将主要组织帐户用于已加入域的设备和公司资源，而且还能用于完成作业所需的全部 Web 和 SaaS 应用程序。 用户除了无需记忆多组用户名和密码，还可根据其组织的组成员身份和身为员工的状态，自动预配（或取消预配）其应用程序访问权限。 


## <a name="benefits-of-azure-identity"></a>Azure 标识的优点

使用 Azure 标识管理可以执行以下操作：

-   为整个企业中的每个用户创建和管理单一标识，从而通过 [Azure Active Directory Connect](connect/active-directory-aadconnect.md) 保持用户、组和设备的同步。


-   使用 [self-service password reset](active-directory-passwords.md) 提高用户工作效率。

-   充分利用基于云的世界性企业级标识和访问管理解决方案的[高可用性和可靠性](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications)。

