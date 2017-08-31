---
title: "Azure Active Direcory 租户目录使用方法概述 | Microsoft Docs"
description: "介绍什么是 Azure AD 租户，以及如何使用 Azure Active Directory 管理 Azure"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 08/21/2017
ms.date: 08/22/2017
ms.author: v-junlch
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.openlocfilehash: 055b4ba498b283a699f1c8df9502690034fac0d5
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="manage-your-azure-ad-directory"></a>管理 Azure AD 目录

## <a name="what-is-an-azure-ad-tenant"></a>什么是 Azure AD 租户？
在 Azure Active Directory (Azure AD) 中，租户是组织在注册 Azure 或 Office 365 之类的 Microsoft 云服务时接收的 Azure AD 目录的专用实例。 每个 Azure AD 目录都是独特的，独立于其他 Azure AD 目录。 就像公司办公大楼是组织特有的安全资产一样，Azure AD 目录也设计为仅供组织使用的安全资产。 Azure AD 体系结构将客户数据和标识信息隔离开来，因此，一个 Azure AD 目录的用户和管理员不可能意外或恶意性地访问另一目录中的数据。

![管理 Azure Active Directory](./media/active-directory-administer/aad_portals.png)

## <a name="how-can-i-get-an-azure-ad-directory"></a>如何获取 Azure AD 目录？
Azure AD 在大多数 Microsoft 云服务的后面提供核心目录和身份管理功能，其中包括：

- Azure
- Microsoft Office 365
- Microsoft Dynamics CRM Online
- Microsoft Intune

注册其中任何一个 Microsoft 云服务便会获得一个 Azure AD 目录。 用户可根据需要创建更多的目录。 例如，可以将第一个目录保留为生产目录，并创建另一个目录进行测试或过渡。

### <a name="using-the-azure-ad-directory-that-comes-with-a-new-azure-subscription"></a>使用新 Azure 订阅附带的 Azure AD 目录

我们建议在注册其他 Microsoft 服务时，使用注册第一个服务时用过的管理员帐户。 首次注册 Microsoft 服务时提供的信息用来为组织创建新的 Azure AD 目录实例。 如果在订阅其他 Microsoft 服务时使用该目录对登录尝试进行身份验证，这些服务可能会使用默认目录中配置的现有用户帐户、策略、设置或本地目录集成。

例如，如果最初注册了 Windows Intune 订阅并通过部署目录同步和/或单一登录服务器完成了将本地 Active Directory 与 Azure AD 目录进一步集成所需的步骤，则可以注册其他 Microsoft 云服务（例如 Office 365），这样也可以利用目前用于 Windows Intune 的目录集成优势。

有关将本地目录与 Azure AD 集成的详细信息，请参阅[目录集成](./connect/active-directory-aadconnect.md)。

### <a name="associate-an-existing-azure-ad-directory-with-a-new-azure-subscription"></a>将现有的 Azure AD 目录与新的 Azure 订阅相关联
可将新的 Azure 订阅与对现有 Office 365 或 Microsoft Intune 订阅的登录进行身份验证的相同目录进行关联。 有关该方案的详细信息，请参阅[在 Azure 中管理 Office 365 订阅的目录](active-directory-how-subscriptions-associated-directory.md#manage-the-directory-for-your-office-365-subscription-in-azure)。

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>通过以组织身份注册 Microsoft 云服务来创建 Azure AD 目录
如果尚未订阅 Microsoft 云服务，可使用以下链接之一进行注册。 注册第一个服务后，会自动创建 Azure AD 目录。

- [Azure](https://account.windowsazure.cn/organization)
- [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
- [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="manage-an-azure-provisioned-default-directory"></a>管理 Azure 设置的默认目录
现在注册 Azure 时，自动创建一个目录，用户的订阅将与该目录相关联。 但是，如果最初是在 2013 年 10 月之前注册 Azure，则不会自动创建一个目录。 在那种情况下，Azure 可能已为帐户进行了“回填”，即为其设置了默认目录。 那么，用户的订阅已与该默认目录相关联。

2013 年 10 月已完成了目录回填，以总体增强 Azure 安全模型。 它有助于向所有 Azure 客户提供组织标识功能，确保了在目录中的用户上下文的目录中可访问所有 Azure 资源。 没有目录就无法使用 Azure。 因此，在 2013 年 7 月 7 日之前注册但没有目录的任何用户都必须创建一个目录。 如果已创建了目录，则订阅已与该目录相关联。

使用 Azure AD 不收取费用。 目录是免费资源。 还有一个额外的单独许可的 Azure Active Directory 高级版级别，它提供额外的功能，例如公司品牌和自助密码重置。

若要更改目录的显示名称，请在门户中单击该目录并单击“配置”。 如本主题后面所述，用户可以添加新目录或删除不再需要的目录。 若要将订阅与其他目录相关联，请在左侧导航窗格中单击“设置”扩展，并在“订阅”页的底部单击“编辑目录”。 还可以使用已注册的 DNS 名称而非默认的 *.partner.onmschina.cn 域来创建自定义域，对于 SharePoint Online 之类的服务这样做可能更好。

## <a name="how-can-i-manage-directory-data"></a>如何管理目录数据
作为一个或多个 Microsoft 云服务订阅的管理员，可以使用 Azure 管理门户、Microsoft Intune 帐户门户或 Office 365 管理中心来管理组织目录数据。 还可以下载并运行[用于 Windows PowerShell 的 Azure Active Directory 模块](https://msdn.microsoft.com/library/azure/jj151815.aspx) cmdlet 来帮助管理 Azure AD 中存储的数据。

通过上述任一门户（或 cmdlet），可以执行以下操作：

- 创建和管理用户帐户和组帐户
- 管理组织订阅的相关云服务
- 设置与目录服务的本地集成

Azure 管理门户、Office 365 管理中心、Microsoft Intune 帐户门户和 Azure AD cmdlet 均从与组织的目录关联的 Azure AD 的单个共享实例读取数据并将数据写入到该实例中，如下图所示。 门户（或 cmdlet）通过这种方式充当用于输入和/或修改目录数据的前端接口。

使用任意门户或 cmdlet 更改组织的数据时，如果此时已在此类服务之一的上下文中登录，则下一次登录时，所做的更改也会显示在其他门户中。 此数据在所订阅的 Microsoft 云服务中共享。

例如，如果使用 Office 365 管理中心阻止某个用户登录，则该操作会阻止该用户登录到组织当前订阅的任何其他服务。 如果在 Microsoft Intune 帐户门户中查看同一用户帐户，则也会看到该用户被阻止。

## <a name="how-can-i-add-and-manage-multiple-directories"></a>如何添加和管理多个目录？
可以在 Azure 管理门户中添加 Azure AD 目录。 在左侧选择“Active Directory”扩展，并单击“添加”。

可将每个目录作为完全独立的资源进行管理：每个目录都是一个具有完整功能的对等方，在逻辑上独立于所管理的其他目录；目录之间不存在父子关系。 目录之间的这种独立性包括资源独立性、管理独立性和同步独立性。

- **资源独立性**。 如果在一个目录中创建或删除了资源，这不影响另一个目录中的任何资源，但对于外部用户来说，情况并非完全如此，具体如下所述。 如果将自定义域“contoso.com”用于一个目录，则不能将它用于任何其他目录。
- **管理独立性**。  如果目录“Contoso”的某个非管理用户创建了测试目录“Test”，那么：
  
  - 目录同步工具，用于将数据与单个 AD 林同步。
  - 目录“Contoso”的管理员对目录“Test”没有直接管理特权，除非“Test”的管理员专门向其授予了这些特权。 “Contoso”的管理员可以控制对目录“Test”的访问，因为他们可以控制创建“Test”的用户帐户。
    
    另外，如果更改（添加或删除）某个用户在一个目录中的管理员角色，这项更改不会影响该用户在另一个目录中可能具有的任何管理员角色。
- **同步独立性**。 可以独立配置每个 Azure AD，以便通过下列任一工具的单个实例同步数据：
  
  - 目录同步工具，用于将数据与单个 AD 林同步
  - 适用于 Forefront Identity Manager 的 Azure Active Directory 连接器，用于将数据与一个或多个本地林和/或非 AD 数据源同步。

另请注意，与其他 Azure 资源不同，目录不是 Azure 订阅的子资源。 因此，如果取消 Azure 订阅或让其过期，则仍可以使用 Azure AD PowerShell、Azure 图形 API 或其他界面（例如 Office 365 管理中心）访问目录数据。 还可以将其他订阅与目录相关联。

## <a name="how-can-i-delete-an-azure-ad-directory"></a>如何删除 Azure AD 目录？
全局管理员可以从门户删除 Azure AD 目录。 删除某个目录时，该目录中包含的所有资源也会被删除；因此，在删除之前，应确认不需要该目录。

> [!NOTE]
> 如果用户使用工作或学校帐户登录，则该用户不得尝试删除其主目录。 例如，如果用户是作为 joe@contoso.partner.onmschina.cn登录的，则该用户不能删除默认域为 contoso.partner.onmschina.cn 的目录。
> 
> 

Azure AD 要求删除目录之前必须符合特定的条件。 这可以降低删除目录后对用户或应用程序造成负面影响（例如，影响用户登录 Office 365 或访问 Azure 中的资源）的风险。 例如，如果用于订阅的某个目录被无意删除，则用户无法访问该订阅的 Azure 资源。

需检查以下情况：

- 该目录中的唯一用户应该是要删除该目录的全局管理员。 只有在删除所有其他用户后，才能删除该目录。 如果用户是从本地同步的，则必须关闭同步，并且必须使用 Azure 门户或 Azure PowerShell cmdlet 从云目录中删除这些用户。 不要求删除组或联系人，例如，从 Office 365 管理中心添加的联系人。
- 目录中不能有任何应用程序。 只有在删除所有应用程序后，才能删除目录。
- 不能有任何多重身份验证提供程序链接到该目录。
- 与目录关联的任何 Microsoft Online Services（例如 Azure、Office 365 或 Azure AD Premium）不能存在任何订阅。 例如，如果在 Azure 中创建了一个默认目录，并且 Azure 订阅仍然依赖于此目录进行身份验证，则不能删除此目录。 类似地，如果其他用户已将订阅与某个目录相关联，则你无法删除该目录。 


## <a name="next-steps"></a>后续步骤
- [Azure AD 论坛](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
- [Azure 多重身份验证论坛](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
- [Stack Overflow for Azure 问题](http://stackoverflow.com/questions/tagged/azure)
- [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory)
- [在 Azure AD 中分配管理员角色](active-directory-assign-admin-roles.md)

<!--Update_Description: wording update -->   
