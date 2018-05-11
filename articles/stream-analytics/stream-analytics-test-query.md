---
title: 使用样本数据测试 Azure 流分析作业 | Azure
description: 如何测试流分析作业中的查询。
keywords: 测试作业, 输入采样, 上传样本数据
documentationcenter: ''
services: stream-analytics
author: rockboyfor
manager: digimobile
ms.assetid: ''
ms.service: stream-analytics
ms.workload: data-services
origin.date: 03/18/2018
ms.date: 05/07/2018
ms.author: v-yeche
ms.openlocfilehash: ec56c58c92d47d9bdb13a4e1afa43765a21d781d
ms.sourcegitcommit: 0b63440e7722942ee1cdabf5245ca78759012500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
---
# <a name="test-your-stream-analytics-query-with-sample-data"></a>使用样本数据测试流分析查询

使用 Azure 流分析，可以在门户中上传样本数据并测试查询，且不需要启动或停止作业。

## <a name="upload-sample-data-and-test-the-query"></a>上传样本数据并测试查询

1. 导航到某个现有的流分析作业 > 单击“查询”以打开查询编辑器窗口。 

2. 若要使用样本输入数据来测试查询，请右键单击任意输入，并选择“上载文件中的示例数据”。 当前只能上传 JSON 格式的数据。 如果数据采用其他格式（如 CSV），则应在上传前将其转换为 JSON 格式。 可以使用任何开源转换工具（如 [CSV 到 JSON 转换器](http://www.convertcsv.com/csv-to-json.htm)）将数据转换为 JSON 格式。

    ![流分析查询编辑器测试查询](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

3. 完成上传后，单击“测试”，以针对已提供的示例数据来测试此查询。

    ![流分析查询编辑器测试示例数据](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

4. 若要保存测试输出以供将来使用，浏览器中显示查询输出，以及下载结果的链接。 现在可以轻松反复地修改查询，并对其进行重复测试，以查看输出有何变化。

   ![流分析查询编辑器示例输出](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

当在查询中使用多个输出时，可以单独查看每个输出的结果，并在它们之间轻松切换。 在验证浏览器中显示的结果后，可以保存查询、启动作业并使其在不出现错误的情况下处理事件。
如需更多帮助，请尝试访问我们的 [Azure 流分析论坛](https://www.azure.cn/support/contact/)。

## <a name="next-steps"></a>后续步骤

* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Update_Description: update meta properties, wording update, update link -->