---
title: 什么是 Azure Active Directory？ | Microsoft Docs
description: 了解 Azure Active Directory，包括必要的术语和相关功能。
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.topic: overview
origin.date: 11/13/2018
ms.date: 03/18/2019
ms.author: v-junlch
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.openlocfilehash: 0792dbddd3e242b98ad7867060e247fb5c3482b1
ms.sourcegitcommit: 46a8da077726a15b5923e4e688fd92153ebe2bf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "58186650"
---
# <a name="what-is-azure-active-directory"></a>什么是 Azure Active Directory？
Azure Active Directory (Azure AD) 是 Microsoft 提供的多租户、基于云的目录和标识管理服务。 Azure AD 将核心目录服务、应用程序访问管理和标识保护融入单个解决方案中，提供基于标准的平台，可帮助开发人员根据集中的策略和规则实现对其应用的访问控制。

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

<!-- Update_Description: update metedata properties -->