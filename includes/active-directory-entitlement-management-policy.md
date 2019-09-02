---
title: include 文件
description: include 文件
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
origin.date: 05/16/2019
ms.date: 08/09/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 086eb3b379b351c7fad596b650381a3bb95c13a0
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68972857"
---
### <a name="policy-for-users-in-your-directory"></a>策略:适用于目录中的用户

如果你希望将策略应用于目录中可请求此访问包的用户和组，请执行以下步骤。

1. 在“可以请求访问权限的用户”部分，选择“你目录中的用户”。  

1. 在“选择用户和组”部分，单击“添加用户和组”。  

1. 在“选择用户和组”窗格中，选择要添加的用户和组。

    ![访问包 - 策略 - 选择用户和组](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. 单击“选择”以添加用户和组。 

1. 向下跳转到“策略:[](#policy-request)请求”部分。

### <a name="policy-for-users-not-in-your-directory"></a>策略:适用于不在目录中的用户

如果你希望将策略应用于不是目录中可请求此访问包的用户和组，请执行以下步骤。 必须在“组织关系协作限制”设置中配置要允许的目录。 

> [!NOTE]
> 将为不是目录中的其请求已审批或自动审批的用户创建来宾用户帐户。 将邀请来宾，但他们不会收到邀请电子邮件。 传递其访问包分配时，他们将收到电子邮件。 默认情况下，当来宾用户不再有任何访问包分配时（因为他们的上次分配已过期或已取消），将会阻止该来宾用户帐户登录并将其删除。 如果希望无限期地在目录中保留来宾用户（即使他们没有任何访问包分配），可以更改权利管理配置的设置。

1. 在“可以请求访问权限的用户”部分，选择“不在你目录中的用户”。  

1. 在“选择外部 Azure AD 目录”部分，单击“添加目录”。  

1. 输入域名并搜索外部 Azure AD 目录。

1. 根据提供的目录名称和初始域验证该目录是否正确。

    > [!NOTE]
    > 该目录中的所有用户都可请求此访问包。 这包括与该目录关联的所有子域（而不仅仅是搜索中使用的域）中的用户。

    ![访问包 - 策略 - 选择目录](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. 单击“添加”以添加该目录。 

1. 重复此步骤以添加更多目录。

1. 添加要包括在策略中的所有目录后，单击“选择”。 

1. 向下跳转到“策略:[](#policy-request)请求”部分。

### <a name="policy-none-administrator-direct-assignments-only"></a>策略:无(仅限管理员直接分配)

如果希望策略绕过访问请求，并允许管理员直接将特定用户分配到访问包，请执行这些步骤。 用户无需请求访问包。 你仍可以设置过期设置，但没有请求设置。

1. 在“可请求访问权限的用户”部分，选择“无(仅限管理员直接分配”。  

    创建访问包后，可以直接将特定的内部和外部用户分配到该访问包。 如果指定外部用户，将在目录中创建来宾用户帐户。

1. 向下跳转到“策略:[](#policy-expiration)过期时间”部分。

### <a name="policy-request"></a>策略:请求

在“请求”部分，指定在用户请求访问包时应用的审批设置。

1. 若要要求对所选用户发起的请求进行审批，请将“需要审批”切换开关设置为“是”。   若要自动审批请求，请将切换开关设置为“否”。 

1. 如果要求审批，请在“选择审批者”部分单击“添加审批者”。  

1. 在“选择审批者”窗格中，选择一个或多个用户和/或组作为审批者。

    只有其中一个选定的审批者需要审批请求。 不需要所有审批者审批。 审批决定以第一个评审请求的审批者为准。

    ![访问包 - 策略 - 选择审批者](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. 单击“选择”以添加审批者。 

1. 单击“显示高级请求设置”以显示其他设置。 

    ![访问包 - 策略 - 选择目录](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. 若要要求用户提供请求访问包的理由，请将“需要理由”设置为“是”。  

1. 若要要求审批者提供访问包请求审批结果的理由，请将“需要审批者的理由”设置为“是”。  

1. 在“批准请求超时(天)”框中，指定审批者必须评审请求的时限。  如果在此天数内没有任何审批者评审请求，该请求将会过期，而用户必须对访问包再次提交请求。

### <a name="policy-expiration"></a>策略:过期时间

在“过期时间”部分，指定用户的访问包分配何时过期。

1. 在“过期时间”部分，将“访问包过期时间”设置为“日期”、“天数”或“永不”。     

    对于“日期”，请选择将来的过期日期。 

    对于“天数”，请指定 0 到 3660 天的数字。 

    根据所做的选择，用户的访问包分配将在特定的日期过期、审批后经过特定的天数之后过期，或者永不过期。

1. 单击“显示高级过期时间设置”以显示其他设置。 

1. 若要允许用户延期其分配，请将“允许用户延期访问权限”设置为“是”。  

    如果策略中允许延期，在将用户的访问包分配设置为过期之前的 14 天以及 1 天，用户会收到一封电子邮件，其中提示他们是否要延期分配。

    ![访问包 - 策略 - 过期时间设置](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>策略:启用策略

1. 若要立即将访问包提供给策略中的用户使用，请单击“是”以启用策略。 

    创建完访问包后，将来始终可以启用该策略。

    ![访问包 - 策略 - 启用策略设置](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. 单击“下一步”或“创建”。  

