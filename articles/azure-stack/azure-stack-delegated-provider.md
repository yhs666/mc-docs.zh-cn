---
title: 在 Azure Stack 中委托产品/服务 | Microsoft Docs
description: 了解如何委托他人来管理创建产品/服务以及为你注册用户的事情。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/28/2018
ms.date: 03/22/2018
ms.author: v-junlch
ms.reviewer: alfredop
ms.openlocfilehash: 961a659df9a20d5898f8991b7545d403e5a86a43
ms.sourcegitcommit: 61fc3bfb9acd507060eb030de2c79de2376e7dd3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
ms.locfileid: "30155447"
---
# <a name="delegate-offers-in-azure-stack"></a>在 Azure Stack 中委托产品/服务

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

作为 Azure Stack 操作员，你经常需要委托他人来管理创建产品/服务以及注册用户的事情。 例如，服务提供商可能需要经销商来代表他们注册和管理客户。 或者，如果你是企业的中心 IT 小组的成员，则可能需要下属公司在你不参与的情况下注册用户。

可以通过委托来接触和管理更多的用户（与直接接触和管理相比），这有助于完成相关任务。 下图演示了一个级别的委托，但 Azure Stack 支持多个级别。 委托的提供商 (DP) 可以反过来委托其他提供商，最多可以有五个级别。

![委托级别](./media/azure-stack-delegated-provider/image1.png)

Azure Stack 操作员可以使用委托功能将创建产品/服务和用户的任务委托给其他用户。

## <a name="roles-and-steps-in-delegation"></a>委托的角色和步骤
在了解委托时，请记住，一共涉及到三个角色：

- *Azure Stack 操作员*管理 Azure Stack 基础结构、创建产品/服务模板，以及委托他人将其提供给他们的用户。

- 受委托的 Azure Stack 操作员称为委托的提供商。 他们可能属于其他组织，例如其他 Azure Active Directory (Azure AD) 用户。

- 用户可以注册并使用产品/服务来管理其工作负荷、创建 VM、存储数据，等等。

如下图所示，可以通过两个步骤来设置委托。

1. 确定委托的提供商。 为此，可以让他们根据计划订阅一项产品/服务，该计划只包含订阅服务。 订阅此产品/服务的用户可以获得 Azure Stack 操作员的部分权限，包括能够扩展产品/服务以及为其注册用户。

2. 将产品/服务委托给受委托的提供商。 此产品/服务充当委托的提供商可以提供的东西的模板。 委托的提供商现在可以接受该产品/服务。 为其选择一个名称（但请勿更改其服务和配额），然后将其提供给客户。

![创建委托的提供商，并允许其注册用户](./media/azure-stack-delegated-provider/image2.png)

用户需要与主提供商建立关系才能充当委托的提供商。 换言之，他们需要创建订阅。 在这种情况下，此订阅表明委托的提供商有权代表主提供商提供产品/服务。

建立这种关系以后，Azure Stack 操作员就可以将产品/服务委托给受委托的提供商。 委托的提供商现在可以接受该产品/服务，将其重命名（但不更改其实质）并提供给客户。

以下部分介绍如何建立委托的提供商，委托一项产品/服务，然后验证用户是否可以注册该产品/服务。

## <a name="set-up-roles"></a>设置角色

如果要查看委托的提供商如何工作，则除了 Azure Stack 操作员帐户，还需要其他 Azure AD 帐户。 如果没有这两个帐户，请创建。 这些帐户可以属于任何 Azure AD 用户，称为委托的提供商和用户。

| **角色** | **组织权限** |
| --- | --- |
| 委托的提供商 |User |
| User |User |

## <a name="identify-the-delegated-providers"></a>确定委托的提供商
1. 以 Azure Stack 操作员身份登录。

2. 创建使用户成为委托的提供商的产品/服务：
   
   a.  [创建计划](azure-stack-create-plan.md)。
       此计划应该只包括订阅服务。 本文使用名为 **PlanForDelegation** 的计划。
   
   b.  根据此计划[创建产品/服务](azure-stack-create-offer.md)。 本文使用名为 **OfferToDP** 的产品/服务。
   
   c.  创建完产品/服务以后，将委托的提供商添加为此产品/服务的订户。 为此，请选择“订阅” > “添加” > “新建租户订阅”。
   
   ![将委托的提供商添加为订户](./media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> 对于所有 Azure Stack 产品/服务，可以选择将其公开给用户注册，也可以选择不公开，让 Azure Stack 操作员管理注册。 委托的提供商通常是一个小的组。 需要控制其成员，因此大多数情况下不应公开此产品/服务。
> 
> 

## <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure Stack 操作员创建委托的产品/服务

现在已建立委托的提供商。 下一步是创建要委托的可供客户使用的计划和产品/服务。 最好是将此产品/服务准确地定义成想要客户看到的那种规格，因为委托的提供商不能更改该产品/服务中包括的计划和配额。

1. 以 Azure Stack 操作员的身份[创建计划](azure-stack-create-plan.md)以及基于该计划的[产品/服务](azure-stack-create-offer.md)。 本文使用名为 **DelegatedOffer** 的产品/服务。
   
   > [!NOTE]
   > 此产品/服务不必是公共的。 如果进行选择，可以选择将其公开。 但大多数情况下，只需向委托的提供商提供其访问权限。 在你按照以下步骤中的说明委托专用产品/服务以后，委托的提供商即可对其进行访问。
   > 
   > 
1. 委托产品/服务。 转到“DelegatedOffer”。 然后，在“设置”窗格中选择“委托的提供商” > “添加”。

2. 从下拉列表框中选择委托的提供商的订阅，然后选择“委托”。

> ![添加委托的提供商](./media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>委托的提供商自定义产品/服务

以委托的提供商身份登录到用户门户，然后使用委托的产品/服务作为模板来创建新的产品/服务。

1. 选择“新建” > “租户产品/服务 + 计划” > “产品/服务”。

    ![创建新的产品/服务](./media/azure-stack-delegated-provider/image5.png)


1. 为产品/服务指定一个名称。 本文使用 **ResellerOffer**。 选择委托的产品/服务作为模板，然后选择“创建”。
   
   ![指定名称](./media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > 请注意此过程与 Azure Stack 操作员体验到的产品/服务创建过程的差异。 委托的提供商并不根据基础计划和附加计划来构造产品/服务。 他们只能从委托给自己的产品/服务中进行选择，不能对这些产品/服务进行更改。

1. 让产品/服务公开，方法是：选择“浏览”，然后选择“产品/服务”。 选择产品/服务，然后选择“更改状态”。

2. 委托的提供商通过自己的 URL 公开这些产品/服务。 这些产品/服务只能通过委托的门户来查看。 若要查找和更改此 URL，请执行以下操作：
   
    a.  选择“浏览” > “更多服务” > “订阅”。 然后选择委托的提供商的订阅。 例如“DPSubscription” > “属性”。
   
    b.  将门户 URL 复制到单独的位置，例如记事本。
   
    ![选择委托的提供商的订阅](./media/azure-stack-delegated-provider/dpportaluri.png)  
   
   现在已经以委托的提供商的身份创建了一项委托的产品/服务。 以委托的提供商的身份注销。 关闭一直在使用的浏览器窗口。

## <a name="sign-up-for-the-offer"></a>注册产品/服务
1. 在新的浏览器窗口中，转到上一步保存的委托的门户 URL。 以用户身份登录到门户。 
   
   >[!NOTE]
   >除非使用委托的门户，否则委托的产品/服务不可见。 

2. 在仪表板中，选择“获取订阅”。 可以看到，只向用户提供由委托的提供商创建的委托的产品/服务：

> ![查看和选择产品/服务](./media/azure-stack-delegated-provider/image8.png)
> 
> 

产品/服务委托过程已完成。 用户现在可以注册此产品/服务，方法是获取其订阅。

## <a name="multiple-tier-delegation"></a>多层委托

委托的提供商可以通过多层委托将产品/服务委托给其他实体。 这样可以实现很多目的，例如，可以开拓更深层的经销商渠道，以便管理 Azure Stack 的提供商将产品/服务委托给分销商。 该分销商又可以对经销商进行委托。 Azure Stack 支持最多五个级别的委托。

在创建多层产品/服务委托时，委托的提供商又会将产品/服务委托给下一提供商。 委托的提供商的此过程与 Azure Stack 操作员的此过程相同（请参阅 [Azure Stack 操作员创建委托的产品/服务](#cloud-operator-creates-the-delegated-offer)）。



<!-- Update_Description: wording update -->