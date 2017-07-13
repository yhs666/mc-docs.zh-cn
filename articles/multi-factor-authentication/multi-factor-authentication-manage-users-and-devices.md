---
title: "管理员管理用户和设备 - Azure MFA | Microsoft Docs"
description: "本文介绍如何更改用户设置，例如，强制用户再次完成验证过程。"
documentationcenter: 
services: multi-factor-authentication
author: alexchen2016
manager: digimobile
editor: yossib
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/23/2017
ms.date: 06/27/2017
ms.author: v-junlch
ms.openlocfilehash: a3775aa4af4ac6e34a4bf599a1405a5ffc0705e1
ms.sourcegitcommit: a93ff901be297d731c91d77cd7d5c67da432f5d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2017
---
# 管理云中 Azure 多重身份验证的用户设置
<a id="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud" class="xliff"></a>
作为管理员，可以管理以下用户和设备设置：

- 要求用户再次提供联系方法
- 删除用户现有的应用密码

## 要求用户再次提供联系方法
<a id="require-users-to-provide-contact-methods-again" class="xliff"></a>
此设置会强制用户在登录时再次完成注册过程。 请注意，如果用户拥有应用密码，则非浏览器应用将继续工作。  你也可以通过选择“删除选定用户生成的所有现有应用密码”来删除用户的应用密码 。

### 如何要求用户再次提供联系方法
<a id="how-to-require-users-to-provide-contact-methods-again" class="xliff"></a>
1. 登录到 Azure 经典管理门户。
2. 在左侧单击“Active Directory”。
3. 在“目录”下，单击被要求再次提供联系方法的用户对应的目录。
4. 在顶部单击“用户”。
5. 在页面底部，单击“管理多重身份验证”。 此时将打开“多重身份验证”页。
6. 找到要管理的用户，并勾选其名称旁边的框。 你可能需要在顶部切换视图。
7. 右侧将显示“管理用户设置”链接。 请单击此按钮。
8. 选中“要求选定用户重新提供联系方式”。
   ![提供联系方式](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
9. 单击“保存”。
10. 单击“关闭”

## 删除用户现有的应用密码
<a id="delete-users-existing-app-passwords" class="xliff"></a>
此设置会删除用户创建的所有应用密码。 与这些应用密码关联的非浏览器应用将会停止工作，直到创建新应用密码为止。

### 如何删除用户现有的应用密码
<a id="how-to-delete-users-existing-app-passwords" class="xliff"></a>
1. 登录到 Azure 经典管理门户。
2. 在左侧单击“Active Directory”。
3. 在“目录”下，单击你要删除其应用密码的用户对应的目录。
4. 在顶部单击“用户”。
5. 在页面底部，单击“管理多重身份验证”。 此时将打开“多重身份验证”页。
6. 找到要管理的用户，并勾选其名称旁边的框。 你可能需要在顶部切换视图。
7. 右侧将显示“管理用户设置”链接。 请单击此按钮。
8. 选中“删除选定用户生成的所有现有应用密码”。
   ![删除应用密码](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. 单击“保存”。
10. 单击“关闭”。


## 后续步骤
<a id="next-steps" class="xliff"></a>

- 详细了解如何[配置 Azure 多重身份验证设置](multi-factor-authentication-whats-next.md)

- 如果用户需要支持，请将他们指向[双重验证用户指南](./end-user/multi-factor-authentication-end-user.md)

