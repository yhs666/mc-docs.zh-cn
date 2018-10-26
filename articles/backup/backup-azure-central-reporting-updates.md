---
title: 更新 Azure 备份中心报告内容包
description: 介绍如何更新 Power BI 中的 Azure 备份内容包
services: backup
documentationcenter: ''
author: lingiw
manager: shivamg
editor: ''
ms.assetid: 59df5bec-d959-457d-8731-7b20f7f1013e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/24/2018
ms.date: 10/19/2018
ms.author: v-lingwu
ms.openlocfilehash: e6a7fda36aa29a141752661c4f55a707612a3b91
ms.sourcegitcommit: ee042177598431d702573217e2f3538878b6a984
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477768"
---
# <a name="update-the-azure-backup-central-reporting-content-pack"></a>更新 Azure 备份中心报告内容包 

[Azure 备份内容包](/backup/backup-azure-configure-reports#view-reports-in-power-bi)可用于集中查看有关备份的报告。 内容包定期更新，以添加更多的功能和进行 Bug 修复。 本文将介绍有关更新内容包、延迟此更新和逐步更新的步骤。

## <a name="get-updates-to-the-content-pack"></a>获取内容包的更新

### <a name="get-the-updated-content-pack"></a>获取更新的内容包
如果未对内容包的副本进行任何更改，则它会自动更新。 内容包更改时，你会收到 Power BI 中的通知和电子邮件通知。 可以选择在方便时获取更新的内容包。 

### <a name="postpone-the-update"></a>推迟更新
最佳做法是将内容包导入到[自定义工作区](https://youtu.be/26zyOtyHPJM?t=1m57s)。 现在可以编辑报告。
如前所述，如果内容包发生更改，你会在 Power BI 中看到通知。 可以选择稍后获取内容包。 

## <a name="coming-soon"></a>即将支持
   
Azure 备份内容包已更新，以支持更多工作负载。 工作负荷包括 Azure SQL Database for IaaS VM 备份和 System Center Data Protection Manager。 此支持已添加到当前对 Azure 备份和 Azure VM 备份的支持。 此支持意味着可以在一个中心位置查看和分析所有备份数据。 [也可自定义报告](https://youtu.be/26zyOtyHPJM)以满足组织的需求。

Azure 备份内容包随附的预配置报告正在发生更改。 这些更改使得报告在工作负载方面更有意义。 可在下方抢先了解即将发布的一组报告。

### <a name="summary"></a>摘要
   
![摘要](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Summary.png)

### <a name="billing"></a>计费

![计费](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Billing.png)

### <a name="compliance"></a>合规性

![合规性](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Compliance.png)

### <a name="storage"></a>存储

![存储](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Storage.png)

### <a name="backup-items"></a>备份项
![BackupItems](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-BackupItem.png)

### <a name="alerts"></a>警报

![警报](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Alerts.png)

### <a name="jobs"></a>作业

![作业](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Jobs.png)
    

## <a name="next-steps"></a>后续步骤：

- [Azure 备份常见问题](backup-azure-backup-faq.md)

