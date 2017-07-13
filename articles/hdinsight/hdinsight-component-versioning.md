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
ms.date: 06/05/2017
ms.author: v-dazen
ms.translationtype: Human Translation
ms.sourcegitcommit: 08618ee31568db24eba7a7d9a5fc3b079cf34577
ms.openlocfilehash: fb920e2866a96c08ec96071750bbe71a6b902e00
ms.contentlocale: zh-cn
ms.lasthandoff: 05/26/2017


---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>HDInsight 提供了哪些 Hadoop 组件和版本？

了解有关为 Azure HDInsight 以及其内含的 Hadoop 生态系统组件和版本提供的服务级别。 每个 HDInsight 版本都是某个版本的 Hortonworks 数据平台 (HDP) 的云分发。

[!INCLUDE [hdinsight-linux-acn-version.md](../../includes/hdinsight-linux-acn-version.md)]

每个 HDInsight 版本都是某个版本的 Hortonworks 数据平台 (HDP) 的云分发。

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>可与不同 HDInsight 版本使用的 Hadoop 组件
Azure HDInsight 支持多个可随时部署的 Hadoop 群集版本。 每个版本选项创建 Hortonworks 数据平台 (HDP) 分发的特定版本和该分发内包含的一组组件。 Azure HDInsight 使用的默认群集当前版本为 3.5，基于 HDP 2.5。

下表中逐项列出了与 HDInsight 群集版本关联的组件版本： 

> [!NOTE]
> 服务默认版本随时可更改，恕不另行通知。 如果依赖某个版本，我们建议在创建群集时使用 .NET SDK/Azure PowerShell 和 Azure CLI 指定版本。 
>
>

| 组件 | HDInsight 版本 3.6 | HDInsight version 3.5（默认） | HDInsight 版本 3.4 | HDInsight 版本 3.3 | HDInsight 版本 3.2 | HDInsight 版本 3.1 | HDInsight 版本 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks 数据平台 |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop 和 YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive 和 HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez-Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0（仅限 Linux） |1.6.2 + 2.0（仅限 Linux） |1.6.0（仅限 Linux） |1.5.2（仅限 Linux/实验性生成） |1.3.1（仅限 Windows） |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|

## <a name="how-to-check-current-hadoop-component-version-information"></a>如何检查当前的 Hadoop 组件版本信息

与 HDInsight 群集版本关联的 Hadoop 生态系统组件版本可能会随 HDInsight 的更新而更改。 若要检查 Hadoop 组件并验证正在为群集使用哪些版本，请使用 Ambari REST API。 **GetComponentInformation** 命令检索有关服务组件的信息。 有关详细信息，请参阅 [Ambari 文档][ambari-docs]。

仅适用于基于 Windows 的群集：获取组件版本的另一种方法是使用远程桌面登录到群集并直接检查“C:\apps\dist\" 目录的内容。

> [!IMPORTANT]
> Linux 是在 HDInsight 3.4 版或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 3.3 弃用](#hdi-version-33-nearing-deprecation-date)。 

### <a name="release-notes"></a>发行说明

请参阅 [HDInsight 发行说明](hdinsight-release-notes.md)，了解 HDInsight 最新版本的更多发行说明。

## <a name="supported-hdinsight-versions"></a>支持的 HDInsight 版本
下表列出当前可用的 HDInsight 版本以及它们使用的相应 Hortonworks 数据平台版本和发布日期。 如果知道，还提供其支持到期日期和弃用日期。 请注意以下事项：

* 默认情况下，会针对 HDInsight 2.1 和更高版本的群集部署具有两个头节点的高度可用群集。 它们不适用于 HDInsight 1.6 群集。
* 某版本的支持到期后，可能不再通过 Azure 门户提供它。 下表列出 Azure 经典管理门户上提供的版本。 可继续使用 Windows PowerShell [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) 命令中的 `Version` 参数和 .NET SDK 获取群集版本，直到其弃用日期为止。

| HDInsight 版本 | HDP 版本 | VM OS | 高可用性 | 在 Azure 门户上提供 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDI 3.6 |HDP 2.6 |Ubuntu 16 |是 |是 |
| HDI 3.5 |HDP 2.5 |Ubuntu 16 |是|是 |
| HDI 3.3 |HDP 2.3 |Windows Server 2012R2 |是 |是 |
| HDI 3.2 |HDP 2.2 |Windows Server 2012R2 |是 |否 |
| HDI 3.1 |HDP 2.1 |Windows Server 2012R2 |是 |否 |
| HDI 3.0 |HDP 2.0 |Windows Server 2012R2 |是 |否 |
| HDI 2.1 |HDP 1.3 |Windows Server 2012R2 |是 |否 |
| HDI 1.6 |HDP 1.1 | |否 |否 |

##<a name="hdi-version-33-nearing-deprecation-date"></a>HDI 版本 3.3 接近弃用日期
如果有 HDI 3.3 群集，请尽快将群集升级到 HDI 3.5 或 HDI 3.6。 HDI 3.3 Windows 的弃用时间表可能因区域而异。 如果所在区域的计划弃用日期与本通信中确定的日期不同，客户将会收到单独的通信。

### 常见问题解答：

#### 基于 Windows 的 HDInsight 的停用时间表是怎样的？
基于 Windows 的 HDInsight 将于 2018 年 7 月 31 日停用

#### 停用基于 Windows 的 HDInsight 将对现有客户有何影响？
* 停用日之后，将无法在 Windows 群集上创建新的 HDI。
* 停用日之后，将无法在 Windows 群集上调整现有的 HDInsight的大小。
  * 之后HDInsight 的发布将仅可在 Linux 操作系统上使用。Windows 操作系统上将不会做HDI 的发布。
* 请注意，对 HDInsight 3.3 的支持已于 2016 年 6 月 27 到期。因此，HDInsight 3.3 或更低版本已不受支持，也无相关修复。

#### 哪些基于 Windows 的 HDInsight 版本将受到影响？
Azure HDInsight 3.3 版是适用于 Windows 的最新 HDInsight 版本。在停用日之前，必须将任何运行 Windows（3.3 或更低版本）的 HDInsight 群集迁移至运行 Linux（3.5 或更高版本）的 HDInsight，才能继续创建新的 HDI 群集，或调整现有 HDI 群集的大小。 

#### 我需要做些什么？
请于 2018 年 7 月 31 日之前参考[本文档](hdinsight-migrate-from-windows-to-linux.md)将基于 Windows 的 HDInsight 群集迁移到基于 Linux 的受支持的 HDInsight 群集。有关受支持的 HDI 版本的信息，请访问[此处](#supported-hdinsight-versions)

#### 在何处可以找到群集操作系统类型？
在 Azure 门户中，转至 HDInsight 群集概述页面。在该页面的 Essentials 下面，您可以找到群集类型。此内容将指明群集的操作系统类型。

#### 如果我在 2018 年 7 月 31 日之前无法迁移至基于 Linux 的群集，我的 HDInsight Windows 群集将面临何种影响？
HDInsight Windows 群集将照样样运行，但是您将无法在 Windows 群集上创建新的 HDInsight，或者调整 Windows 群集上的现有 HDInsight 大小。

#### 我的群集拥有 .NET 依赖项。如何在 Linux 上解决该依赖项？
在 HDInsight Linux 群集上，部署有 .NET 开源实施的软件平台 [Mono](http://www.mono-project.com/)。有关详细信息，请参见[本文档](hdinsight-migrate-from-windows-to-linux.md)。
 
#### 我是一位尝试创建基于 Windows 的 HDInsight 群集的新客户，但未在 Azure 门户中找到相关选项，或者无法通过 PowerShell 或 SDK 创建。我如何才能创建一个基于 Windows 的 HDI 群集？
自 2017 年 7 月 3 日起，只有基于 Windows 的现有 HDInsight 客户才能创建基于 Windows 的新 HDI 群集（直到停用日期为止）。我们建议您创建基于 Linux 的 HDI 群集。
 
#### 从基于 Windows 的 HDInsight 迁移至基于 Linux 的 HDInsight 会对价格产生影响吗？
没有影响，基于两种操作系统的 HDInsight 价格相同。

#### 迁移至仅运行 Linux 操作系统的 HDInsight，将为客户带来哪些优势？
* 通过 HDInsight 服务，加快开源大数据技术的上市速度
* 享有一个大型的支持社区和生态系统
* 通过 Hadoop 开源社区和更新的大数据技术更好地利用正在进行的开发活动

#### 与基于 Windows 的 HDInsight 相比，基于 Linux 的 HDInsight 还能提供哪些其他功能？
从 HDInsight 3.4 版开始，我们发布了仅基于 Linux 操作系统的 HDInsight。因此，HDInsight 中的一些组件将仅可用于 Linux — Apache Ranger、Kafka、Interactive Hive、Spark、HDInsight 应用程序。

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight 群集版本的服务级别协议
SLA 用“支持窗口”来定义。 “支持窗口”是指 HDInsight 群集版本受 Microsoft 客户服务和支持部门支持的时间段。 如果 HDInsight 群集版本具有早于当前日期的 **支持过期日期** ，则表示它处于支持窗口外。 有关支持的 HDInsight 群集版本的列表，请参见上表。 给定 HDInsight 版本 X（一旦提供更新的 X+1 版本）的支持到期日期为按以下公式计算所得时间的较晚者：  

* 公式 1：发布 HDInsight 群集版本 X 的日期加 180 天。
* 公式 2：HDInsight 群集版本 X+1（X 之后的后续版本）在门户中可用的日期加 90 天。

**弃用日期**是指在此后不能在 HDInsight 上创建此群集版本的日期。

> [!NOTE]
> 基于 Windows 的 HDInsight 群集（包括版本 2.1、3.0、3.1、3.2 和 3.3）在 Azure 来宾 OS 系列 4 上运行，该系列使用 64 位版本的 Windows Server 2012 R2 并支持 .NET Framework 4.0、4.5、4.5.1 和 4.5.2。
>
>

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>与 HDInsight 版本相关的 Hortonworks 发行说明
* HDInsight 群集版本 3.4 使用基于 [Hortonworks 数据平台 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)的 Hadoop 分发版。 这是使用门户时创建的 **默认** Hadoop 群集。
* HDInsight 群集版本 3.3 使用基于 [Hortonworks 数据平台 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)的 Hadoop 分发版。

    * [此处](https://storm.apache.org/2015/11/05/storm0100-released.html)提供了 Apache Storm 发行说明。
    * [此处](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843)提供了 Apache Hive 发行说明。
* HDInsight 群集版本 3.2 使用基于 [Hortonworks 数据平台 2.2][hdp-2-2] 的 Hadoop 分发版。  

    * 特定 Apache 组件的发行说明 - [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450)、[Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954)、[HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810)、[Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581)、[M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180)、[HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181)、[YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197)、[Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179)、[Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742)、[Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486)、[Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112) 和 [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620)。
* HDInsight 群集版本 3.1 使用基于 [Hortonworks 数据平台 2.1.7][hdp-2-1-7] 的 Hadoop 分发版。
* HDInsight 群集版本 3.0 使用基于 [Hortonworks 数据平台 2.0][hdp-2-0-8] 的 Hadoop 分发版。
* HDInsight 群集版本 2.1 使用基于 [Hortonworks 数据平台 1.3][hdp-1-3-0]的 Hadoop 分发版。
* HDInsight 群集版本 1.6 使用基于 Hortonworks 数据平台 1.1的 Hadoop 分发版。

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

