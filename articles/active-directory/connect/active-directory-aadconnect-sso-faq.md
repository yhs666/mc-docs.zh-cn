---
title: "Azure AD Connect：无缝单一登录 - 常见问题 | Microsoft Docs"
description: "有关 Azure Active Directory 无缝单一登录的常见问题的解答。"
services: active-directory
keywords: "什么是 Azure AD Connect, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
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
ms.date: 07/31/2017
ms.author: v-junlch
ms.openlocfilehash: 2661fd3d92216ec3bb0c53f5cc018335a0227cd7
ms.sourcegitcommit: cd0f14ddb0bf91c312d5ced9f38217cfaf0667f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory 无缝单一登录：常见问题

本文解答有关 Azure AD 无缝 SSO 的常见问题。 请随时返回查看新内容。

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>无缝 SSO 可与哪些登录方法配合使用？

无缝 SSO 可与[密码哈希同步](active-directory-aadconnectsync-implement-password-synchronization.md)或[直通身份验证](active-directory-aadconnect-pass-through-authentication.md)登录方法结合使用，但不能与 Active Directory 联合身份验证服务 (ADFS) 结合使用。

## <a name="is-seamless-sso-a-free-feature"></a>无缝 SSO 是否是免费功能？

无缝 SSO 是一个免费功能，不需要拥有任何付费版本的 Azure AD 即可使用此功能。 功能正式发布后，仍可免费使用。

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>哪些应用程序利用无缝 SSO 的 `domain_hint` 或 `login_hint` 参数功能？

我们正在汇编会发送和不会发送这些参数的应用程序列表。 如果你的应用程序有相关需要，请在评论部门告诉我们。

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>无缝 SSO 是否支持使用 `Alternate ID`（而不是 `userPrincipalName`）作为用户名？

是的。 在 Azure AD Connect 中经过配置后，无缝 SSO 支持使用 `Alternate ID` 作为用户名，如[此处](active-directory-aadconnect-get-started-custom.md)所示。 并非所有 Office 365 应用程序都支持 `Alternate ID`。 请参阅支持声明的特定应用程序文档。

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>我希望在不使用 AD FS 的情况下将非 Windows 10 设备注册到 Azure AD。 是否可以改用无缝 SSO？

可以，此方案需要 2.1 版或更高版本的[工作区加入客户端](https://www.microsoft.com/download/details.aspx?id=53554)。

## <a name="how-can-i-disable-seamless-sso"></a>如何禁用无缝 SSO？

可以使用 Azure AD Connect 禁用无缝 SSO。

运行 Azure AD Connect，选择“更改用户登录页面”，单击“下一步”。 然后取消选中“启用单一登录”选项。 在向导中继续操作。 完成向导后，便会在租户中禁用无缝 SSO。

不过，屏幕上将显示一条消息，如下所示：

“单一登录现在已被禁用，但若要完成清理，需要另外执行一些手动步骤。 了解详细信息”

下面是需要执行的手动步骤：

1. 获取已在其中启用了无缝 SSO 的 AD 林的列表

    - 首先，下载并安装 [Microsoft Online Services 登录助手](http://go.microsoft.com/fwlink/?LinkID=286152)。
    - 然后下载并安装[用于 Windows PowerShell 的 64 位 Azure Active Directory 模块](http://go.microsoft.com/fwlink/p/?linkid=236297)。
    - 导航到 `%programfiles%\Azure Active Directory Connect` 文件夹。
    - 使用以下命令导入无缝 SSO PowerShell 模块：`Import-Module .\AzureADSSO.psd1`。
        - 在 PowerShell 中，调用 `New-AzureADSSOAuthenticationContext`。 此命令应会提供一个弹出窗口，用于输入 Azure AD 租户管理员凭据。
        - 调用 `Get-AzureADSSOStatus`。 此命令提供已在其中启用了此功能的 AD 林的列表（请查看“域”列表）。
2. 从上面列出的每个 AD 林中手动删除 `AZUREADSSOACCT` 计算机帐户。

## <a name="next-steps"></a>后续步骤

- [**快速入门**](active-directory-aadconnect-sso-quick-start.md) - 启动并运行 Azure AD 无缝 SSO。
- [**技术深入探讨**](active-directory-aadconnect-sso-how-it-works.md) - 了解此功能的工作原理。
- [**故障排除**](active-directory-aadconnect-troubleshoot-sso.md) - 了解如何解决此功能的常见问题。
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - 用于提出新的功能请求。

<!-- Update_Description: update meta properties -->