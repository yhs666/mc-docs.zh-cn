---
title: 什么是 Azure Active Directory (Azure AD)？ | Microsoft Docs
description: 使用 Azure Active Directory 将现有的本地标识扩展到云中，或开发 Azure AD 集成的应用程序。
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: v-junlch
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
origin.date: 04/09/2018
ms.date: 06/25/2018
ms.custom: it-pro
ms.openlocfilehash: 6f205d5e98de9f0b8d5f70cd13b8400dd07fe365
ms.sourcegitcommit: 8b36b1e2464628fb8631b619a29a15288b710383
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2018
ms.locfileid: "36948666"
---
# <a name="what-is-azure-active-directory"></a>什么是 Azure Active Directory？
Azure Active Directory (Azure AD) 是 Microsoft 推出的基于云的多租户目录，也是标识管理服务，可以将核心目录服务、应用程序访问管理和标识保护组合到单个解决方案中。 Azure AD 还提供功能丰富、基于标准的平台，该平台支持开发人员根据集中的策略和规则为应用程序提供访问控制。

![Azure AD Connect 堆栈](./media/active-directory-whatis/Azure_Active_Directory.png)

- **面向应用开发人员。** Azure AD 可让你集成全球数百万个组织所用的标识管理解决方案，从而专注于构建应用。

- **面向 Office 365 客户和 Azure 客户。** 你已在使用 Azure AD。 每个 Office 365 租户和 Azure 租户实际上都是 Azure AD 租户，因此你可以立即开始管理员工对集成云应用的访问。

## <a name="how-reliable-is-azure-ad"></a>Azure AD 的可靠性如何？
Azure AD 的多租户、地理分布、高可用性设计意味着可以依赖它来解决最关键的业务需求。 在全球运转有 28 个可自动故障转移的数据中心，这让人能够体会到 Azure AD 的高度可靠，即使数据中心发生服务中断，目录数据也至少会在两个以上的地域分散的数据中心内保留副本，并且提供立即访问权限。

有关服务级别协议的详细信息，请参阅[服务级别协议](https://www.azure.cn/support/legal/sla/)。

## <a name="choose-an-edition"></a>选择版本
所有 Microsoft Online 业务服务都依赖于 Azure Active Directory (Azure AD) 进行登录及满足其他标识需求。 如果订阅了任何 Microsoft Online 业务服务（例如，Office 365 或 Azure），则已获得 Azure AD，能够访问所有免费功能。 使用 Azure Active Directory 免费版，可以管理用户和组、与本地目录同步，以及在 Azure、Office 365 和数千种主流 SaaS 应用程序（如 Salesforce、Workday、Concur、DocuSign、Box、ServiceNow、Dropbox 等）上单一登录。 

若要增强 Azure Active Directory，可以使用 Azure Active Directory 基本版、Premium P1 版和 Premium P2 版添加付费功能。 Azure Active Directory 付费版建立在现有免费目录基础之上，提供企业级功能，包括自助服务、增强型监视、安全报告、多重身份验证 (MFA) 和移动工作者安全访问。

> [!NOTE]
> 有关这些版本的定价选项，请参阅 [Azure Active Directory 定价](https://www.azure.cn/pricing/details/identity/)。 中国地区目前不支持 Azure Active Directory Premium P1 版、Premium P2 版和 Azure Active Directory 基本版。 有关 Azure AD 定价的详细信息，可与 Azure Active Directory 论坛联系。
>

## <a name="how-can-i-get-started"></a>如何开始？

**如果是 IT 管理员：**

- [立即试用！](/active-directory/) - 现在就可以使用此链接注册试用版，并在不到五分钟内部署第一个云解决方案



            **如果是开发人员：**
 
- 查看 Azure Active Directory 的[开发人员指南](../develop/active-directory-developers-guide.md)

- [开始试用](https://www.azure.cn/pricing/1rmb-trial/) - 立即注册试用版，开始将应用集成到 Azure AD

