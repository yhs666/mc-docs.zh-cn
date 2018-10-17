---
title: 如何将自定义域添加到 Azure Active Directory | Microsoft Docs
description: 了解如何使用 Azure Active Directory 门户添加自定义域。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
origin.date: 09/10/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: c1ad0ca44e50781176d1b7ca828504e21ecf2792
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913765"
---
# <a name="how-to-add-your-custom-domain-name-using-the-azure-active-directory-portal"></a>如何：使用 Azure Active Directory 门户添加自定义域名
每个新的 Azure AD 租户都附带了一个初始域名 *domainname*.partner.onmschina.cn。 无法更改或删除初始域名，但可以将组织的名称添加到列表中。 添加自定义域名有助于创建用户所熟悉的用户名，例如 _alain@contoso.com_。

>[!Note]
>你必须为每个自定义的域名从头到尾重复这整个流程。

## <a name="add-a-custom-domain-name"></a>添加自定义域名
首先，必须将自定义域名添加到 Azure AD 租户。

### <a name="to-add-a-custom-domain-name"></a>添加自定义域名
1. 使用目录的全局管理员帐户登录到 [Azure AD 门户](https://portal.azure.cn/)。

2. 依次选择“Azure Active Directory”、“自定义域名”和“添加自定义域名”。

    ![“Fabrikam - 自定义域名”边栏选项卡，其中突出显示了“添加自定义域”选项](./media/add-custom-domain/add-custom-domain.png)

3. 在“自定义域名”框中键入你的公司域名（例如 _contoso.com_），然后选择“添加域”。

    >[!Important]
    >若要正常完成此过程，必须包含 .com、.net 或其他任何顶级扩展名。

    ![“Fabrikam - 自定义域名”边栏选项卡，其中突出显示了“添加域”按钮](./media/add-custom-domain/add-custom-domain-blade.png)

4. 从 **Contoso** 边栏选项卡复制 DNS 条目信息。

    ![包含 DNS 条目信息的 Contoso 边栏选项卡](./media/add-custom-domain/contoso-blade-with-dns-info.png)

## <a name="add-your-domain-name-with-a-domain-name-registrar"></a>向域名注册机构添加你的域名
接下来，必须为新的自定义域更新 DNS 区域文件。 可以使用你的 Azure、Office 365 或外部 DNS 记录的 DNS 区域，也可以使用不同的 DNS 注册机构（例如 [InterNIC](https://go.microsoft.com/fwlink/p/?LinkId=402770)）添加新的 DNS 条目。

### <a name="to-add-your-domain-name"></a>添加域名 
1. 登录到自定义域的域名注册机构。 如果你没有合适的权限来更新条目，则需要联系具有这些权限的人员。

2. 向注册机构更新 DNS 条目后，必须使用 Azure AD 提供的信息更新 DNS 区域文件。

    >[!Note]
    >DNS 条目不会更改邮件路由或 Web 托管的工作方式。

## <a name="verify-your-custom-domain-name"></a>验证自定义域名
注册自定义域名后，可能需要花费几秒到几小时，然后 DNS 信息才会传播到 Azure AD 可以看到它处于有效状态的地方。

### <a name="to-verify-your-custom-domain-name"></a>验证自定义域名
1. 使用目录的全局管理员帐户登录到 [Azure AD 门户](https://portal.azure.cn/)。

2. 依次选择“Azure Active Directory”、“自定义域名”。

3. 在“Fabrikam - 自定义域名”边栏选项卡上，选择自定义域名 **Contoso**。

    ![“Fabrikam - 自定义域名”边栏选项卡，其中突出显示了“Contoso”](./media/add-custom-domain/custom-blade-with-contoso-highlighted.png)

4. 在 **Contoso** 边栏选项卡上，选择“验证”以确保自定义域已正确注册并且在 Azure AD 中有效。

    ![包含 DNS 条目信息和“验证”按钮的“Contoso”边栏选项卡](./media/add-custom-domain/contoso-blade-with-dns-info-verify.png)

### <a name="common-verification-issues"></a>常见验证问题
如果 Azure AD 无法验证自定义域名，请尝试以下建议的方法：
- **至少等待一小时，然后重试**。 只有在传播 DNS 记录之后，Azure AD 才能验证域，而此过程可能需要一小时或更长时间。

- **确保 DNS 记录正确。** 返回到域名注册机构站点，确保其中包含该条目，并且该条目与 Azure AD 提供的 DNS 条目信息相匹配。

    如果无法在注册机构站点上更新记录，必须与有权添加条目并验证其准确性的某人共享该条目。

- **确保域名尚未在其他目录中。** 只能在一个目录中验证一个域名，这意味着，如果当前在另一目录中验证你的域名，则无法同时在新目录中验证该域名。 若要解决此重复问题，必须从旧目录中删除该域名。 有关删除域名的详细信息，请参阅[管理自定义域名](../users-groups-roles/domains-manage.md)。    

## <a name="next-steps"></a>后续步骤
- 将用户添加到域，请参阅[管理自定义域名](../users-groups-roles/domains-manage.md)。

- 若要结合 Azure Active Directory 使用 Windows Server 的本地版本，请参阅[将本地目录与 Azure Active Directory 集成](../connect/active-directory-aadconnect.md)。

<!-- Update_Description: wording update -->