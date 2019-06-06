---
title: 在 Azure Active Directory 中添加新的 Azure Stack 租户帐户 | Microsoft Docs
description: 部署 Azure Stack 开发工具包后，需要至少创建一个租户用户帐户，以便浏览租户门户。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/12/2019
ms.date: 04/29/2019
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 09/17/2018
ms.openlocfilehash: f89bedadd0510492c4684c8e7a6aec7d6fa7e763
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248738"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>在 Azure Active Directory 中添加新的 Azure Stack 租户帐户

[部署 Azure Stack 开发工具包](../asdk/asdk-install.md)后，需要租户用户帐户，以便浏览租户门户并测试套餐和计划。 可以[使用 Azure 门户](#create-an-azure-stack-tenant-account-using-the-azure-portal)或使用 PowerShell 创建租户帐户。

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>使用 Azure 门户创建 Azure Stack 租户帐户

必须具有 Azure 订阅才能使用 Azure 门户。

1. 登录到 [Azure](https://portal.azure.cn)。
2. 在左侧导航栏中，选择“Active Directory”  并切换到要用于 Azure Stack 的目录，或者创建一个新目录。
3. 选择“Azure Active Directory” > “用户” > “新建用户”。   

    ![“用户 - 所有用户”页，其中突出显示了“新建用户”](media/azure-stack-add-new-user-aad/new-user-all-users.png)

4. 在“用户”  页上，填写所需的信息。

    ![添加新用户，包含用户信息的“用户”页](media/azure-stack-add-new-user-aad/new-user-user.png)

   - **名称（必填）。** 新用户的名字和姓氏。 例如，Mary Parker。
   - **用户名（必填）。** 新用户的用户名。 例如，mary@contoso.com。
       用户名的域名部分必须是初始默认域名 <_yourdomainname_>.partner.onmschina.cn，或者是一个自定义域名，例如 contoso.com。 若要详细了解如何创建自定义域名，请参阅[如何向 Azure Active Directory 添加自定义域名]((/active-directory/fundamentals/add-custom-domain)。
   - **个人资料。** （可选）你可以添加关于用户的详细信息。 也可以在以后添加用户信息。 若要详细了解如何添加用户信息，请参阅[如何添加或更改用户个人资料信息]((/azure/active-directory/fundamentals/active-directory-users-profile-azure-portal)。
   - **目录角色。**  选择“用户”  。

5. 选中“显示密码”  并复制“密码”  框中提供的自动生成的密码。 在初始登录过程中需要此密码。

6. 选择“创建”  。

    此时将创建用户并将其添加到 Azure AD 租户中。

7. 使用新帐户登录到 Azure 门户。 出现提示时更改密码。
8. 使用新帐户登录到 `https://portal.local.azurestack.external`，以查看租户门户。

## <a name="create-an-azure-stack-user-account-using-powershell"></a>使用 PowerShell 创建 Azure Stack 用户帐户

如果没有 Azure 订阅，则无法使用 Azure 门户添加租户用户帐户。 在这种情况下，可以改用适用于 Windows PowerShell 的 Azure Active Directory 模块。

> [!NOTE]
> 如果使用 Microsoft 帐户 (Live ID) 部署 Azure Stack 开发工具包，则无法使用 AAD PowerShell 创建租户帐户。 

1. 安装[适用于 IT 专业人员 RTW 的 Microsoft Online Services 登录助手](https://www.microsoft.com/en-us/download/details.aspx?id=41950)。
2. 安装[适用于 Windows PowerShell 的 Azure Active Directory 模块（64 位版本）](https://go.microsoft.com/fwlink/p/?linkid=236297)并将其打开。
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

## <a name="next-steps"></a>后续步骤

[在 AD FS 中添加 Azure Stack 用户](azure-stack-add-users-adfs.md)