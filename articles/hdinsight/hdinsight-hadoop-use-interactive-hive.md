---
title: "使用 HDInsight 中的交互式 Hive | Azure"
description: "了解如何在 HDInsight 中使用交互式 Hive（基于 LLAP 的 Hive）。"
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/06/2017
ms.date: 03/28/2017
ms.author: v-dazen
ms.openlocfilehash: 01e3535e95dd08ec680842c79bb34e1b6a076b00
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>在 HDInsight（预览版）中使用交互式 Hive
交互式 Hive（也称为 [Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP)）是一个新的 HDInsight [群集类型](hdinsight-hadoop-provision-linux-clusters.md#cluster-types)。  交互式 Hive 允许在内存中缓存，这使 Hive 查询更具交互性且速度更快。 这项新功能使 HDInsight 成为世界上性能、灵活性和开放性最高的，具有内存中缓存（使用 Hive 和 Spark）和高级分析功能（通过与 R 服务的深度集成）的云端大数据解决方案之一。 

交互式 Hive 群集与 Hadoop 群集不同。 它只包含 Hive 服务。 

> [!NOTE]
> MapReduce、Pig、Sqoop、Oozie，以及其他服务很快将从此群集类型中删除。
> 只能通过 Ambari Hive 视图、Beeline 和 Hive ODBC 访问交互式 Hive 群集中的 Hive 服务。 无法通过 Hive 控制台、Templeton、Azure CLI 和 Azure PowerShell 访问它。 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>创建交互式 Hive 群集
只有基于 Linux 的群集支持交互式 Hive 群集。 有关创建 HDInsight 群集的信息，请参阅[在 HDInsight 中创建基于 Linux 的 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)。

## <a name="execute-hive-from-interactive-hive"></a>从交互式 Hive 执行 Hive
执行 Hive 查询的方式有多种：

* 使用 Ambari Hive 视图运行 Hive

    有关使用 Hive 视图的信息，请参阅 [在 HDInsight 中将 Hive 视图与 Hadoop 配合使用](hdinsight-hadoop-use-hive-ambari-view.md)。
* 使用 Beeline 运行 Hive

    有关在 HDInsight 上使用 Beeline 的信息，请参阅[通过 Beeline 在 HDInsight 中将 Hive 与 Hadoop 配合使用](hdinsight-hadoop-use-hive-beeline.md)。

    可以在头节点或空的边缘节点中使用 Beeline。  建议在空的边缘节点中使用 Beeline。  有关使用空边缘节点创建 HDInsight 群集的信息，请参阅[在 HDInsight 中使用空边缘节点](hdinsight-apps-use-edge-node.md)。
* 使用 Hive ODBC 运行 Hive

    有关使用 Hive ODBC 的信息，请参阅[使用 Microsoft Hive ODBC 驱动程序将 Excel 连接到 Hadoop](hdinsight-connect-excel-hive-odbc-driver.md)。

**若要查找 JDBC 连接字符串：**

1. 使用以下 URL 登录到 Ambari： https://\<ClusterName\>.azurehdinsight.cn。
2. 在左侧菜单中，单击“Hive”。
3. 单击突出显示图标以复制 URL：

   ![HDInsight Hadoop 交互式 Hive LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>另请参阅
* [在 HDInsight 中创建基于 Linux 的 Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)：了解如何在 HDInsight 中创建交互式 Hive 群集。
* [通过 Beeline 在 HDInsight 中将 Hive 与 Hadoop 配合使用](hdinsight-hadoop-use-hive-beeline.md)：了解如何使用 Beeline 提交 Hive 查询。
* [使用 Microsoft Hive ODBC 驱动程序将 Excel 连接到 Hadoop](hdinsight-connect-excel-hive-odbc-driver.md)：了解如何将 Excel 连接到 Hive。