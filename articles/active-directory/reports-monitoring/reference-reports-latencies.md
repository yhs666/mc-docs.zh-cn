---
title: Azure Active Directory 报告延迟 | Microsoft Docs
description: 了解在 Azure 门户中显示报告事件所花费的时间
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
origin.date: 11/13/2018
ms.date: 12/06/2018
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.openlocfilehash: 59682da6ba745974b100e2c4e285ed87231b0fbb
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028773"
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory 报告延迟

延迟是 Azure Active Directory (Azure AD) 报告数据在 [Azure 门户](https://portal.azure.cn)中显示所需的时间。  

## <a name="activity-reports"></a>活动报表

- [审核日志](concept-audit-logs.md) - 提供有关用户和组、托管应用程序和目录活动的系统活动信息

下表列出了活动报表的延迟信息。 

> [!NOTE]
> **延迟 (95%)** 是指报告 95% 的日志所用的时间，**延迟 (99%)** 是指报告 99% 的日志所用的时间。 
>

| 报表 | 延迟 (95%) |延迟 (99%)|
| :-- | --- | --- | 
| 审核日志 | 2 分钟  | 5 分钟  |
 
## <a name="next-steps"></a>后续步骤

- [Azure AD 报告概述](overview-reports.md)

<!-- Update_Description: wording update -->