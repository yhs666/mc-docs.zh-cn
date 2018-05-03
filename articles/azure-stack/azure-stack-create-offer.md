---
title: 在 Azure Stack 中创建产品/服务 | Microsoft Docs
description: 作为云管理员，了解如何在 Azure Stack 中为用户创建产品/服务。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 03/27/2018
ms.date: 04/20/2018
ms.author: v-junlch
ms.openlocfilehash: 401239c7df241357d5ea205d3436b56c02b0f330
ms.sourcegitcommit: 85828a2cbfdb58d3ce05c6ef0bc4a24faf4d247b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
# <a name="create-an-offer-in-azure-stack"></a>在 Azure Stack 中创建产品/服务

[产品/服务](azure-stack-key-features.md)是提供者提供给用户购买或订阅的一个或多个计划的组合。 本文档演示如何创建包含在上一步中[创建的计划](azure-stack-create-plan.md)的产品/服务。 订阅方使用此产品/服务可以预配虚拟机。

1. 登录到 Azure Stack 管理员门户 (https://adminportal.local.azurestack.external))，单击“新建” > “租户产品/服务 + 计划” > “产品/服务”。

   ![](./media/azure-stack-create-offer/image01.png)
2. 在“新建产品/服务”窗格中，填写**显示名称**和**资源名称**，然后选择一个新的或现有**资源组**。 显示名称是产品/服务的友好名称。 此友好名称是用户在订阅时可看到的有关该产品/服务的唯一信息。 因此，请务必使用直观名称，以帮助用户了解该产品/服务附带了什么功能。 只有管理员可以看到资源名称。 管理员使用此名称将该产品/服务作为 Azure 资源管理器资源处理。

   ![](./media/azure-stack-create-offer/image01a.png)
3. 单击“基本计划”以打开“计划”窗格，选择要包含该产品/服务的计划，然后单击“选择”。 单击“创建”以创建该产品/服务。

   ![](./media/azure-stack-create-offer/image02.png)
4. 创建产品/服务后，可以更改其状态。 必须将产品/服务设为“公共”，用户才能在订阅时获得完整视图。 产品/服务可以是：
   - **公共**：对用户可见。
   - **专用**：仅对云管理员可见。 在以下情况下有用：起草计划或产品/服务时，或者如果云管理员想要[为用户创建每个订阅](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator)。
   - **已解除授权**：已对新订阅方关闭。 云管理员可以使用已解除授权来防止将来订阅，但不影响当前订阅方。

    > [!TIP]  
    > 产品/服务的更改不会对用户立即可见。 若要查看所做的更改，用户可能需要先注销，然后重新登录到用户门户，这样才能看到新的产品/服务。 

   若要更改产品/服务的状态： 

   - **1803 和更高版本**：  
     在产品/服务的“概述”边栏选项卡上，单击“辅助功能状态”，选择要使用的状态（例如“公共”），然后单击“保存”。 
 
     ![选择辅助功能状态](./media/azure-stack-create-offer/change-state.png) 

     或者，在访问产品/服务之后，可以转到“产品/服务设置”，然后选择“辅助功能状态”以更改状态。 

   - **低于 1803 的版本**：  
     单击“所有资源”，搜索新产品/服务，单击该新产品/服务，单击“更改状态”，然后单击“公共”。

  
   > [!NOTE] 
   > 还可以使用 PowerShell 来创建默认产品/服务、计划和配额。 有关详细信息，请参阅 [Azure Stack 服务管理员自述文件](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin)。
   >


### <a name="next-steps"></a>后续步骤
[创建订阅](azure-stack-subscribe-plan-provision-vm.md)      
[预配虚拟机](azure-stack-provision-vm.md)

<!-- Update_Description: wording update -->