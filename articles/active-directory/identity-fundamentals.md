---
title: "Azure 标识管理基础知识 | Microsoft Docs"
description: 
keywords: 
author: jeffgilb
manager: femila
ms.date: 3/28/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 78da854d58905bc82228bcbff1de0fcfbc12d5ac
ms.openlocfilehash: f837b002ef2f7f4648b76f6eb0550f09172bb197
ms.lasthandoff: 04/22/2017

---
# <a name="fundamentals-of-azure-identity-management"></a>Azure 标识管理基础知识
随着公司的数字资源逐渐转向公司网络外部、云中和设备上，为了对用户访问公司应用程序和数据的方式和时间进行控制和监视，最好的方式是使用卓越的基于云的标识和访问管理解决方案。

十多年来，Microsoft 一直在保护基于云的标识的安全，而现在则通过 [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions) 为你提供相同的保护系统。 有了 Azure AD，企业管理员就可以轻松地确认用户和管理员的责任，提供比以前更好的安全性和监管。

Azure AD Premium 是基于云的标识和访问管理解决方案，提供的高级保护功能支持对所有应用使用一个安全标识、支持标识保护（通过 [Microsoft Intelligence Security Graph](https://www.microsoft.com/en-us/security/intelligence) 增强），以及支持 Privileged Identity Management。 Azure AD Premium 不只是另一个监视或报告工具，它还能实时保护用户的标识，允许你创建基于风险的自适应访问策略来保护组织的数据。

Microsoft 不仅提供各处通用的标识，而且提供一套可以在组织中实现 IT 自动化、确保 IT 安全性以及进行 IT 管理的工具。 即使在出现云计算以后，也仍然需要管理和控制各种 IT 任务，例如通过呼叫支持人员来重置用户密码、进行用户组管理，以及提出应用程序请求。 让情况更为复杂的是，员工现在会将其个人设备带到工作中，并且会使用随时可用的 SaaS 应用程序。 这就使得跨公司数据中心和公有云平台维持对应用程序的控制极具挑战性。

> [!Note]
> 本文所述功能需要的 Azure Active Directory P1 或 P2 订阅可以单独购买，也可以作为[企业移动性 + 安全性 E3 或 E5](https://docs.microsoft.com/enterprise-mobility-security/solutions/learn-about-ems) 订阅的一部分来购买。

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>通过自助服务和单一登录体验提高工作效率，降低支持人员成本

如果只需记住单一用户名和密码且所有设备的体验是一致的，则员工的工作效率更高。 另外，如果能够执行自助任务，例如，如果能够[重置遗忘的密码](https://docs.microsoft.com/azure/active-directory/active-directory-passwords)或请求访问某个应用程序而不需等待支持人员的帮助，则还可节省时间。

Azure AD [将本地 Active Directory 扩展](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)到云，让用户不仅能够将主要组织帐户用于已加入域的设备和公司资源，而且还能用于完成作业所需的全部 Web 和 SaaS 应用程序。 用户除了无需记忆多组用户名和密码，还可根据其组织的组成员身份和身为员工的状态，自动预配（或取消预配）其应用程序访问权限。

## <a name="manage-and-control-access-to-corporate-resources"></a>管理和控制对公司资源的访问
Microsoft 标识和访问管理解决方案可帮助 IT 部门保护对企业数据中心和云中的应用程序和资源的访问，从而支持附加的验证级别，比如[多重身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)。

## <a name="benefits-of-azure-identity"></a>Azure 标识的优点

使用 Azure 标识管理可以执行以下操作：

-   为混合企业中的每个用户创建和管理单一标识，从而通过 [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) 保持用户、组和设备的同步。

-   通过对本地应用程序和云应用程序强制执行基于规则的[多重身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)，启用应用程序访问安全措施。

-   使用 [self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) 提高用户工作效率。

-   充分利用基于云的世界性企业级标识和访问管理解决方案的[高可用性和可靠性](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications)。


