---
title: "Azure Active Directory 中的重置密码 | Microsoft 文档"
description: "管理员在 Azure Active Directory 中为用户重置密码"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
editor: 
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: 5e176b10ae5b2ac140392b31b499cb9b77018764
ms.sourcegitcommit: 469a0ce3979408a4919a45c1eb485263f506f900
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>在 Azure Active Directory 中重置用户的密码

在用户忘记密码或用户被锁定等情况下，管理员可能需要重置用户的密码。 可通过以下步骤完成用户密码重置。

## <a name="how-to-reset-the-password-for-a-user"></a>如何重置用户的密码

1. 使用具有目录权限的帐户登录到 [Azure 门户](https://portal.azure.cn)来重置用户密码。
2. 选择“Azure Active Directory” > “用户和组” > “所有用户”。
3. 选择要为其重置密码的用户。
2. 对于所选用户，选择“重置密码”。


    
6. 在“重置密码”上，选择“重置密码”。
7. 会显示一个临时密码，可将其提供给用户。 下一次登录时将要求用户更改密码。 

   > [!NOTE]
   > 此临时密码不具有过期时间，因此在用于登录之前将始终有效，登录后将强制用户更改密码。 

## <a name="next-steps"></a>后续步骤
* [向用户分配管理员角色](active-directory-users-assign-role-azure-portal.md)

