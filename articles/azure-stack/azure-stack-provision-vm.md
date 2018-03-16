---
title: "在 Azure Stack 中创建测试 VM | Microsoft Docs"
description: "了解如何在 Azure Stack 中以云操作员身份预配测试 VM。"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 09/25/2017
ms.date: 03/02/2018
ms.author: v-junlch
ms.reviewer: 
ms.openlocfilehash: 7d5e756b0cc9d1f85099bdd7721966ae884b2ff8
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>在 Azure Stack 中创建测试虚拟机

*适用于：Azure Stack 开发工具包*

作为 Azure Stack 操作员，可以创建测试虚拟机以验证 [Azure Stack](azure-stack-poc.md) 开发人员工具包部署。

> [!NOTE]
> 预配虚拟机之前，必须先[将 Windows Server 2016 Evaluation 映像添加到 Azure Stack Marketplace](azure-stack-add-default-image.md)。
> 
> 

## <a name="create-a-virtual-machine"></a>创建虚拟机
1. 在 Azure Stack 开发工具包主机上，[登录](azure-stack-connect-azure-stack.md)到管理员门户 (`https://adminportal.local.azurestack.external`)，然后单击“新建” > “计算” > “Windows Server 2016 Datacenter 评估版” > “创建”。  
2. 在“基本信息”边栏选项卡中，键入**名称**、**用户名**和**密码**。 选择“订阅”。 创建一个**资源组**或选择现有资源组，然后单击“确定”。  
3. 在“选择大小”边栏选项卡中，单击“A1 标准”，并单击“选择”。  
4. 在“设置”边栏选项卡中接受默认值，然后单击“确定”
5. 在“摘要”边栏选项卡中，单击“确定”创建虚拟机。  
6. 若要查看新虚拟机，请单击“所有资源”，然后搜索该虚拟机并单击其名称。
    ![](./media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>后续步骤
[在 Azure Stack 中使用管理员门户和用户门户](azure-stack-manage-portals.md)

