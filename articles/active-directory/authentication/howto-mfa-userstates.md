---
title: Azure 多重身份验证用户状态
description: 了解 Azure 多重身份验证中的用户状态。
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
origin.date: 06/26/2017
ms.date: 08/03/2018
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: f8d30b0edeed03f706cbd4520241b648c5c8fc26
ms.sourcegitcommit: 98c7d04c66f18b26faae45f2406a2fa6aac39415
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2018
ms.locfileid: "39487035"
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>如何要求对用户或组进行双重验证

**通过更改用户状态启用 Azure 多重身份验证**是要求进行双重验证的传统方法。 启用的所有用户在每次登录时都要执行双重验证。 启用某个用户会覆盖可能影响该用户的任何条件性访问策略。 

## <a name="enable-azure-mfa-by-changing-user-status"></a>通过更改用户状态启用 Azure MFA

Azure 多重身份验证中的用户帐户具有以下三种不同状态：

| 状态 | 说明 | 受影响的非浏览器应用 | 受影响的浏览器应用 | 新式身份验证受影响 |
|:---:|:---:|:---:|:--:|:--:|
| 已禁用 |没有在 Azure MFA 中注册某个新用户的默认状态。 |否 |否 |否 |
| Enabled |用户已加入 Azure MFA 但尚未注册。 在用户下次登录时会提示他们进行注册。 |否。  它们继续工作，直到注册过程完成。 | 是的。 会话过期后，会要求进行 Azure MFA 注册。| 是的。 访问令牌过期后，会要求进行 Azure MFA 注册。 |
| 强制 |用户已登记，并已完成 Azure MFA 的注册过程。 |是的。  应用需要应用密码。 |是的。 在登录时会要求进行 Azure MFA。 | 是的。 在登录时会要求进行 Azure MFA。 |

用户的状态反映管理员是否已在 Azure MFA 中登记用户以及用户是否已完成注册过程。

所有用户的初始状态均为“已禁用”。 在 Azure MFA 中注册用户后，用户的状态将更改为“已启用”。 当已启用的用户登录并完成注册过程后，用户的状态将更改为“强制”。  

### <a name="view-the-status-for-a-user"></a>查看用户状态

使用以下步骤来访问可在其中查看和管理用户状态的页面：

1. 以管理员身份登录到 [Azure 门户](https://portal.azure.cn)。
2. 转到“Azure Active Directory” > “用户和组” > “所有用户”。
3. 选择“多重身份验证”。
   选择“多重身份验证”![](./media/howto-mfa-userstates/selectmfa.png)
4. 此时会打开一个新页面，其中显示了用户状态。
   ![多重身份验证用户状态 - 屏幕截图](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>更改用户状态

1. 使用前文的步骤访问 Azure 多重身份验证“用户”页面。
2. 找到希望对其启用 Azure MFA 的用户。 可能需要在顶部更改视图。 
   ![查找用户 - 屏幕截图](./media/howto-mfa-userstates/enable1.png)
3. 勾选用户名旁边的框。
4. 在右侧，在“快速步骤”下，选择“启用”或“禁用”。
   ![启用选定的用户 - 屏幕截图](./media/howto-mfa-userstates/user1.png)

   >[!TIP]
   >“已启用”的用户在注册 Azure MFA 后会自动切换到“强制”。 不应手动将用户状态更改为“强制”。 

5. 在打开的弹出窗口中确认你的选择。 

启用用户后，通过电子邮件通知他们。 告诉他们将需要在下次登录时进行注册。 还可以包括指向 [Azure MFA 最终用户指南](../user-help/multi-factor-authentication-end-user.md)的链接，以便帮助他们上手。

### <a name="use-powershell"></a>使用 PowerShell
若要使用 [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/overview) 更改用户状态，请更改 `$st.State`。 有三种可能的状态：

- Enabled
- 强制
- 已禁用  

不要直接将用户移动到“强制”状态。 如果这样做了，则非基于浏览器的应用将停止工作，因为用户尚未完成 Azure MFA 注册并获得应用密码。

当你需要批量启用用户时，使用 PowerShell 是一个不错的选择。 创建一个 PowerShell 脚本，它会循环访问用户列表并启用它们：

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = "Enabled"
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

下面的脚本就是一个示例。

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = "Enabled"
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="next-steps"></a>后续步骤

- 管理[用户及其设备](howto-mfa-userdevicesettings.md)的 Azure 多重身份验证设置。

<!-- Update_Description: link update -->