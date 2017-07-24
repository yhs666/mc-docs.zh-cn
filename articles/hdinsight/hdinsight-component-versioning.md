---
title: "Hadoop 组件和版本 - Azure HDInsight | Azure"
description: "了解 HDInsight 中的 Hadoop 组件和版本以及此 HortonWorks 数据平台的云分发中可用的服务级别。"
keywords: "hadoop 版本,hadoop 生态系统组件,hadoop 组件,如何检查 hadoop 版本"
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/14/2017
ms.date: 07/24/2017
ms.author: v-dazen
ms.openlocfilehash: 0f9684410660b4caa12e7f7fdb6b2006cdae780d
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>HDInsight 提供了哪些 Hadoop 组件和版本？

了解 Azure HDInsight 中的 Hadoop 生态系统组件和版本。 另外，还将了解如何检查 HDInsight 中的 Hadoop 组件版本。 

[!INCLUDE [hdinsight-linux-acn-version.md](../../includes/hdinsight-linux-acn-version.md)]

每个 HDInsight 版本都是某个版本的 Hortonworks 数据平台 (HDP) 的云分发。

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>可与不同 HDInsight 版本使用的 Hadoop 组件
Azure HDInsight 支持多个可随时部署的 Hadoop 群集版本。 每个版本选项创建 Hortonworks 数据平台 (HDP) 分发的特定版本和该分发内包含的一组组件。 截止 2017 年 2 月 17 日，Azure HDInsight 使用的默认群集当前版本为 3.5，基于 HDP 2.5。

下表中逐项列出了与 HDInsight 群集版本关联的组件版本： 

> [!NOTE]
> 服务默认版本随时可更改，恕不另行通知。 如果依赖某个版本，我们建议在创建群集时使用 .NET SDK/Azure PowerShell 和 Azure CLI 指定版本。 
>
>

| 组件 | HDInsight 版本 3.6 | HDInsight version 3.5（默认） | HDInsight 版本 3.3 | HDInsight 版本 3.2 | HDInsight 版本 3.1 | HDInsight 版本 3.0 |
| --- | --- | --- | --- | --- | --- | --- |
| Hortonworks 数据平台 |2.6 |2.5 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop 和 YARN |2.7.3 |2.7.3 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive 和 HCatalog |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|
| Apache Tez-Hive2 | 0.8.4 |-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0（仅限 Linux） |1.6.2 + 2.0（仅限 Linux） |-|1.3.1（仅限 Windows） |-|-|
| Apache Ambari | 2.5.0 | 2.4.0 |-|-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |-|-|-|

## <a name="how-to-check-current-hadoop-component-version-information"></a>如何检查当前的 Hadoop 组件版本信息

与 HDInsight 群集版本关联的 Hadoop 生态系统组件版本可能会随 HDInsight 的更新而更改。 若要检查 Hadoop 组件并验证正在为群集使用哪些版本，请使用 Ambari REST API。 **GetComponentInformation** 命令检索有关服务组件的信息。 有关详细信息，请参阅 [Ambari 文档][ambari-docs]。

仅适用于基于 Windows 的群集：获取组件版本的另一种方法是使用远程桌面登录到群集并直接检查“C:\apps\dist\" 目录的内容。

> [!IMPORTANT]
> Linux 是在 HDInsight 3.4 版或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [Windows 在 HDInsight 上停用](#hdi-version-33-nearing-retirement-date)。 

### <a name="release-notes"></a>发行说明

请参阅 [HDInsight 发行说明](hdinsight-release-notes.md)，了解 HDInsight 最新版本的更多发行说明。

## <a name="supported-hdinsight-versions"></a>支持的 HDInsight 版本
下表列出当前可用的 HDInsight 版本以及它们使用的相应 Hortonworks 数据平台版本和发布日期。 如果已知，还提供其支持到期日期和停用日期。 注意以下版本信息：

* 默认情况下，会针对 HDInsight 2.1 和更高版本的群集部署具有两个头节点的高度可用群集。 它们不适用于 HDInsight 1.6 群集。
* 某版本的支持到期后，可能不再通过 Azure 门户提供它。 下表列出 Azure 经典管理门户上提供的版本。 可继续使用 Windows PowerShell [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) 命令中的 `Version` 参数和 .NET SDK 获取群集版本，直到其停用日期为止。

| HDInsight 版本 | HDP 版本 | VM OS | 高可用性 | 在 Azure 门户上提供 |
| --- | --- | --- | --- | --- |
| HDI 3.6 |HDP 2.6 |Ubuntu 16 |是 |是 |
| HDI 3.5 |HDP 2.5 |Ubuntu 16 |是 |是 |
| HDI 3.3 |HDP 2.3 |Windows Server 2012R2 |是 |是 |
| HDI 3.2 |HDP 2.2 |Windows Server 2012R2 |是 |否 |
| HDI 3.1 |HDP 2.1 |Windows Server 2012R2 |是 |否 |
| HDI 3.0 |HDP 2.0 |Windows Server 2012R2 |是 |否 |
| HDI 2.1 |HDP 1.3 |Windows Server 2012R2 |是 |否 |
| HDI 1.6 |HDP 1.1 | |否 |否 |

## <a name="hdi-version-33-nearing-retirement-date"></a>HDI 版本 3.3 即将停用
对 HDI 3.3 群集的支持于 06/27/2016 到期，并将于 07/31/2017 停用。 如果有 HDI 3.3 群集，请尽快将群集升级到 HDI 3.5 或 HDI 3.6。 HDI 3.3 Windows 的停用时间表可能因区域而异。 如果区域所计划的停用日期不同，你会单独收到通知。

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight 群集版本的服务级别协议
SLA 是按**支持窗口**定义的。 “支持窗口”是指 HDInsight 群集版本受 Microsoft 客户服务和支持部门支持的时间段。 如果 HDInsight 群集版本具有过去的**支持到期日期**，则表示它处于支持窗口外。 有关支持的 HDInsight 群集版本的列表，请参见上表。 给定 HDInsight 版本 X（一旦提供更新的 X+1 版本）的支持到期日期为按以下公式计算所得时间的较晚者：  

* 公式 1：发布 HDInsight 群集版本 X 的日期加 180 天。
* 公式 2：HDInsight 群集版本 X+1（X 之后的后续版本）在门户中可用的日期加 90 天。

**停用日期**是指在此后不能在 HDInsight 上创建此群集版本的日期。 从 2017 年 7 月 31 日开始，不能在群集的停用日期过后调整其大小。 

> [!NOTE]
> 基于 Windows 的 HDInsight 群集（包括版本 2.1、3.0、3.1、3.2 和 3.3）在 Azure 来宾 OS 系列 4 上运行，该系列使用 64 位版本的 Windows Server 2012 R2 并支持 .NET Framework 4.0、4.5、4.5.1 和 4.5.2。
>
>

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>与 HDInsight 版本相关的 Hortonworks 发行说明
* HDInsight 群集版本 3.6 使用基于 [Hortonworks 数据平台 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) 的 Hadoop 分发版。 
* HDInsight 群集版本 3.5 使用基于 [Hortonworks 数据平台 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html) 的 Hadoop 分发版。 这是使用门户时创建的**默认** Hadoop 群集。
* HDInsight 群集版本 3.4 使用基于 [Hortonworks 数据平台 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)的 Hadoop 分发版。 
* HDInsight 群集版本 3.3 使用基于 [Hortonworks 数据平台 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)的 Hadoop 分发版。

  * [此处](https://storm.apache.org/2015/11/05/storm0100-released.html)提供了 Apache Storm 发行说明。
  * [此处](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843)提供了 Apache Hive 发行说明。
* HDInsight 群集版本 3.2 使用基于 [Hortonworks 数据平台 2.2][hdp-2-2] 的 Hadoop 分发版。  

  * 特定 Apache 组件的发行说明 - [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450)、[Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954)、[HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810)、[Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581)、[M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180)、[HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181)、[YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197)、[Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179)、[Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742)、[Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486)、[Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112) 和 [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620)。
* HDInsight 群集版本 3.1 使用基于 [Hortonworks 数据平台 2.1.7][hdp-2-1-7] 的 Hadoop 分发版。创建于 11/7/2014 之前的 HDInsight 3.1 群集基于 [Hortonworks 数据平台 2.1.1][hdp-2-1-1]。
* HDInsight 群集版本 3.0 使用基于 [Hortonworks 数据平台 2.0][hdp-2-0-8] 的 Hadoop 分发版。
* HDInsight 群集版本 2.1 使用基于 [Hortonworks 数据平台 1.3][hdp-1-3-0]的 Hadoop 分发版。
* HDInsight 群集版本 1.6 使用基于 Hortonworks 数据平台 1.1 的 Hadoop 分发版。

### <a name="pricing-and-sla"></a>定价和 SLA
有关 HDInsight 的定价和 SLA 的信息，请参阅 [HDInsight 定价](https://www.azure.cn/pricing/details/hdinsight/)。

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>群集的默认节点配置和虚拟机大小
下表列出了 HDInsight 群集的默认虚拟机 (VM) 大小：

> [!IMPORTANT]
> 如果群集中需要 32 个以上的工作节点，则必须选择至少具有 8 个核心和 14 GB RAM 的头节点大小。
>
>
| 群集类型 | Hadoop | HBase | Storm | Spark |
| --- | --- | --- | --- | --- |
| 头：默认 VM 大小 |D3 v2 |D3 v2 |A3 |D12 v2 |
| 头：建议的 VM 大小 |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2 |A3、A4、A5 |D12 v2、D13 v2、D14 v2 |
| 辅助角色：默认 VM 大小 |D3 v2 |D3 v2 |D3 v2 |Windows：D12 v2；Linux：D4 v2 |
| 辅助角色：建议的 VM 大小 |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2 |Windows：D12 v2、D13 v2、D14 v2；Linux：D4 v2、D12 v2、D13 v2、D14 v2 |
| Zookeeper：默认 VM 大小 | |A3 |A2 | |
| Zookeeper：建议的 VM 大小 | |A3、A4、A5 |A2、A3、A4 | |

> [!NOTE]
> 头称为 Storm 群集类型的 *Nimbus* 。 辅助角色称为 HBase 群集类型的“区域”以及 Storm 群集类型的“监督程序”。

## <a name="next-steps"></a>后续步骤
- [适用于 HDInsight 上的 Hadoop、Spark 等的群集安装](hdinsight-hadoop-provision-linux-clusters.md)
- [在 Windows 电脑中操作 Hadoop on HDInsight](hdinsight-hadoop-windows-tools.md)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: https://www.azure.cn/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/bk_HDP_RelNotes/content/ch_relnotes_v220.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
