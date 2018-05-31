---
title: 在 Azure Stack 中创建计划 | Microsoft Docs
description: 作为云管理员，创建一个让订阅方预配虚拟机的计划。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 04/23/2018
ms.date: 05/24/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: 5a50556529956e0d9a2828338fa68e8aa849dad3
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34474971"
---
# <a name="create-a-plan-in-azure-stack"></a>在 Azure Stack 中创建计划

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

[计划](azure-stack-key-features.md)是对一个或多个服务的分组。 作为提供商，可以创建提供给用户的计划。 反过来，用户可以订阅产品/服务，以便使用其所包括的计划和服务。 此示例演示如何创建一个包括计算、网络和存储资源提供程序的计划。 订阅方使用此计划可以预配虚拟机。

1. 登录 Azure Stack 管理员门户 (https://adminportal.local.azurestack.external)。

2. 若要创建用户可以订阅的计划和套餐，请单击“新建” > “套餐 + 计划” > “计划”。  
   ![选择计划](./media/azure-stack-create-plan/select-plan.png)

3. 在“新建计划”窗格中，填写“显示名称”和“资源名称”。 显示名称是用户可以看到的计划友好名称。 只有管理员可以看到资源名称，这是管理员将计划作为 Azure 资源管理器资源进行处理时使用的名称。  
   ![指定详细信息](./media/azure-stack-create-plan/plan-name.png)

4. 创建一个新**资源组**或选择一个现有资源组，作为计划的容器。  
   ![指定资源组](./media/azure-stack-create-plan/resource-group.png)

5. 选择“服务”，然后选择与“Microsoft.Compute”、“Microsoft.Network”和“Microsoft.Storage”对应的复选框。 接下来，选择“选择”以保存配置。 将鼠标指针悬停在每个选项上时会显示复选框。  
   ![选择服务](./media/azure-stack-create-plan/services.png)

6. 选择“配额”、“Microsoft.Storage (本地)”，然后选择默认配额，或选择“新建配额”来自定义配额。  
   ![配额](./media/azure-stack-create-plan/quotas.png)

7. 如果要新建配额，请输入配额的**名称** > 指定配额值 > 选择“确定”。 “创建配额”窗格将关闭。
   ![新建配额](./media/azure-stack-create-plan/new-quota.png)

   然后选择创建的新配额。 选择配额会分配它并关闭选择窗格。  
   ![分配配额](./media/azure-stack-create-plan/assign-quota.png)

8. 重复步骤 6 和 7 来为“Microsoft.Network (本地)”和“Microsoft.Compute (本地)”创建并分配配额。  在三项服务全部分配配额后，它们将如下图所示。  
   ![完成的配额分配](./media/azure-stack-create-plan/all-quotas-assigned.png)

9. 在“配额”窗格中，选择“确定”，然后在“新建计划”窗格中，选择“创建”来创建计划。  
    ![创建计划](./media/azure-stack-create-plan/create.png)
10. 若要查看新计划，请选择“所有资源”，然后搜索该计划并选择其名称。 如果资源列表很长，可使用“搜索”来通过名称定位你的计划。  
   ![检查计划](./media/azure-stack-create-plan/plan-overview.png)

### <a name="next-steps"></a>后续步骤
[创建产品/服务](azure-stack-create-offer.md)

<!-- Update_Description: wording update -->