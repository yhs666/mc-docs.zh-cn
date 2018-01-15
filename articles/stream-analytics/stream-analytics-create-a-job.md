---
title: "如何为流分析创建数据分析处理作业 | Azure"
description: "为流分析创建数据分析处理作业 | 学习路径段。"
keywords: "数据分析处理"
documentationcenter: 
services: stream-analytics
author: rockboyfor
manager: digimobile
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
origin.date: 03/28/2017
ms.date: 01/15/2018
ms.author: v-yeche
ms.openlocfilehash: bcfec6cfcc143690e4d19daa07b346a6a4d31e87
ms.sourcegitcommit: 14ff2d13efd62d5add6e44d613eb5a249da7ccb1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2018
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>如何为流分析创建数据分析处理作业
在 Azure 流分析中的最上层资源是一个流分析作业。  它包含一个或多个输入数据源、一个表达数据转换的查询以及一个或多个结果写入的输出目标。 用户可以利用所有这些元素，针对流式数据方案进行数据分析处理。

若要开始使用流分析，请创建一个新的流分析作业。  请注意，该操作直到作业启动后才对计费产生影响。

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 选择“新建” > “数据 + 分析” > “流分析作业”。
3. 选择“创建” 。

3. 指定流分析作业所需的配置。

    * 在“作业名称”  框中，输入一个名称以标识该流分析作业。 对“作业名称”  进行验证后，作业名称框中会出现一个绿色的复选标记。 “作业名称”只能包含字母数字字符和字符“-”，且长度必须在 3 到 63 个字符之间。
    * 使用“位置”指定想要运行作业的地理位置。
    * 指定新的或现有“资源组”，以保存应用程序的相关资源。
4. 选择“创建” 。
创建流分析作业需要几分钟时间。 要查看状态，可以在通知中心监视进度。

    ![Azure 门户数据分析处理作业创建作业](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. 新作业显示的状态为“已创建”。 请注意，“启动”  按钮被禁用。 启动作业之前，先配置作业输入、查询和输出。

    ![Azure 门户数据分析处理作业状态](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>获取帮助
如需进一步的帮助，请尝试我们的 [Azure 流分析论坛](https://www.azure.cn/support/forums/)

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)
<!--Update_Description: wording update, update link -->