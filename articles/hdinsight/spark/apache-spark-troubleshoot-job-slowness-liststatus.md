---
title: 在 Azure HDInsight 中处理 Apache Spark 流作业所需的时间比平时要长
description: 在 Azure HDInsight 中处理 Apache Spark 流作业所需的时间比平时要长
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 07/29/2019
ms.date: 09/23/2019
ms.openlocfilehash: d392737e95262158c19bfbe05aed692a8db9a109
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921327"
---
# <a name="scenario-apache-spark-streaming-jobs-take-longer-than-usual-to-process-in-azure-hdinsight"></a>方案：在 Azure HDInsight 中处理 Apache Spark 流作业所需的时间比平时要长

本文介绍在 Azure HDInsight 群集中使用 Apache Spark 组件时出现的问题的故障排除步骤和可能的解决方法。

## <a name="issue"></a>问题

你注意到某些 Apache Spark 流作业的速度较慢，或者其处理时间比平常要长。 对于 Spark 流应用程序，每批消息对应于提交到 Spark 的一个作业。 平时只需 X 秒即可处理的作业偶尔可能需要额外花费 2-3 分钟。

## <a name="cause"></a>原因

一个可能的原因是 Apache Kafka 生成者需要 2 分钟以上才能完成写出到 Kafka 群集的操作。 若要进一步调试 Kafka 问题，可将一些日志添加到使用 Kafka 生成者发出消息的代码，并将这些日志关联到 Kafka 群集中的日志。

另一个可能的原因是频繁读取和写入 WASB 导致后续微批出现滞后。 由于删除重复项的 `O(n!)` 算法原因，`Filesystem.listStatus` 的 WASB 实现非常缓慢。 由于需要执行从 `BlobListItem` 到 `FileMetadata` 再到 `FileStatus` 的额外转换，该算法会占用过多内存。 例如，该算法需要 30 分钟以上的时间才能列出 700,000 个文件。 因此，如果 SparkSQL 的每个微批频繁调用 `ListBlobs`，将会导致后续微批出现滞后，从而导致出现使用较高计划延迟时发生的情况。 [此修补程序](https://issues.apache.org/jira/browse/HADOOP-15547)可修复该问题，但如果环境中缺少此修补程序，`ListBlobs` 将会出现高延迟。 此外，即使每小时删除文件，后端中的列出操作也必须循环访问所有行（包括已删除的行），因为垃圾回收过程尚未完成。 尽管该修补程序可以部分地解决问题，但垃圾回收问题仍可能导致批的流处理出现延迟。

## <a name="resolution"></a>解决方法

应用 [HADOOP-15547](https://issues.apache.org/jira/browse/HADOOP-15547) 修复程序。 如果无法应用该修复程序，可以使用 HDFS 作为检查点位置。 将 `checkpointDirectory` 设置为如下所示的内容：`hdfs://mycluster/checkpoint`。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 社区支持](https://azure.microsoft.com/support/community/)获取 Azure 专家的解答。

* 与 [@AzureSupport](https://twitter.com/azuresupport)（Microsoft Azure 官方帐户）联系，它可以将 Azure 社区引导至适当的资源来改进客户体验：提供解答、支持和专业化服务。

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
