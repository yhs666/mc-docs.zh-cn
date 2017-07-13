---
title: "开始使用 Azure SQL 数据库的备份和还原进行数据保护和恢复 | Azure"
description: "本教程介绍如何从自动化备份还原到某个时间点、如何将自动化备份存储在 Azure 恢复服务保管库中，以及如何从 Azure 恢复服务保管库还原"
keywords: "sql 数据库教程"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
origin.date: 12/08/2016
ms.date: 01/20/2017
ms.author: v-johch
ms.openlocfilehash: 79847bd81ae94364a063519b3f305cbf2a86a113
ms.sourcegitcommit: b9eccd13f7aaab4e2f67b3a12d5ebb501750287d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
<!------------------
This topic is annotated with TEMPLATE guidelines for TUTORIAL TOPICS.

Metadata guidelines

title
    60 characters or less. Tells users clearly what they will do (deploy an ASP.NET web app to App Service). Not the same as H1. It's 60 characters or fewer including all characters between the quotes and the Microsoft Docs site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

```
Example: "This tutorial shows how to deploy an ASP.NET web application to a web app in Azure App Service by using Visual Studio 2015."
```
------------------>

<!----------------

TEMPLATE GUIDELINES for tutorial topics

The tutorial topic shows users how to solve a problem using a product or service. It includes the prerequisites and steps users need to be successful.  

It is a "solve a problem" topic, not a "learn concepts" topic.

DO include this:
    • What users will do
    • What they will create or accomplish by the end of the tutorial
    • Time estimate
    • Optional but useful: Include a diagram or video. Diagrams help users see the big picture of what they are doing. A video of the steps can be used by customers as an alternative to following the steps in the topic.
    • Prerequisites: Technical expertise and software requirements
    • End-to-end steps. At the end, include next steps to deeper or related tutorials so users can learn more about the service

DON'T include this:
    • Conceptual info about the service. This info is in overview topics that you can link to in the prerequisites section if necessary

------------------->

<!------------------
GUIDELINES for the H1 

```
The H1 should answer the question "What will I do in this topic?" Write the H1 heading in conversational language and use search keywords as much as possible. Since this is a "solve a problem" topic, make sure the title indicates that. Use a strong, specific verb like "Deploy."  

Heading must use an industry standard term. If your feature is a proprietary name like "elastic pools", use a synonym. For example: "Learn about elastic pools for multi-tenant databases." In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.
```

-------------------->

<a id="get-started-with-backup-and-restore-for-data-protection-and-recovery" class="xliff"></a>

# 开始使用备份和还原进行数据保护和恢复

<!------------------
    GUIDELINES for introduction

```
The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top keywords that you are using throughout the article.The introduction should be brief and to the point of what users will do and what they will accomplish. 

In this example:
```

Sentence #1 Explains what the user will do. This is also the metadata description. 
    This tutorial shows how to deploy an ASP.NET web application to a web app in Azure App Service by using Visual Studio 2015. 

Sentence #2 Explains what users will learn and the benefit.  
    When you’re finished, you’ll have a simple web application up and running in the cloud.

-------------------->

本入门教程介绍如何使用 Azure 门户完成以下任务：

- 查看数据库的现有备份
- 将数据库还原到以前的时间点
- 从 Azure 恢复服务保管库还原数据库

用时估计：完成本教程大约需要 30 分钟（假设满足先决条件）。

<a id="prerequisites" class="xliff"></a>

## 先决条件

* 需要一个 Azure 帐户。 可以[申请 Azure 1 元试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。 

* 必须能够使用帐户连接到 Azure 门户，该帐户是订阅所有者或参与者角色的成员。 若要详细了解基于角色的访问控制 (RBAC)，请参阅 [Azure 门户中的访问管理入门](../active-directory/role-based-access-control-what-is.md)。

* 已完成本教程中[通过 Azure 门户和 SQL Server Management Studio 开始使用 Azure SQL 数据库服务器、数据库和防火墙规则](./sql-database-get-started.md)或等效的 [PowerShell 版本](./sql-database-get-started-powershell.md)的步骤。 如果没有，请完成此必学教程，或在完成本教程的 [PowerShell 版本](./sql-database-get-started-powershell.md)部分时执行 PowerShell 脚本，然后再继续。

<!------------------
> [!TIP]
> You can perform these same tasks in a getting started tutorial by using either [C#](./sql-database-get-started-csharp.md) or [PowerShell](./sql-database-get-started-powershell.md).
>
-------------------->

<a id="sign-in-by-using-your-existing-account" class="xliff"></a>

## 使用现有帐户登录
使用 [现有订阅](https://account.windowsazure.cn/Home/Index)，按照以下步骤连接到 Azure 门户。

1. 打开所选浏览器并连接到 [Azure 门户](https://portal.azure.cn/)。
2. 登录到 [Azure 门户](https://portal.azure.cn/)。
3. 在“登录”  页上，提供订阅的凭据。

   ![登录](./media/sql-database-get-started/login.png)

<a name="create-logical-server-bk"></a>

<a id="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database" class="xliff"></a>

## 查看服务生成的数据库备份的最早还原点

在教程的本部分中，从数据库的[服务生成的自动备份](./sql-database-automated-backups.md)中查看有关最早还原点的信息。 

1. 打开数据库的“SQL 数据库”边栏选项卡 (sqldbtutorialdb)。

    ![新建示例 db 边栏选项卡](./media/sql-database-get-started/new-sample-db-blade.png)

2. 在工具栏上，单击“还原” 。

    ![还原工具栏](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

3. 在“还原”边栏选项卡上，查看最早还原点。

    ![最早还原点](./media/sql-database-get-started-backup-recovery/oldest-restore-point.png)

<a id="restore-a-database-to-a-previous-point-in-time" class="xliff"></a>

## 将数据库还原到以前的时间点

在本教程的此部分，需将数据库还原到特定时间点的新数据库。

1. 在数据库的“还原”边栏选项卡，查看要将数据库还原到以前某个时间点的新数据库的默认名称（名称为附加了时间戳的现有数据库名称）。 此名称会随后续步骤中指定的时间而变化。

    ![还原的数据库的名称](./media/sql-database-get-started-backup-recovery/restored-database-name.png)

2. 单击“还原点(UTC)”输入框中的“日历”图标。

    ![还原点](./media/sql-database-get-started-backup-recovery/restore-point.png)

2. 在日历上，选择保持期内的一个日期

    ![还原点日期](./media/sql-database-get-started-backup-recovery/restore-point-date.png)

3. 在“还原点(UTC)”输入框中，指定要从自动数据库备份中将数据库中的数据还原到的所选日期的时间。

    ![还原点时间](./media/sql-database-get-started-backup-recovery/restore-point-time.png)

    >[!NOTE]
    >请注意，数据库名称已随所选日期和时间而变化。 另请注意，不能更改要在特定时间点还原到其上的服务器。 若要还原到其他服务器，请使用[异地还原](./sql-database-disaster-recovery.md#recover-using-geo-restore)。
    >

4. 单击“确定”将数据库还原到以前某个时间点的新数据库。

5. 在工具栏上，单击通知图标可查看还原作业的状态。

    ![还原作业进度](./media/sql-database-get-started-backup-recovery/restore-job-progress.png)

6. 在还原作业完成后，打开“SQL 数据库”  边栏选项卡可查看新还原的数据库。

    ![还原的数据库](./media/sql-database-get-started-backup-recovery/restored-database.png)

   > [!NOTE]
   > 从此处，可使用 SQL Server Management Studio 连接到已还原的数据库，以执行所需任务，例如[从恢复的数据库中提取一部分数据，复制到现有数据库或删除现有数据库，并将已还原数据库的名称重命名为现有数据库名称](./sql-database-recovery-using-backups.md#point-in-time-restore)。
   >


<a id="next-steps" class="xliff"></a>

## 后续步骤

- 若要了解服务生成的自动备份，请参阅[自动备份](./sql-database-automated-backups.md)
- 若要了解如何从备份中还原，请参阅[从备份中还原](./sql-database-recovery-using-backups.md)