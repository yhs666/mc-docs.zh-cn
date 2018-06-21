---
title: 在 Azure Stack 中订阅产品/服务 | Microsoft Docs
description: 在 Azure Stack 中创建产品/服务的订阅
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 02/06/2018
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: 9d5d5117947e1c5f70a275d7c57ddc333897a377
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
ms.locfileid: "29798112"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>在 Azure Stack 中创建产品/服务的订阅

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

[创建某个产品/服务](azure-stack-create-offer.md)后，用户需要订阅此产品/服务才能使用此产品/服务。   
- 云操作员可以从管理员门户内部为用户创建订阅。  创建的订阅可用于公共和专用产品/服务。
- 租户用户可以在使用用户门户时订阅公共产品/服务。  

以下部分提供有关云操作员如何为用户创建订阅的指导，以及如何以用户身份订阅产品/服务。

## <a name="create-a-subscription-as-a-cloud-operator"></a>以云操作员的身份创建订阅
云操作员可使用管理员门户为用户创建产品/服务订阅。  你可以为自己的目录租户成员创建订阅。  如果已启用[多租户](azure-stack-enable-multitenancy.md)，则还可以为其他目录租户中的用户创建订阅。

可为公共和专用产品/服务创建订阅。  如果不希望租户创建他们自己的订阅，可将所有产品/服务设置为专用，然后代表租户创建订阅。 将 Azure Stack 与外部计帐系统或服务目录系统集成时，通常会使用此方法。

为某个用户创建订阅后，该用户可以登录到用户门户，然后会发现他们已订阅产品/服务。  

### <a name="to-create-the-subscription-for-a-user"></a>为用户创建订阅
1. 在管理门户中，转到“用户订阅”。
2. 选择“添加”打开“新建用户订阅”窗格。 在此处指定以下详细信息：  

    a. **显示名称** – 用于标识订阅的友好名称，显示为“用户订阅名称”。

    b. **用户** – 从此订阅的可用目录租户中指定一个用户。 用户名显示为“所有者”。  用户名的格式取决于标识解决方案。 例如：   

      -  **Azure AD：***&lt;user1>@&lt;contoso.partner.onmschina.cn>*

      -  **AD FS：***&lt;user1>@&lt;azurestack.local>*     

    c.  **目录租户** – 选择用户帐户所属的目录租户。 如果未启用多租户，则只能使用本地目录租户。

3. 选择“产品/服务”打开“产品/服务”窗格，然后为此订阅选择一个**产品/服务**。 由于是为用户创建订阅，因此可以选择专用或公共产品/服务。

4. 选择“创建”以创建订阅。 “用户订阅”窗格中随即显示新的订阅。  以后当用户登录到用户门户时，他们可以查看有关此订阅的详细信息。

### <a name="to-make-an-add-on-plan-available"></a>提供附加计划  
云操作员随时可将附加计划添加到以前创建的订阅：   
1. 在管理门户中，转到“更多服务” > “用户订阅”，然后选择要更改的订阅。   

2. 选择“附加” > “添加”打开“添加计划”窗格。  

3. 选择要添加为附加计划的计划。  然后，控制台会显示与订阅关联的计划。




## <a name="create-a-subscription-as-a-user"></a>以用户的身份创建订阅
以用户身份登录到用户门户后，可在目录租户（组织）中查找并订阅公共产品/服务和附加计划。 如果 Azure Stack 环境支持[多租户](azure-stack-enable-multitenancy.md)，则可以从远程目录租户订阅产品/服务。

### <a name="to-subscribe-to-an-offer"></a>订阅产品/服务
1. [登录](azure-stack-connect-azure-stack.md)到 Azure Stack 用户门户 ( https://portal.local.azurestack.external )，并单击“获取订阅”。

   ![获取订阅](./media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. 在“显示名称”字段中键入订阅的名称，单击“产品/服务”，单击“选择产品/服务”窗格中的某个产品/服务，然后单击“创建”。

   ![创建产品/服务](./media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. 若要查看创建的订阅，请单击“更多服务”、单击“订阅”，然后单击新订阅。  

订阅产品/服务之后，请刷新门户以查看哪些服务是新订阅的一部分。

### <a name="to-subscribe-to-an-add-on-plan"></a>订阅附加计划
如果产品/服务有附加计划，随时可将该计划添加到订阅。  

1. 在用户门户中，选择“更多服务” > “订阅”，然后选择要更改的订阅。

2. 选择“添加计划”按钮，然后选择所需的附加计划。 如果“添加计划”不可用，则表示与此订阅关联的产品/服务没有附加计划。



## <a name="next-steps"></a>后续步骤
[部署虚拟机](azure-stack-provision-vm.md)

