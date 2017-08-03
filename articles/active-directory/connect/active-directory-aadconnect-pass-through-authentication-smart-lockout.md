---
title: "Azure AD Connect：直通身份验证 - 智能锁定 | Microsoft Docs"
description: "本文介绍 Azure Active Directory (Azure AD) 直通身份验证如何防范本地帐户遭到云中的暴力密码攻击。"
services: active-directory
keywords: "Azure AD Connect 直通身份验证, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: alexchen2016
manager: digimobile
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/12/2017
ms.date: 07/19/2017
ms.author: v-junlch
ms.openlocfilehash: b114c1fc559f8b602972e152eeccf2b9011db146
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Azure Active Directory 直通身份验证：智能锁定

## <a name="overview"></a>概述

Azure AD 可防范暴力密码攻击，并防止真正的用户被锁在 Office 365 和 SaaS 应用程序之外。 此功能称为**智能锁定**，使用直通身份验证作为登录方法时，便可支持此功能。 智能锁定默认已针对所有租户启用，会随时保护用户帐户；不需要手动将其打开。

智能锁定的工作原理是跟踪失败的登录尝试，在达到特定的**锁定阈值**后，开始触发**锁定持续时间**。 在锁定持续期间内，攻击者的任何登录尝试将被拒绝。 如果攻击继续进行，则在锁定持续时间结束后，后续的失败登录尝试会导致触发更长的锁定持续时间。

>[!NOTE]
>默认的“锁定阈值”为 10 次失败尝试，默认的“锁定持续时间”为 60 秒。

智能锁定还能区分真正用户和攻击者的登录，在大多数情况下只会锁定攻击者。 此功能可防止攻击者恶意锁定真正用户。 我们使用以往的登录行为、用户的设备和浏览器以及其他信号来区分真正用户和攻击者。 我们在不断改进算法。

由于直通身份验证会将密码验证请求转发到本地 Active Directory (AD)，因此需要防止攻击者锁定用户的 AD 帐户。 由于我们具有自己的 AD 帐户锁定策略（具体而言，为[**帐户锁定阈值**](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx)和[**在此后重置帐户锁定计数器**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)策略），因此需要适当地配置 Azure AD 的“锁定阈值”和“锁定持续时间”值，以便在攻击抵达本地 AD 之前，先在云中将其筛选掉。

>[!NOTE]
>尽管智能锁定功能本身是免费的，但使用图形 API 修改 Azure AD 的“锁定阈值”和“锁定持续时间”值是 Azure AD Premium P2 中的一项功能。 此外，只有租户上的全局管理员才能使用智能锁定。

为确保同时保护用户的本地 AD 帐户，需要确保：

1.  Azure AD 的锁定阈值_小于_ AD 的帐户锁定阈值。 我们建议将 AD“帐户锁定阈值”设置为 Azure AD“锁定阈值”的至少两倍或三倍。
2.  Azure AD 的“锁定持续时间”（以秒表示）_长于_ AD 的“在此后重置帐户锁定计数器”（以分钟表示）。

## <a name="verify-your-ad-account-lockout-policies"></a>验证 AD 帐户锁定策略

遵照以下说明验证 AD 帐户锁定策略：

1.  打开“组策略管理”工具。
2.  编辑已应用到所有用户的组策略（例如“默认域策略”）。
3.  导航到“计算机配置”>“策略”>“Windows 设置”>“安全设置”>“帐户策略”>“帐户锁定策略”。
4.  验证“帐户锁定阈值”和“在此后重置帐户锁定计数器”值。

![AD 帐户锁定策略](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values"></a>使用图形 API 管理租户的智能锁定值

>[!IMPORTANT]
>使用图形 API 修改 Azure AD 的“锁定阈值”和“锁定持续时间”值是 Azure AD Premium P2 中的一项功能。 此外，只有租户上的全局管理员才能使用智能锁定。

可以使用[图形资源管理器](https://developer.microsoft.com/graph/graph-explorer)读取、设置和更新 Azure AD 的智能锁定值。 但是，也可以通过编程方式执行这些操作。

### <a name="read-smart-lockout-values"></a>读取智能锁定值

遵循以下步骤读取租户的智能锁定值：

1. 以租户全局管理员的身份登录到图形资源管理器。 出现提示时，授予所需权限。
2. 单击“修改权限”，选择“Directory.ReadWrite.All”权限。
3. 按如下所述配置图形 API 请求：将版本设置为“BETA”，将请求类型设置为“GET”，将 URL 设置为 `https://graph.chinacloudapi.cn/beta/<your-tenant-domain>/settings`。
4. 单击“运行查询”查看租户的智能锁定值。 如果以前未设置过租户的值，则会看到一个空集。

### <a name="set-smart-lockout-values"></a>设置智能锁定值

遵循以下步骤设置租户的智能锁定值（仅适用于首次设置）：

1. 以租户全局管理员的身份登录到图形资源管理器。 出现提示时，授予所需权限。
2. 单击“修改权限”，选择“Directory.ReadWrite.All”权限。
3. 按如下所述配置图形 API 请求：将版本设置为“BETA”，将请求类型设置为“POST”，将 URL 设置为 `https://graph.chinacloudapi.cn/beta/<your-tenant-domain>/settings`。
4. 将以下 JSON 请求复制并粘贴到“请求正文”字段。 相应地更改智能锁定值，并为 `templateId` 使用随机 GUID。
5. 单击“运行查询”设置租户的智能锁定值。

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>如果不使用 **BannedPasswordList** 和 **EnableBannedPasswordCheck** 值，可分别将其保留为空 ("") 和“false”。

使用[这些步骤](#read-smart-lockout-values)验证是否已正确设置租户的智能锁定值。

### <a name="update-smart-lockout-values"></a>更新智能锁定值

遵循以下步骤更新租户的智能锁定值（如果之前已设置这些值）：

1. 以租户全局管理员的身份登录到图形资源管理器。 出现提示时，授予所需权限。
2. 单击“修改权限”，选择“Directory.ReadWrite.All”权限。
3. [遵循这些步骤读取租户的智能锁定值](#read-smart-lockout-values)。 复制使用“displayName”作为“PasswordRuleSettings”的项的 `id` 值 (GUID)。
4. 按如下所述配置图形 API 请求：将版本设置为“BETA”，将请求类型设置为“PATCH”，将 URL 设置为 `https://graph.chinacloudapi.cn/beta/<your-tenant-domain>/settings/<id>` - 为 `<id>` 使用步骤 3 中复制的 GUID。
5. 将以下 JSON 请求复制并粘贴到“请求正文”字段。 相应地更改智能锁定值。
6. 单击“运行查询”更新租户的智能锁定值。

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

使用[这些步骤](#read-smart-lockout-values)验证是否已正确更新租户的智能锁定值。

## <a name="next-steps"></a>后续步骤
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

