---
title: 什么是 Azure Active Directory 报告？ | Microsoft Docs
description: 提供 Azure Active Directory 报告的一般概述。
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
origin.date: 01/15/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.openlocfilehash: e6958390efc9856fe10cc5120a7301dc16984c66
ms.sourcegitcommit: b91bbb0f49b12b1a7ac9eaf4742daa0c4a513f11
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876453"
---
# <a name="what-are-azure-active-directory-reports"></a>什么是 Azure Active Directory 报告？

借助 Azure Active Directory 报告，可以深入了解环境的运行情况。  
报表体系结构依赖于活动报表。

![报告](./media/overview-reports/01.png)

## <a name="activity-reports"></a>活动报表

- 审核日志 - 可以通过[审核日志活动报表](concept-audit-logs.md)访问在租户中执行的每个任务的历史记录。
 
    **审核日志报告**为你提供系统活动的符合性记录。使用此数据可以解决常见方案，例如：

    - 我的租户中有人获得了访问管理员组的权限。 谁给予他们访问权限？ 

    - 我想要了解登录到特定应用的用户的列表，因为我最近将该应用上架了，想要了解其是否正常运行

    - 我想要知道在我的租户中进行了多少次密码重置


访问审核日志报表需要什么 Azure AD 许可证？  
审核日志报表适用于你有许可证的功能。 如果有特定功能的许可证，则还可以访问其审核日志信息。

有关更多详细信息，请参阅 [Azure Active Directory 特性和功能](https://www.microsoft.com/cloud-platform/azure-active-directory-features)中的“比较免费版、基本版和高级版中正式推出的功能”。   

## <a name="next-steps"></a>后续步骤


- [审核日志报表](concept-audit-logs.md)


