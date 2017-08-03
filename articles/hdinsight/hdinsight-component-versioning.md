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
ms.date: 07/31/2017
ms.author: v-dazen
ms.openlocfilehash: 5b67e516259d8c19aadd49f6fed01d3dcae8b221
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
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
> Linux 是在 HDInsight 3.4 版或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [Windows 在 HDInsight 上停用](#hdinsight-windows-retirement)。 

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

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows 停用

Azure HDInsight (HDI) 版本 3.3 是 Windows 上的最后一个 HDInsight 版本，将在 2018 年 7 月 31 日停用。 如果目前在 Windows 上运行任何 HDI 群集（3.3 或更低版本），必须在 2018 年 7 月 31 日之前迁移到 HDInsight on Linux（HDI 3.5 或更高版本）才能继续创建或调整 HDI 群集。 对 HDI 3.3 on Windows 的支持已在 2016 年 6 月 27 日终止。 

从 HDInsight 版本 3.4 开始，Microsoft 只会在 Linux OS 上发布 HDInsight。 因此，HDInsight 中的某些组件只适用于 Linux：Apache Ranger、Kafka、交互式 Hive、Spark 和 HDInsight 应用程序。 将来的 HDInsight 版本只能在 Linux OS 上使用。 将来不会发布任何适用于 Windows OS 的 HDInsight 版本。

### <a name="faqs"></a>常见问题

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>HDInsight on Windows 的停用时间表是什么？
HDInsight on Windows 的停用日期为 2018 年 7 月 31 日。 如果所在区域的计划停用日期不同，则你会单独收到通知。 

### <a name="what-will-be-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>HDInsight on Windows 停用会对现有客户产生怎样的影响？
* 停用后，无法在 Windows 群集上创建新的 HDI。
* 停用后，无法调整现有 HDInsight on Windows 群集的大小。 
 * 将来的 HDInsight 版本只能在 Linux OS 上使用。 将来不会发布任何适用于 Windows OS 的 HDInsight 版本。
* 请注意，对 HDInsight 3.3 的支持已在 2016 年 6 月 27 日终止。 因此，我们不会针对 HDInsight 3.3 或更低版本提供支持或 bug 修复。 

### <a name="which-versions-of-hdinsight-on-windows-are-impacted"></a>哪些 HDInsight on Windows 版本受影响？
Azure HDInsight 版本 3.3 是适用于 Windows 的最后一个 HDInsight 版本。 必须在停用日期之前将 Windows 上运行的任何 HDInsight 群集（3.3 或更低版本）迁移到 HDInsight on Linux（3.5 或更高版本），才能继续新建 HDI 群集或调整现有 HDI 群集的大小。 

### <a name="what-do-i-need-to-do"></a>我需要做些什么？
请在 2018 年 7 月 31 之前参阅[此文档](/hdinsight/hdinsight-migrate-from-windows-to-linux)，将基于 Windows 的 HDInsight 群集迁移到支持的基于 Linux 的 HDInsight 群集。 有关支持的 HDI 版本的信息，请访问[此文档](/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions) 

### <a name="where-do-i-find-the-cluster-os-type"></a>在何处可以找到群集 OS 类型？
在 Azure 门户中转到 HDInsight 群集概述页。 在该页的“概述”下面可以找到“群集类型”。 这就表示群集的 OS 类型。 

### <a name="what-will-be-the-impact-to-my-hdinsight-windows-cluster-if-i-cant-migrate-to-a-linux-based-cluster-by-jul-31st-2018"></a>如果在 2018 年 7 月 31 之前无法迁移到基于 Linux 的群集，HDInsight Windows 群集会受到哪种影响？
HDInsight Windows 群集会像平时一样运行，但是，无法新建 HDInsight on Windows 群集，或者调整现有 HDInsight on Windows 群集的大小。 

### <a name="my-cluster-has-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>我的群集存在 .NET 依赖关系。 如何在 Linux 上解决这种依赖关系？
可以在 HDInsight Linux 群集上使用 [Mono](http://www.mono-project.com/)（.NET 的开源实现）。 请参阅[此文档](/hdinsight/hdinsight-migrate-from-windows-to-linux)了解更多详细信息。 

### <a name="i-am-a-new-customer-trying-to-create-an-hdinsight-windows-based-cluster-however-i-dont-see-the-option-in-the-azure-portal-or-i-am-unable-to-create-this-from-powershell-or-sdk-how-can-i-create-an-hdi-windows-based-cluster"></a>我的一个新客户尝试创建基于 HDInsight Windows 的群集，但是我在 Azure 门户中未看到相应的选项，或者无法通过 PowerShell 或 SDK 创建此群集。 如何创建基于 HDI Windows 的群集？
从 2017 年 7 月 3 日起（到停用日期为止），只有现有的基于 HDInsight Windows 的客户可以新建基于 HDI Windows 的群集。 我们建议创建基于 Linux 的 HDI 群集。 

### <a name="is-there-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>从 HDInsight on Windows 迁移到 HDInsight on Linux 是否存在定价方面的影响？
没有，任何一个 OS 上的HDInsight 定价都是相同的。 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-hdinsight-only-on-the-linux-os"></a>只在 Linux OS 上使用 HDInsight 会给客户带来哪些优势？
* 通过 HDInsight 服务加快开源大数据技术的面市时间
* 获得大型社区和生态系统的支持
* 更好地利用开源社区的主动开发实现 Hadoop 和更新的大数据技术

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-that-available-in-hdinsight-on-windows"></a>除了 HDInsight on Windows 提供的功能以外，HDInsight on Linux 是否还提供其他功能？
从 HDInsight 版本 3.4 开始，MSFT 只会在 Linux OS 上发布 HDInsight。 因此，HDInsight 中的某些组件只适用于 Linux - Apache Ranger、Kafka、交互式 Hive、Spark 和 HDInsight 应用程序。 

## <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight 群集版本的服务级别协议
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

<!--Update_Description: add FAQ-->