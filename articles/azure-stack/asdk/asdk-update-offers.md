---
title: 本教程介绍如何更新 Azure Stack 产品/服务和计划 | Microsoft Docs
description: 本文介绍如何查看和修改现有的 Azure Stack 产品/服务和计划。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
origin.date: 03/16/2018
ms.date: 03/22/2018
ms.author: v-junlch
ms.reviewer: misainat
ms.openlocfilehash: 5ed064051a99c3b9e71ae07512346440e6f8718a
ms.sourcegitcommit: 61fc3bfb9acd507060eb030de2c79de2376e7dd3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
ms.locfileid: "30155682"
---
# <a name="tutorial-update-offers-and-plans"></a>教程：更新产品/服务和计划
作为 Azure Stack 操作员，你的责任是创建计划，提供适合用户订阅的服务和配额。 这些基本计划包含可以提供给用户的核心服务，每个产品/服务只能有一个基本计划。 如需修改产品/服务，可以通过附加计划的方式来修改计划，扩大最初通过基本计划提供的计算机、存储或网络方面的配额。 

虽然在某些情况下通过单个计划将所有内容组合在一起是最理想的，但大多数情况下，需要先制定一个基本计划，然后以附加计划的方式提供其他服务。 例如，可以通过基本计划提供 IaaS 服务，通过附加计划提供所有 PaaS 服务。 也可通过计划在受限的 ASDK 环境中控制资源的使用。 例如，如果希望用户留意其资源使用情况，可以制定一个相对较小的基本计划（具体取决于所需服务），让系统在用户达到容量限制时通知用户，告知他们已消耗按指定计划分配给他们的资源。 用户于是就会选择可用的附加计划以获取更多资源。 

> [!NOTE]
> 当用户将附加计划添加到现有的产品/服务订阅时，可能需要长达一小时的时间才会显示附加资源。 

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建附加计划 
> * 订阅附加计划

## <a name="create-an-add-on-plan"></a>创建附加计划
修改现有的产品/服务即可创建**附加计划**。

1. 以云管理员身份登录到 [Azure Stack 门户](https://adminportal.local.azurestack.external)。
2. 按照以前用来[创建基本计划](asdk-offer-services.md)的步骤，创建此前尚未提供的全新计划产品服务。 在此示例中，Key Vault (Microsoft.KeyVault) 服务将包括在计划中。
3. 在管理员门户中单击“产品/服务”，然后选择需使用附加计划更新的产品/服务。

   ![](./media/asdk-update-offers/1.PNG)

4.  滚动到产品/服务属性的底部，选择“附加计划”。 单击“添加” 。
   
    ![](./media/asdk-update-offers/2.PNG)

5. 选择要添加的计划（ 在此示例中，该计划称为“密钥保管库计划”），然后单击“选择”，为产品/服务添加该计划。 系统会发送通知，告知你计划已成功添加到产品/服务中。
   
    ![](./media/asdk-update-offers/3.PNG)

6. 查看随产品/服务提供的附加计划的列表，验证新附加计划是否已列出。
   
    ![](./media/asdk-update-offers/4.PNG)

## <a name="subscribe-to-the-add-on-plan"></a>订阅附加计划
若要使用附加计划来扩展现有的 Azure Stack 订阅，需登录到 Azure Stack 用户门户。

请通过以下步骤来发现 Azure Stack 操作员提供的附加资源，然后向以前订阅的产品/服务添加附加计划。

1. 登录到[用户门户](https://portal.local.azurestack.external)。
2. 若要查找需使用附加计划进行扩展的订阅，请单击“更多服务”，然后单击“订阅”并选择你的订阅。
   
    ![](./media/asdk-update-offers/5.PNG)

3.  在订阅概览中，单击“添加计划”。
   
    ![](./media/asdk-update-offers/6.PNG)

4. 在“添加计划”边栏选项卡中，选择要添加到订阅的附加计划。 此示例选择了“密钥保管库计划”。 系统会发送一个通知，告知你：你已成功实施附加计划，但需刷新门户才能访问更新的订阅。
   
    ![](./media/asdk-update-offers/7.PNG)

5. 最后，查看与订阅相关联的附加计划，确保已成功添加该附加计划。
   
    ![](./media/asdk-update-offers/8.PNG)


## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 创建附加计划 
> * 订阅附加计划

> [!div class="nextstepaction"]
> [从模板创建虚拟机](asdk-create-vm-template.md)


