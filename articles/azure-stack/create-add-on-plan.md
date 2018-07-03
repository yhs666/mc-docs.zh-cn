---
title: 如何更新 Azure Stack 套餐和计划 | Microsoft Docs
description: 本文介绍如何查看和修改现有的 Azure Stack 套餐和计划。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
origin.date: 06/07/2018
ms.date: 06/27/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: 3a7e35f28a5dfe669901a10340585fca16144129
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027270"
---
# <a name="azure-stack-add-on-plans"></a>Azure Stack 附加计划
作为 Azure Stack 操作员，你的责任是创建计划，提供适合用户订阅的服务和配额。 这些[基本计划](azure-stack-create-plan.md)包含可以提供给用户的核心服务，每个套餐只能有一个基本计划。 如需修改套餐，可以通过附加计划的方式来修改计划，扩大最初通过基本计划提供的计算机、存储或网络方面的配额。 

虽然在某些情况下通过单个计划将所有内容组合在一起是最理想的，但大多数情况下，需要先制定一个基本计划，然后以附加计划的方式提供其他服务。 例如，可以通过基本计划提供 IaaS 服务，通过附加计划提供所有 PaaS 服务。 也可通过计划在 Azure Stack 环境中控制资源的使用。 例如，如果希望用户留意其资源使用情况，可以制定一个相对较小的基本计划（具体取决于所需服务），让系统在用户达到容量限制时通知用户，告知他们已消耗按指定计划分配给他们的资源。 用户于是就会选择可用的附加计划以获取更多资源。 

> [!NOTE]
> 当用户将附加计划添加到现有的套餐订阅时，可能需要长达一小时的时间才会显示附加资源。 

## <a name="create-an-add-on-plan"></a>创建附加计划
修改现有的套餐即可创建附加计划：

1. 以云管理员身份登录到 Azure Stack 管理员门户。
2. 遵循[创建基本计划](azure-stack-create-plan.md)的步骤，创建此前尚未提供的新计划套餐服务。 在此示例中，Key Vault (Microsoft.KeyVault) 服务将包含在新计划中。
3. 在管理员门户中单击“套餐”，然后选择需使用附加计划更新的套餐。

   ![](./media/create-add-on-plan/1.PNG)

4.  滚动到套餐属性的底部，选择“附加计划”。 单击“添加” 。
   
    ![](./media/create-add-on-plan/2.PNG)

5. 选择要添加的计划（ 在此示例中，该计划称为“密钥保管库计划”），然后单击“选择”，为套餐添加该计划。 系统会发送通知，告知你计划已成功添加到套餐中。
   
    ![](./media/create-add-on-plan/3.PNG)

6. 查看随套餐提供的附加计划的列表，验证新附加计划是否已列出。
   
    ![](./media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>后续步骤

  [创建产品/服务](azure-stack-create-offer.md)

