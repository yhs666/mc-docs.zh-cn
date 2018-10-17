---
title: 快速入门：使用 Azure 门户下载审核报告 | Microsoft Docs
description: 了解如何使用 Azure 门户下载审核报告
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
origin.date: 09/13/2018
ms.date: 10/09/2018
ms.author: v-junlch
ms.reviewer: dhanyahk
ms.openlocfilehash: 137f3430a6dfc712d884ded36c4af9872c477c7e
ms.sourcegitcommit: b91bbb0f49b12b1a7ac9eaf4742daa0c4a513f11
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876456"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>快速入门：使用 Azure 门户下载审核报告

在本快速入门中，你将了解如何下载过去 24 小时内租户的审核日志。

## <a name="prerequisites"></a>先决条件

需要：

- 一个 Azure Active Directory 租户。 
- 属于租户的“安全管理员”、“安全读者”或“全局管理员”角色的用户。 此外，租户中的任何用户都可以访问自己的审核日志。

## <a name="quickstart-download-an-audit-report"></a>快速入门：下载审核报告

1. 导航到 [Azure 门户](https://portal.azure.cn)。
2. 从左侧导航窗格中选择 **Azure Active Directory**，然后使用“切换目录”按钮选择活动目录。
3. 在仪表板中，选择 **Azure Active Directory**，然后选择“审核日志”。 
4. 在“日期范围”筛选器下拉列表中选择“24 小时”，然后选择“应用”以查看过去 24 小时内的审核日志。 
5. 选择“下载”按钮以下载包含已筛选记录的 CSV 文件。 

![报告](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>后续步骤

- [Azure Active Directory 报告延迟](reference-reports-latencies.md)

