---
title: "Azure 多重身份验证用户状态"
description: "在 Azure MFA 中了解用户状态信息。"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/16/2017
ms.date: 03/31/2017
ms.author: v-junlch
ms.openlocfilehash: cf51a52b0f76da7aed7ac90ab0d5bf02bdfe9e96
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="user-states-in-azure-multi-factor-authentication"></a>Azure 多重身份验证中的用户状态
Azure 多重身份验证中的用户帐户具有以下三种不同状态：

| 状态 | 说明 | 受影响的非浏览器应用 | 
|:---:|:---:|:---:|
| 已禁用 |未加入多重身份验证 (MFA) 的新用户的默认状态。 |否 |
| Enabled |用户已加入 Azure MFA 但尚未注册。 系统将在用户下次登录时提示注册。 |否。  它们继续工作，直到注册过程完成。 |
| 强制 |用户已登记，并已完成 Azure MFA 的注册过程。 |是的。  应用需要应用密码。 |

## <a name="changing-a-user-state"></a>更改用户状态
用户的状态反映管理员是否已在 Azure MFA 中登记用户以及用户是否已完成注册过程。

所有用户的初始状态均为“已禁用”。 在 Azure MFA 中登记用户时，用户的状态将更改为“已启用”。 当已启用的用户登录并完成注册过程时，用户的状态将更改为“强制”。  

### <a name="view-user-states"></a>查看用户状态

使用以下步骤来访问可在其中查看和管理用户状态的页面：

1. 以管理员身份登录 [Azure 经典管理门户](https://manage.windowsazure.cn) 。
2. 在左侧选择“Active Directory”。
3. 从目录中选择要查看的用户。
   ![选择目录 - 屏幕截图](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. 选择“用户”。
5. 在页面底部，选择“管理多重身份验证”。 
   ![选择“管理多重身份验证”- 屏幕截图](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. 此时会打开一个显示用户状态的新选项卡。
   ![多重身份验证用户状态 - 屏幕截图](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-state-from-disabled-to-enabled"></a>将状态从“已禁用”更改为“已启用”

1. 使用前面所述的步骤访问多重身份验证用户页面。 
2. 找到想要对其启用 Azure MFA 的用户。 你可能需要在顶部切换视图。 确保状态为“已禁用”。
   ![查找用户 - 屏幕截图](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. 勾选用户名旁边的框。
4. 在右侧的快速步骤下，单击“启用”。
   ![启用选定的用户 - 屏幕截图](./media/multi-factor-authentication-get-started-cloud/user1.png)
5. 选择“启用多重身份验证”。
   ![启用多重身份验证 - 屏幕截图](./media/multi-factor-authentication-get-started-cloud/enable2.png)
6. 请注意，用户状态已从“已禁用”更改为“已启用”。
   ![用户现已启用 - 屏幕截图](./media/multi-factor-authentication-get-started-cloud/user.png)

启用用户后，应通过电子邮件通知他们。 在电子邮件告知他们在下次登录时需要注册，以及某些非浏览器应用可能不支持双重验证。 还可以包括指向 [Azure MFA 最终用户指南](../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md)的链接，以便帮助他们上手。 

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>将状态从已启用/强制更改为已禁用

1. 使用 [查看用户状态](#view-user-states) 中的步骤访问多重身份验证用户页面。
6. 找到要禁用的用户。 你可能需要在顶部切换视图。 确保状态为“已启用”或“强制”。
7. 勾选用户名旁边的框。
8. 在右侧的快速步骤下，单击“禁用”。
   ![禁用用户 - 屏幕截图](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
9. 系统会提示你确认该操作。 单击 **“是”**。
10. 如果已成功禁用用户，将显示成功消息。 单击“关闭” 。