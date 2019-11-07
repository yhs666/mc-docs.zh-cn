---
author: mmacy
ms.service: active-directory-b2c
ms.topic: include
origin.date: 10/01/2019
ms.date: 10/22/2019
ms.author: v-junlch
ms.openlocfilehash: f7be54c83bedb0e45e573e466c3ac7683d1e39d7
ms.sourcegitcommit: 817faf4e8d15ca212a2f802593d92c4952516ef4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72846911"
---
1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”  筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure Active Directory”（而不是“Azure AD B2C”）。   或者选择“所有服务”，然后搜索并选择“Azure Active Directory”。  
1. 在“管理”  下，选择“应用注册(旧版)” 
1. 选择“新建应用程序注册”  。
1. 输入应用程序的名称。 例如，*managementapp1*。
1. 对于“应用程序类型”，请选择“Web 应用/API”   。
1. 在“登录 URL”中输入任何有效的 URL。  例如，`https://localhost`。 此终结点不需要可访问，但必须是有效的 URL。
1. 选择“创建”  。
1. 记下**已注册的应用**“概述”页上显示的**应用程序 ID**。 在稍后的步骤中会使用此值。

