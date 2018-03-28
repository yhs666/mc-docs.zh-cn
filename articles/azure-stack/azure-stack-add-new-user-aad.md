---
title: 在 Azure Active Directory 中添加新的 Azure Stack 租户帐户 | Microsoft Docs
description: 部署 Azure Stack 开发工具包后，需要至少创建一个租户用户帐户，以便浏览租户门户。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/21/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: 707277486f1f94b19967e9dc5ff57b5cc7cb4fe6
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>在 Azure Active Directory 中添加新的 Azure Stack 租户帐户
[部署 Azure Stack 开发工具包](azure-stack-run-powershell-script.md)后，需要租户用户帐户，以便浏览租户门户和测试产品/服务和计划。 可以[使用 Azure 门户](#create-an-azure-stack-tenant-account-using-the-azure-portal)或[使用 PowerShell](#create-an-azure-stack-tenant-account-using-powershell) 创建租户帐户。

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>使用 Azure 门户创建 Azure Stack 租户帐户
必须具有 Azure 订阅才能使用 Azure 门户。

1. 登录到 [Azure](https://portal.azure.cn)。
2. 在 Azure 左侧导航栏中，单击“Active Directory”。
3. 在目录列表中，单击要用于 Azure Stack 的目录或创建一个新目录。
4. 在此目录页上，单击“用户”。
5. 单击“添加用户”。
6. 在“添加用户”向导的“用户类型”列表中，选择“组织中的新用户”。
7. 在“用户名”框中，键入用户的名称。
8. 在 **@** 框中，选择适当的条目。
9. 单击“下一步”箭头。
10. 在向导的“用户配置文件”页中，键入**名字**、**姓氏**和**显示名称**。
11. 在“角色”列表中，选择“用户”。
12. 单击“下一步”箭头。
13. 在“获取临时密码”页上，单击“创建”。
14. 复制**新密码**。
15. 使用新帐户登录到 Azure。 出现提示时更改密码。
16. 使用新帐户登录到 `https://portal.local.azurestack.external`，以查看租户门户。

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>使用 PowerShell 创建 Azure Stack 租户帐户
如果没有 Azure 订阅，则无法使用 Azure 门户添加租户用户帐户。 在这种情况下，可以改用适用于 Windows PowerShell 的 Azure Active Directory 模块。

> [!NOTE]
> 如果使用 Microsoft 帐户 (Live ID) 部署 Azure Stack 开发工具包，则无法使用 AAD PowerShell 创建租户帐户。 
> 
> 

1. 安装[适用于 IT 专业人员 RTW 的 Microsoft Online Services 登录助手](https://www.microsoft.com/en-us/download/details.aspx?id=41950)。
2. 安装[适用于 Windows PowerShell 的 Azure Active Directory 模块（64 位版本）](http://go.microsoft.com/fwlink/p/?linkid=236297)并将其打开。
3. 运行以下 cmdlet：

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. 使用新帐户登录到 Azure。 出现提示时更改密码。
2. 使用新帐户登录到 `https://portal.local.azurestack.external`，以查看租户门户。


