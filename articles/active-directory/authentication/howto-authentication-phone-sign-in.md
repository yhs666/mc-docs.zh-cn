---
title: 使用 Microsoft Authenticator 应用（公共预览版）进行无密码 Azure AD 登录
description: 不使用密码通过 Microsoft Authenticator 应用（公共预览版）登录到 Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
origin.date: 09/20/2018
ms.date: 12/28/2018
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: librown
ms.openlocfilehash: d4e3f4337ac3ee908b878604f82b654466139598
ms.sourcegitcommit: 4f91d9bc4c607cf254479a6e5c726849caa95ad8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996177"
---
# <a name="password-less-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>使用 Microsoft Authenticator 应用（公共预览版）进行无密码手机登录

使用 Microsoft Authenticator 应用可以登录到任何 Azure AD 帐户，且无需输入密码。 Microsoft Authenticator 使用基于密钥的身份验证来启用绑定到设备的用户凭据，并使用生物识别或 PIN。

![要求用户在其 Microsoft Authenticator 应用中批准登录尝试的浏览器登录示例](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

在 Microsoft Authenticator 应用中启用手机登录的用户在输入用户名后，看到的不是密码提示，而是一条消息，告知他们在应用中点击一个数字。 在应用中，该用户必须输入匹配的数字，选择“批准”，提供 PIN 或生物识别特征，然后身份验证将会完成。

## <a name="enable-my-users"></a>启用我的用户

在公共预览版中，管理员必须先通过 PowerShell 添加一个策略，以允许使用租户中的凭据。 在执行此步骤之前，请查看“已知问题”部分。

### <a name="tenant-prerequisites"></a>租户先决条件

- Azure Active Directory
- 为最终用户启用了 Azure 多重身份验证
- 用户可以注册其设备

### <a name="steps-to-enable"></a>启用步骤

确保已安装 Azure Active Directory V2 PowerShell 模块的最新公共预览版。 你可能希望通过执行以下命令卸载并重新安装以确认这一点：

1. `Uninstall-Module -Name AzureADPreview`
2. `Install-Module -Name AzureADPreview`

可以使用以下 PowerShell 命令启用无密码电话登录预览：

1. `Connect-AzureAD -AzureEnvironmentName AzureChinaCloud`
   1. 在身份验证对话框中，使用租户中的帐户登录。 该帐户必须是安全管理员或全局管理员。
1. `New-AzureADPolicy -Type AuthenticatorAppSignInPolicy -Definition '{"AuthenticatorAppSignInPolicy":{"Enabled":true}}' -isOrganizationDefault $true -DisplayName AuthenticatorAppSignIn`

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>我的最终用户如何启用手机登录？

在公共预览版中，没有任何方法可以强制用户创建或使用此新凭据。 只有在管理员启用了最终用户的租户，并且用户已将其 Microsoft Authenticator 应用更新为启用手机登录时，最终用户才能使用无密码登录。

> [!NOTE]
> 此功能已在 2017 年 3 月开始植入到应用中，因此，在为租户启用该策略时，用户可能会立即遇到此流程。 请注意这一点，并让用户为此更改做好准备。
>

1. 在 Azure 多重身份验证中注册
1. 已在运行 iOS 8.0 或更高版本或者 Android 6.0 或更高版本的设备上安装最新版本的 Microsoft Authenticator。
1. 已在应用中添加支持推送通知的工作或学校帐户。 可以在 [Microsoft Authenticator 应用入门](/active-directory/user-help/microsoft-authenticator-app-how-to)中找到最终用户文档。

在 Microsoft Authenticator 应用中为用户设置支持推送通知的 MFA 帐户后，用户可以遵循[使用手机（而不是密码）登录](../user-help/microsoft-authenticator-app-phone-signin-faq.md)一文中的步骤完成手机登录注册。

[了解 Azure 多重身份验证](../authentication/howto-mfa-getstarted.md)

<!-- Update_Description: wording update -->