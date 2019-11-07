---
title: 客户在 Azure Active Directory B2C 中注册期间禁用电子邮件验证
description: 了解客户在 Azure Active Directory B2C 中注册期间如何禁用电子邮件验证。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
origin.date: 09/25/2018
ms.date: 10/23/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: c11358dbd0a1adb6f2784665720338300f083c43
ms.sourcegitcommit: 817faf4e8d15ca212a2f802593d92c4952516ef4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72847058"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>客户在 Azure Active Directory B2C 中注册期间禁用电子邮件验证

默认情况下，Azure Active Directory B2C (Azure AD B2C) 将验证客户的本地帐户（使用电子邮件地址或用户名注册的用户的帐户）的电子邮件地址。 Azure AD B2C 通过要求客户在注册过程中验证电子邮件地址来确保电子邮件地址有效。 它还可防止恶意执行组件使用自动进程在应用程序中生成欺诈帐户。

某些应用程序开发人员愿意在注册过程中跳过电子邮件验证，而让客户在以后验证电子邮件地址。 要支持此功能，可将 Azure AD B2C 配置为禁用电子邮件验证。 这样做可创建更流畅的注册过程，并使开发人员可以灵活地将已验证电子邮件地址的客户与尚未验证电子邮件地址的客户区分开来。

请按照以下步骤禁用电子邮件验证：

1. 登录到 [Azure 门户](https://portal.azure.cn)
1. 使用顶部菜单中的“目录 + 订阅”  筛选器来选择包含 Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”  。 或者，选择“所有服务”  并搜索并选择“Azure AD B2C”  。
1. 选择“用户流”  。
1. 选择要禁用电子邮件验证的用户流。 例如，*B2C_1_signinsignup*。
1. 选择“页面布局”。 
1. 选择“本地帐户注册页”  。
1. 在**用户属性**下，选择“电子邮件地址”  。
1. 在“要求验证”  下拉列表中，选择“否”  。
1. 选择“保存”  。 现在已为此用户流禁用电子邮件验证。

> [!WARNING]
> 在注册过程中禁用电子邮件验证可能会导致垃圾邮件。 如果禁用默认 Azure AD B2C 提供的电子邮件验证，我们建议你实现替换验证系统。

<!-- Update_Description: wording update -->