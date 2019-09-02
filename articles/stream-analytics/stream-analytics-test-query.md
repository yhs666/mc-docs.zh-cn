---
title: 使用样本数据测试 Azure 流分析作业
description: 本文介绍如何使用 Azure 门户测试 Azure 流分析作业、示例输入以及上传样本数据。
services: stream-analytics
author: lingliw
ms.author: v-lingwu
ms.reviewer: jasonh
manager: digimobile
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 8/9/2019
ms.date: 6/21/2019
ms.openlocfilehash: f224309d5464d8878a705178509eea02b573fae0
ms.sourcegitcommit: 3702f1f85e102c56f43d80049205b2943895c8ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68969544"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>使用样本数据测试流分析查询

使用 Azure 流分析，可以从输入采样数据或在 Azure 门户中上传样本数据以测试查询，而不需要启动或停止作业。

## <a name="upload-or-sample-data-from-a-live-source-to-test-the-query"></a>从实时源上传或采样数据以测试查询

1. 登录到 Azure 门户。 

2. 找到现有流分析作业并选择它。

3. 在“流分析作业”页上的“作业拓扑”  标题下，选择“查询”  以打开“查询编辑器”窗口。 

4. 若要测试查询，可以从实时输入采样数据，或从文件上传数据。 此数据必须以 JSON、CSV 或 AVRO 进行序列化。 示例输入必须以 UTF-8 编码并且未压缩。 仅支持使用逗号 (,) 分隔符来测试门户上的 CSV 输入。

    1. 使用实时输入：右键单击任意输入。 然后选择“从输入采样数据”  。 在下一个屏幕中，可以设置采样的持续时间。 从实时源采样事件将检索最多 1000 个事件或 1 MB（以先达到的为准），因此采样的数据可能不代表指定的完整时间间隔。

    1. 使用文件：右键单击任意输入。 并选择“上传文件中的样本数据”  。 

    ![流分析查询编辑器测试查询](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. 完成采样或上传后，选择“测试”  ，以针对已提供的样本数据测试此查询。

    ![流分析查询编辑器测试示例数据](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. 如果需要测试输出以供将来使用，浏览器中显示查询输出，以及用于下载结果的链接。 

7. 反复修改查询，并对其再次进行测试，以查看输出有何变化。

   ![流分析查询编辑器示例输出](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   在查询中使用多个输出时，结果将显示在单独的选项卡上，可以在它们之间轻松切换。

8. 验证浏览器中显示的结果后，**保存**查询。 然后**启动**作业，并让它处理传入事件。

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)

<!--Update_Description: update meta properties, wording update -->