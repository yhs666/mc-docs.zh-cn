---
title: 什么是 Azure Active Directory (Azure AD)？ | Microsoft Docs
description: 了解如何使用 Azure Active Directory 将现有的本地标识扩展到云中，或开发 Azure AD 集成应用。
services: active-directory
author: eross-msft
manager: mtillman
ms.author: v-junlch
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
origin.date: 09/13/2018
ms.date: 12/10/2018
ms.custom: it-pro
ms.openlocfilehash: 521fa9cdef50d3eca5ab88c857662ec7c4e5fd5a
ms.sourcegitcommit: 833865e1f1e99b3acd10781451eed636cc7cc810
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "53157387"
---
# <a name="what-is-azure-active-directory"></a>什么是 Azure Active Directory？
Azure Active Directory (Azure AD) 是 Microsoft 提供的多租户、基于云的目录和标识管理服务。 Azure AD 将核心目录服务、应用程序访问管理和标识保护组合到一个解决方案中，提供基于标准的平台，帮助开发人员根据集中策略和规则为其应用程序提供访问控制。

![Azure AD Connect 堆栈](./media/active-directory-whatis/Azure_Active_Directory.png)

>[!Note]
>中国地区目前不支持 Azure Active Directory Premium P1 版、Premium P2 版和 Azure Active Directory 基本版。

## <a name="benefits-of-azure-ad"></a>Azure AD 的优势
Azure AD 可帮助你：

-   为整个企业中的每个用户创建和管理单一标识，使用户和组与 [Azure AD Connect](../connect/active-directory-aadconnect.md) 保持同步。

-   通过对本地应用和云应用强制执行基于规则的[多重身份验证](../authentication/concept-mfa-howitworks.md)，启用应用程序访问安全性。

## <a name="who-uses-azure-ad"></a>谁使用 Azure AD
Azure AD 适用于应用开发人员以及 Office 365、Azure 用户。

- **面向应用开发人员。** Azure AD 通过提供与全球数百万组织所用的标识管理解决方案的集成，帮助你专注于构建应用。

- **面向 Office 365 客户和 Azure 客户。** 你已在使用 Azure AD。 每个 Office 365 和 Azure 租户实际上是 Azure AD 租户，因此你可以立即开始管理用户对集成云应用的访问。

## <a name="how-reliable-is-azure-ad"></a>Azure AD 的可靠性如何？
Azure AD 的多租户、地理分布、高可用性设计意味着可以依赖它来解决最关键的业务需求。 Azure AD 通过自动故障转移在全球 28 个数据中心中运行。 这意味着即使数据中心出现故障，目录数据的副本也会存在于至少另外两个区域分散的数据中心中，并且可供即时访问。

有关服务级别协议的详细信息，请参阅[服务级别协议](https://www.azure.cn/support/legal/sla/)。

## <a name="choose-an-edition"></a>选择版本
有关这些版本的定价选项，请参阅 [Azure Active Directory 定价](https://www.azure.cn/pricing/details/active-directory/)


## <a name="next-steps"></a>后续步骤
- [将 Azure AD 与 Windows Server Active Directory 集成](../hybrid/how-to-connect-install-express.md)。

