---
title: "Azure AD 直通身份验证 - 快速入门 | Microsoft Docs"
description: "本文介绍了如何开始使用 Azure Active Directory (Azure AD) 直通身份验证。"
services: active-directory
keywords: "Azure AD Connect 直通身份验证, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/15/2017
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: b4c83eb20fc9d49b8778dc6b7c6a0199333bfdf7
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="azure-active-directory-pass-through-authentication-quick-start" class="xliff"></a>

# Azure Active Directory 直通身份验证：快速入门

Azure Active Directory (Azure AD) 直通身份验证可让用户使用相同的密码登录到本地应用程序和基于云的应用程序。 它通过直接对照本地 Active Directory 对用户的密码进行验证来使用户登录。

<a id="how-to-deploy-azure-ad-pass-through-authentication" class="xliff"></a>

## 如何部署 Azure AD 直通身份验证

若要部署直通身份验证，需要执行以下步骤：
1. *检查先决条件*：在启用此功能之前正确设置你的租户和本地环境。
2. *启用此功能*：在你的租户上开启直通身份验证，并安装轻型本地代理来处理密码验证请求。
3. *测试此功能*：使用直通身份验证测试用户登录。
4. *确保高可用性*：安装另一个独立的代理来为登录请求提供高可用性。

<a id="step-1-check-prerequisites" class="xliff"></a>

## 步骤 1：检查先决条件

确保满足以下先决条件：

<a id="on-the-azure-portal" class="xliff"></a>

### 在 Azure 门户上

1. 在 Azure AD 租户上创建一个仅限云的全局管理员帐户。 这样一来，就可以在本地服务出现故障或不可用时管理租户的配置。 执行此步骤对于确保你的租户不会被锁定非常重要。
2. 向 Azure AD 租户添加一个或多个[自定义域名](../active-directory-add-domain.md)。 用户将使用这些域名之一进行登录。

<a id="in-your-on-premises-environment" class="xliff"></a>

### 在本地环境中

1. 确定一台运行 Windows Server 2012 R2 或更高版本的服务器，将在其上运行 Azure AD Connect。 将该服务器添加到需要验证其密码的用户所在的同一 AD 林。
2. 在步骤 2 中确定的服务器上安装 [Azure AD Connect 的最新版本](https://www.microsoft.com/download/details.aspx?id=47594)。 如果已在运行 Azure AD Connect，请确保版本为 1.1.486.0 或更高版本。
3. 确定另一台运行 Windows Server 2012 R2 或更高版本的服务器，将在其上运行独立的身份验证代理。 身份验证代理版本需要为 1.5.58.0 或更高版本。 为确保登录请求的高可用性，此服务器是必需的。 将该服务器添加到需要验证其密码的用户所在的同一 AD 林。
4. 如果服务器与 Azure AD 之间存在防火墙，则需要配置以下各项：
   - 打开端口：确保你的服务器上的身份验证代理可以通过端口 80 和 443 向 Azure AD 发出出站请求。 如果防火墙根据发起用户强制实施规则，请为来自作为网络服务运行的 Windows 服务的流量打开这些端口。
   - 允许 Azure AD 终结点：如果启用了 URL 筛选，请确保身份验证代理可以与 **\*.msappproxy.net** 和 **\*.servicebus.chinacloudapi.cn** 进行通信。
   - 验证直接 IP 连接：确保服务器上的身份验证代理可以与 [Azure 数据中心 IP 范围](https://www.microsoft.com/en-us/download/details.aspx?id=42064)建立直接 IP 连接。

<a id="step-2-enable-the-feature" class="xliff"></a>

## 步骤 2：启用此功能

可以使用 [Azure AD Connect](active-directory-aadconnect.md) 启用直通身份验证。

如果是首次安装 Azure AD Connect，请选择[自定义安装路径](active-directory-aadconnect-get-started-custom.md)。 在“用户登录”页上，选择“直通身份验证”作为登录方法。 成功完成上述步骤后，将在 Azure AD Connect 所在的服务器上安装直通身份验证代理。 此外，还会在租户中启用直通身份验证功能。

![Azure AD Connect - 用户登录](./media/active-directory-aadconnect-sso/sso3.png)

如果已安装了 Azure AD Connect（使用[快速安装](active-directory-aadconnect-get-started-express.md)或[自定义安装](active-directory-aadconnect-get-started-custom.md)路径），请在 Azure AD Connect 上选择“更改用户登录页”并单击“下一步”。 然后，选择“直通身份验证”作为登录方法。 当成功完成时，会在 Azure AD Connect 所在的服务器上安装直通身份验证代理并在租户上启用此功能。

![Azure AD Connect - 更改用户登录](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>直通身份验证是一项租户级功能。 开启此功能会影响你的租户中“所有”托管域中的用户的登录。

<a id="step-3-test-the-feature" class="xliff"></a>

## 步骤 3：测试此功能

在完成步骤 2 后，你的租户中所有托管域中的用户都将使用直通身份验证进行登录。 但是，联合域中的用户将继续使用 Active Directory 联合身份验证服务 (AD FS) 或你以前配置的其他联合身份验证提供程序进行登录。 如果你将联合域转换为托管域，该域中的所有用户将自动开始使用直通身份验证进行登录。 仅限云的用户不受直通身份验证功能的影响。

<a id="step-4-ensure-high-availability" class="xliff"></a>

## 步骤 4：确保高可用性

如果你计划在生产环境中部署直通身份验证，则应当安装独立的身份验证代理。 请在运行 Azure AD Connect 和第一个身份验证代理的服务器之外的服务器上安装此第二个身份验证代理。 此设置为登录请求提供了高可用性。 遵照以下说明部署独立的身份验证代理：

<a id="download-and-install-the-authentication-agent-software-on-your-server" class="xliff"></a>

### 在服务器上下载并安装身份验证代理软件

1.  [下载](https://go.microsoft.com/fwlink/?linkid=837580)最新的身份验证代理软件。 验证版本是否为 1.5.58.0 或更高版本。
2.  以管理员身份打开命令提示符。
3.  运行以下命令（**/q** 选项表示“静默安装”- 此安装不会提示你接受最终用户许可协议）：`
AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q
`

>[!NOTE]
>每台服务器上只能安装一个身份验证代理。

<a id="register-the-authentication-agent-with-azure-ad" class="xliff"></a>

### 向 Azure AD 注册身份验证代理

1.  以管理员身份打开 PowerShell 窗口。
2.  导航到 **C:\Program Files\Microsoft AAD App Proxy agent**，然后如下所示运行脚本：`.\Registeragent.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy agent\Modules\" -moduleName "AppProxyPSModule" -Feature PassthroughAuthentication`
3.  出现提示时，请输入 Azure AD 租户中全局管理员帐户的凭据。

<a id="next-steps" class="xliff"></a>

## 后续步骤
- [**当前限制**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - 此功能目前以预览版提供。 了解支持的方案和不支持的方案。
- [**技术深入探讨**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - 了解此功能的工作原理。
- [**常见问题**](active-directory-aadconnect-pass-through-authentication-faq.md) - 常见问题的解答。
- [**故障排除**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - 了解如何解决此功能的常见问题。
- [**Azure AD 无缝 SSO**](active-directory-aadconnect-sso.md) - 了解有关此补充功能的详细信息。
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

