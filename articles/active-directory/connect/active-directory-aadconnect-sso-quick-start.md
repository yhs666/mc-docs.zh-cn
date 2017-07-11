---
title: "Azure AD Connect：无缝单一登录 - 快速入门 | Microsoft Docs"
description: "本文介绍了如何开始使用 Azure Active Directory 无缝单一登录。"
services: active-directory
keywords: "什么是 Azure AD Connect, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/12/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: fd1a0def8a74871c139ee2b9c06a675a8e68aa31
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="azure-active-directory-seamless-single-sign-on-quick-start" class="xliff"></a>

# Azure Active Directory 无缝单一登录：快速入门

Azure Active Directory 无缝单一登录（Azure AD 无缝 SSO）在用户位于连接到企业网络的企业台式机上使其自动登录。 它可让用户轻松访问基于云的应用程序，而无需使用其他任何本地组件。

<a id="how-to-deploy-azure-ad-seamless-sso" class="xliff"></a>

## 如何部署 Azure AD 无缝 SSO

若要部署无缝 SSO，需要执行以下步骤：
1. *检查先决条件*：在启用此功能之前正确设置你的租户和本地环境。
2. *启用此功能*：使用 Azure AD Connect 在你的租户上开启无缝 SSO。
3. *推出此功能*：使用组策略向你的某些或所有用户推出此功能。
4. *测试此功能*：使用无缝 SSO 测试用户登录。

<a id="step-1-check-prerequisites" class="xliff"></a>

## 步骤 1：检查先决条件

确保满足以下先决条件：

1. 设置 Azure AD Connect 服务器：如果使用[直通身份验证](active-directory-aadconnect-pass-through-authentication.md)作为登录方法，则无需执行进一步操作。 如果使用[密码哈希同步](active-directory-aadconnectsync-implement-password-synchronization.md)作为登录方法，并且 Azure AD Connect 与 Azure AD 之间有防火墙，请确保：
- 你使用的是 1.1.484.0 版或更高版本的 Azure AD Connect。
- Azure AD Connect 可通过端口 443 与 `*.msappproxy.net` URL 进行通信。 此先决条件仅在启用此功能时适用，不针对实际用户登录。
- Azure AD Connect 可以与 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=42064)建立直接 IP 连接。 同样，此先决条件仅在启用此功能时适用。
2. 对于要使用 Azure AD Connect 同步到 Azure AD 且要为其用户启用无缝 SSO 的每个 AD 林，你都需要具有域管理员凭据。

<a id="step-2-enable-the-feature" class="xliff"></a>

## 步骤 2：启用此功能

可使用 [Azure AD Connect](active-directory-aadconnect.md) 启用无缝 SSO。

如果要执行 Azure AD Connect 的全新安装，请选择“自定义安装路径”。[](active-directory-aadconnect-get-started-custom.md) 在“用户登录”页上，选中“启用单一登录”选项。

![Azure AD Connect - 用户登录](./media/active-directory-aadconnect-sso/sso8.png)

如果已安装了 Azure AD Connect，请在 Azure AD Connect 上选择“更改用户登录页”并单击“下一步”。 然后选中“启用单一登录”选项。

![Azure AD Connect - 更改用户登录](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

在向导中继续操作，直到出现“启用单一登录”页。 对于要使用 Azure AD Connect 同步到 Azure AD 且要为其用户启用无缝 SSO 的每个 AD 林，提供域管理员凭据。 

完成向导后，便在你的租户上启用了无缝 SSO。

>[!NOTE]
> 域管理员凭据不存储在 Azure AD Connect 或 Azure AD 中，仅用来启用此功能。

<a id="step-3-roll-out-the-feature" class="xliff"></a>

## 步骤 3：推出此功能

若要向你的用户推出此功能，你需要通过 Active Directory 中的组策略将两个 Azure AD URL（https://autologon.microsoftazuread-sso.com 和 https://aadg.chinacloudapi.cn.nsatc.net）添加到用户的 Intranet 区域设置。

>[!NOTE]
> 以下说明仅适用于 Windows 上的 Internet Explorer 和 Google Chrome（如果它与 Internet Explorer 共用一组受信任站点 URL）。 有关在 Mac 上设置 Mozilla Firefox 和 Google Chrome 的说明，请阅读下一部分。

<a id="why-do-you-need-to-modify-users-intranet-zone-settings" class="xliff"></a>

### 为何需要修改用户的 Intranet 区域设置？

默认情况下，浏览器会基于 URL 自动计算适当的区域（Internet 或 Intranet）。 例如，http://contoso/ 映射到 Intranet 区域，而 http://intranet.contoso.com/ 映射到 Internet 区域（因为 URL 包含句点）。 浏览器不会将 Kerberos 票证发送到云终结点（例如两个 Azure AD URL），除非该终结点的 URL 已显式添加到浏览器的 Intranet 区域。

<a id="detailed-steps" class="xliff"></a>

### 详细步骤

1. 打开“组策略管理”工具。
2. 编辑应用于你的某些或所有用户的组策略。 在此示例中，我们使用**默认的域策略**。
3. 导航到“用户配置\管理模板\Windows 组件\Internet Explorer\Internet 控制面板\安全性”页面，并选择“区域分配列表的站点”。
![单一登录](./media/active-directory-aadconnect-sso/sso6.png)  
4. 启用此策略，并在对话框中输入以下值（将 Kerberos 票证转发到的 Azure AD URL）和数据（*1* 表示 Intranet 区域）。

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.chinacloudapi.cn.nsatc.net
        Data: 1
    
    >[!NOTE]
    > 如果希望禁止某些用户使用无缝 SSO（例如，如果这些用户在共享网亭上登录），请将前面的值设置为 *4*。 此操作将 Azure AD URL 添加到受限区域，并始终使无缝 SSO 失败。

5. 单击“确定”，然后再次单击“确定”。

![单一登录](./media/active-directory-aadconnect-sso/sso7.png)

<a id="browser-considerations" class="xliff"></a>

### 浏览器注意事项

<a id="mozilla-firefox" class="xliff"></a>

#### Mozilla Firefox

Mozilla Firefox 不会自动执行 Kerberos 身份验证。 每个用户都必须使用以下步骤手动将 Azure AD URL 添加到其 Firefox 设置：
1. 运行 Firefox 并在地址栏中输入 `about:config`。 关闭你看到的任何通知。
2. 搜索 **network.negotiate-auth.trusted-uris** 首选项。 此首选项针对 Kerberos 身份验证列出 Firefox 的受信任站点。
3. 单击右键并选择“修改”。
4. 在字段中输入“https://autologon.microsoftazuread-sso.com, https://aadg.chinacloudapi.cn.nsatc.net”。
5. 单击“确定”并重新打开浏览器。

>[!NOTE]
>无缝 SSO 无法在 Firefox 上以专用浏览模式工作。

<a id="google-chrome-on-mac" class="xliff"></a>

#### Mac 上的 Google Chrome

对于 Mac 和其他非 Windows 平台上的 Google Chrome，请参阅[本文](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist)中有关如何将 Azure AD URL 列入白名单以进行集成身份验证的信息。

使用第三方 Active Directory 组策略扩展向 Mac 上的 Firefox 和 Google Chrome 用户推出 Azure AD URL 不在本文的讨论范围内。

<a id="step-4-test-the-feature" class="xliff"></a>

## 步骤 4：测试此功能

若要针对特定用户测试此功能，请确保满足以下“所有”条件：
  - 用户在某个企业设备上登录。
  - 该设备之前已加入到你的 Active Directory (AD) 域。
  - 该设备已通过远程访问连接（例如 VPN 连接）与企业有线或无线网络中的域控制器 (DC) 建立了直接连接。
  - 你已使用组策略向此用户[推出了此功能](##step-3-roll-out-the-feature)。

若要测试用户仅输入用户名但未输入密码的应用场景，请执行以下操作：
- 在新的专用浏览器会话中登录到 *https://login.partner.microsoftonline.cn/*。

若要测试用户不必输入用户名或密码的应用场景，请执行以下操作： 
- 在新的专用浏览器会话中登录到 *https://login.partner.microsoftonline.cn/contoso.partner.onmschina.cn*。 将“*contoso*”替换为你的租户的名称。
- 或者，在新的专用浏览器会话中登录到 *https://login.partner.microsoftonline.cn/contoso.com*。 将“*contoso.com*”替换为你的租户中某个已验证的域（非联盟域）。

<a id="next-steps" class="xliff"></a>

## 后续步骤

- [**技术深入探讨**](active-directory-aadconnect-sso-how-it-works.md) - 了解此功能的工作原理。
- [**常见问题**](active-directory-aadconnect-sso-faq.md) - 常见问题的解答。
- [**故障排除**](active-directory-aadconnect-troubleshoot-sso.md) - 了解如何解决此功能的常见问题。
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

