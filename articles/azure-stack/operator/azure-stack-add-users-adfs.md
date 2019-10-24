---
title: 在 AD FS 中添加 Azure Stack 用户 | Microsoft Docs
description: 了解如何为 Active Directory 联合身份验证服务 (AD FS) 部署添加 Azure Stack 用户。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/03/2019
ms.date: 10/21/2019
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 06/03/2019
ms.openlocfilehash: a8ca3e64a84278446675d5052c3e4cbe85e12489
ms.sourcegitcommit: 713bd1d1b476cec5ed3a9a5615cfdb126bc585f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578501"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>在 AD FS 中添加 Azure Stack 用户
可以使用“Active Directory 用户和计算机”  管理单元将其他用户添加到 Azure Stack 环境中，并使用 Active Directory 联合身份验证服务 (AD FS) 作为其标识提供者。

## <a name="add-windows-server-active-directory-users"></a>添加 Windows Server Active Directory 用户
> [!TIP]
> 此示例使用默认的 azurestack.local ASDK Active Directory。 

1. 使用允许访问 Windows 管理工具的帐户登录计算机，然后打开新的 Microsoft 管理控制台 (MMC)。
2. 选择“文件”>“添加或删除管理单元”  。
3. 选择“Active Directory 用户和计算机”   > “AzureStack.local”   > “用户”  。
4. 选择“操作”   > “新建”   > “用户”  。
5. 在“新建对象 - 用户”中，提供用户详细信息。 选择“**下一步**”。
6. 提供并确认密码。
7. 选择“下一步”  以完成值。 选择“完成”  以创建用户。


## <a name="next-steps"></a>后续步骤
[创建服务主体](azure-stack-create-service-principals.md)