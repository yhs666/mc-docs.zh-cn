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
origin.date: 07/10/2017
ms.date: 03/01/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: 0d60ba35e5065f7cd50651147fd376ea5ebab9b7
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="create-a-plan-in-azure-stack"></a>在 Azure Stack 中创建计划

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

[计划](azure-stack-key-features.md)是对一个或多个服务的分组。 作为提供商，可以创建提供给用户的计划。 反过来，用户可以订阅产品/服务，以便使用其所包括的计划和服务。 此示例演示如何创建一个包括计算、网络和存储资源提供程序的计划。 订阅方使用此计划可以预配虚拟机。

1. 登录到 Azure Stack 管理员门户 (https://adminportal.local.azurestack.external)。 输入在[运行 PowerShell 脚本](azure-stack-run-powershell-script.md)部分的步骤 5 中创建的帐户的凭据。

2. 若要创建用户可以订阅的计划和产品/服务，请单击“新建” > “租户产品/服务 + 计划” > “计划”。

   ![](./media/azure-stack-create-plan/image01.png)
3. 在“新建计划”边栏选项卡中，填写**显示名称**和**资源名称**。 显示名称是用户可看到的计划的友好名称。 只有管理员可以看到资源名称。 管理员使用此名称将计划作为 Azure 资源管理器资源处理。

   ![](./media/azure-stack-create-plan/image02.png)
4. 创建一个新**资源组**或选择一个现有资源组，作为计划的容器。

   ![](./media/azure-stack-create-plan/image02a.png)
5. 单击“服务”，选择 **Microsoft.Compute**、**Microsoft.Network** 和 **Microsoft.Storage**，然后单击“选择”。

   ![](./media/azure-stack-create-plan/image03.png)
6. 依次单击“配额”、“Microsoft.Storage (本地)”，然后选择默认配额或单击“新建配额”自定义配额。

   ![](./media/azure-stack-create-plan/image04.png)
7. 如果要新建配额，请输入配额的名称 > 设置配额值 > 单击“确定”> 单击新配额的名称。

   ![](./media/azure-stack-create-plan/image06.png)
8. 单击“Microsoft.Network (本地)”，然后选择默认配额或单击“新建配额”自定义配额。

    ![](./media/azure-stack-create-plan/image07.png)
9. 如果要新建配额，请键入配额的名称 > 设置配额值 > 单击“确定”> 单击新配额的名称。

    ![](./media/azure-stack-create-plan/image08.png)
10. 单击“Microsoft.Compute (本地)”，然后选择默认配额或单击“新建配额”自定义配额。

    ![](./media/azure-stack-create-plan/image09.png)
11. 如果要新建配额，请键入配额的名称 > 设置配额值 > 单击“确定”> 单击新配额的名称。

    ![](./media/azure-stack-create-plan/image10.png)
12. 在“配额”边栏选项卡中，单击“确定”，然后在“新建计划”边栏选项卡中，单击“创建”以创建计划。

    ![](./media/azure-stack-create-plan/image11.png)
13. 若要查看新计划，请单击“所有资源”，然后搜索该计划并单击其名称。

    ![](./media/azure-stack-create-plan/image12.png)

### <a name="next-steps"></a>后续步骤
[创建产品/服务](azure-stack-create-offer.md)

