---
title: "如何在 Azure AD 中管理联合证书 | Microsoft Docs"
description: "了解如何自定义联合证书的过期日期，以及如何续订即将过期的证书。"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/09/2017
ms.date: 07/18/2017
ms.author: v-junlch
ms.openlocfilehash: a396cb784d9858e2da61c14e6eaa152e662fc09c
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>在 Azure Active Directory 中管理用于联合单一登录的证书
本文包含 Azure Active Directory 为了与 SaaS 应用程序建立联合单一登录 (SSO) 而创建的证书的相关常见问题。

本文只与配置为使用 **Azure AD 单一登录**的应用相关，如以下示例中所示：

![Azure AD 单一登录](./media/active-directory-sso-certs/fed-sso.PNG)

## <a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>如何自定义联合证书的过期日期
默认情况下，证书设置为两年后过期。 可以遵循以下步骤为证书选择不同的过期日期。 包含的屏幕截图使用 Salesforce 作为示例，但这些步骤也适用于任何联合 SaaS 应用。

1. 在 Azure Active Directory 中应用程序的“快速启动”页上，单击“配置单一登录”。
   
    ![打开 SSO 配置向导。](./media/active-directory-sso-certs/config-sso.png)
2. 选择“Azure AD 单一登录”，然后单击“下一步”。
3. 输入应用程序的“登录 URL”，并选中“配置用于联合单一登录的证书”复选框。 然后单击“下一步”。
   
    ![Azure AD 单一登录](./media/active-directory-sso-certs/new-app-config-sso.PNG)
4. 在下一页上，选择“生成新证书”，并选择希望证书保持有效的期限。 然后单击“下一步”。
   
    ![生成新证书](./media/active-directory-sso-certs/new-app-config-cert.PNG)
5. 接下来，单击“下载证书”。 若要了解如何将证书上传到特定的 SaaS 应用，请单击“查看配置说明”。
   
    ![下载然后上传证书](./media/active-directory-sso-certs/new-app-config-app.PNG)
6. 只有选中了对话框底部的确认复选框并按“提交”之后，才会启用该证书。

## <a name="how-to-renew-a-certificate-that-will-soon-expire"></a>如何续订即将过期的证书
下面显示的续订步骤在理想情况下不会给用户造成任何严重的停机。 本部分中使用的屏幕截图采用 Salesforce 作为示例，但这些步骤也适用于任何联合 SaaS 应用。

1. 在 Azure Active Directory 中应用程序的“快速启动”页上，单击“配置单一登录”。
   
    ![打开 SSO 配置向导](./media/active-directory-sso-certs/renew-sso-button.PNG)
2. 对话框的第一页中应已选择了“Azure AD 单一登录”，因此请单击“下一步”。
3. 在第二页上，选中“配置用于联合单一登录的证书”复选框。 然后单击“下一步”。
   
    ![Azure AD 单一登录](./media/active-directory-sso-certs/renew-config-sso.PNG)
4. 在下一页上，选择“生成新证书”，并选择希望新证书保持有效的期限。 然后单击“下一步”。
   
    ![生成新证书](./media/active-directory-sso-certs/new-app-config-cert.PNG)
5. 单击“下载证书”。 若要成功续订证书，必须执行以下两个步骤：
   
   - 将新证书上传到 SaaS 应用的单一登录配置屏幕。 若要了解如何对特定 SaaS 应用执行此操作，请单击“查看配置说明”。
   - 在 Azure AD 中，选中对话框底部的确认复选框以启用新证书，然后单击“下一步”以提交。
     
     > [!IMPORTANT]
     > 完成这两个步骤中的任何一个时，会禁用单一登录到应用，但完成第二个步骤后会再次启用。 因此，为了最大程度地减少停机时间，请准备好在较短的间隔时间内完成这两个步骤。
     > 
     > 
     
     ![下载然后上传证书](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>相关文章
- [有关 Azure Active Directory 中应用程序管理的文章索引](active-directory-apps-index.md)
- [Azure Active Directory 的应用程序访问与单一登录](active-directory-appssoaccess-whatis.md)
- [排查基于 SAML 的单一登录问题](./develop/active-directory-saml-debugging.md)

<!--Update_Description: wording update -->