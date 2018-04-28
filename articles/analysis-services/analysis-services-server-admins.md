---
title: 管理 Azure Analysis Services 中的服务器管理员 | Azure
description: 了解如何在 Azure 中管理 Analysis Services 服务器的服务器管理员。
author: rockboyfor
manager: digimobile
ms.service: analysis-services
ms.topic: conceptual
origin.date: 04/12/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: ba0b654db3ee787367f09d658ae96625d809d52a
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="manage-server-administrators"></a>管理服务器管理员
服务器管理员必须是 Azure Active Directory (Azure AD) 中服务器所在租户的有效用户或组。 可以对 Azure 门户中的服务器使用“Analysis Services 管理员”，或使用 SSMS 中的“服务器属性”来管理服务器管理员。 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>使用 Azure 门户添加服务器管理员
1. 在门户中，对于服务器，单击“Analysis Services 管理员”。
2. 在“\<servername> - Analysis Services 管理员”中，单击“添加”。
3. 在“添加服务器管理员”中，从 Azure AD 选择用户帐户，或使用电子邮件地址邀请外部用户。

    ![Azure 门户中的服务器管理员](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>使用 SSMS 添加服务器管理员
1. 右键单击服务器 >“属性”。
2. 在“Analysis Server 属性”中，单击“安全性”。
3. 单击“添加”，然后输入 Azure AD 中用户或组的电子邮件地址。

    ![在 SSMS 中添加服务器管理员](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>后续步骤 
[身份验证和用户权限](analysis-services-manage-users.md)  
[管理数据库角色和用户](analysis-services-database-users.md)  
[基于角色的访问控制](https://docs.microsoft.com/zh-cn/azure/role-based-access-control/overview)

<!--Update_Description: update meta properties, wording update -->