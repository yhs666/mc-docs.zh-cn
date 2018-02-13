---
title: "Azure 流分析查询测试 | Azure"
description: "如何测试流分析作业中的查询。"
keywords: "测试查询, 对查询进行故障排除"
documentation center: 
services: stream-analytics
author: rockboyfor
manager: digimobile
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
origin.date: 04/20/2017
ms.date: 01/22/2018
ms.author: v-yeche
ms.openlocfilehash: 67d4221509170e4ec231741b2d132fc478156a3b
ms.sourcegitcommit: 7d5b681976ac2b7e7390ccd8adce2124b5a6d588
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2018
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>在 Azure 门户中测试 Azure 流分析查询

使用 Azure 流分析，可以在无需启动或停止作业的情况下在 Azure 门户中测试查询。

## <a name="test-the-input"></a>测试输入

1. 若要使用示例输入数据进行测试，请右键单击任意输入，然后选择“从文件上传示例数据”。 当前只能上传 JSON 格式的数据。 如果数据采用其他格式（如 CSV），则应在上传前将其转换为 JSON 格式。 可以使用任何开源转换工具（如 [CSV 到 JSON 转换器](http://www.convertcsv.com/csv-to-json.htm)）将数据转换为 JSON 格式。

    ![流分析查询编辑器测试查询](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. 完成上传后，单击“测试”，以针对已提供的示例数据来测试此查询。

    ![流分析查询编辑器测试示例数据](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

若要保存测试输出以供将来使用，浏览器中显示查询输出，以及“下载结果”链接。 现在可以轻松反复地修改查询，并对其进行重复测试，以查看输出有何变化。

![流分析查询编辑器示例输出](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

由于在查询中使用了多个输出，因此可以分别查看两个输出的结果，并轻松在它们之间切换。

对浏览器中显示的结果感到满意后，即可保存查询、启动作业并使其在不出现错误的情况下处理事件。

## <a name="get-help"></a>获取帮助

若需进一步的帮助，请尝试使用我们的 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/forums/)。

## <a name="next-steps"></a>后续步骤

* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Update_Description: update meta properties, wording update -->