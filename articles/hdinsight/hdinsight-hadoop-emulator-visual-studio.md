---
title: 将用于 Visual Studio 的 Data Lake 工具与 Hortonworks 沙盒配合使用 - Azure HDInsight | Azure
description: 了解如何将用于 Visual Studio 的 Azure Data Lake 工具与在本地 VM 中运行的 Hortonworks 沙盒配合使用。 使用这些工具，可以在沙盒中创建和运行 Hive 与 Pig 作业，并查看作业输出和历史记录。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 05/07/2018
ms.date: 06/25/2018
ms.author: v-yiso
ms.openlocfilehash: cbb8a2fdaea3bb49d7025376a672f264c440b9b7
ms.sourcegitcommit: d5a43984d1d756b78a2424257269d98154b88896
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2018
ms.locfileid: "36747345"
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>将针对 Visual Studio 的 Azure Data Lake 工具与 Hortonworks 沙盒配合使用

Azure Data Lake 包含用于处理常规 Hadoop 群集的工具。 本文档提供将 Data Lake 工具与本地虚拟机上运行的 Hortonworks 沙盒配合使用所要执行的步骤。

借助 Hortonworks 沙盒可以在开发环境本地使用 Hadoop。 开发一个解决方案后，若要大规模部署该解决方案，可以转移到 HDInsight 群集。

## <a name="prerequisites"></a>先决条件

* 在开发环境上的虚拟机中运行的 Hortonworks 沙盒。 本文档是根据 Oracle VirtualBox 中运行的沙盒编写和测试的， 有关设置沙盒的详细信息，请参阅 [Hortonworks 沙盒入门](hadoop/apache-hadoop-emulator-get-started.md) 文档。

* Visual Studio 2013、Visual Studio 2015 或 Visual Studio 2017（任意版本）。

* [用于 .NET 的 Azure SDK](/downloads/) 2.7.1 或更高版本。

* [用于 Visual Studio 的 Azure Data Lake 工具](https://www.microsoft.com/download/details.aspx?id=49504)。

## <a name="configure-passwords-for-the-sandbox"></a>配置沙盒的密码

确保 Hortonworks 沙盒正在运行。 然后按照 [Hortonworks 沙盒入门](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords)文档中的步骤进行操作。 这些步骤配置 SSH `root` 帐户和 Ambari `admin` 帐户的密码。 从 Visual Studio 连接到沙盒时，将使用这些密码。

## <a name="connect-the-tools-to-the-sandbox"></a>将工具连接到沙盒

1. 打开 Visual Studio，选择“视图”，然后选择“服务器资源管理器”。

2. 在“服务器资源管理器”中，右键单击“HDInsight”项，然后选择“连接到 HDInsight Emulator”。

    ![服务器资源管理器的屏幕截图，其中已突出显示“连接到 HDInsight Emulator”](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. 在“连接到 HDInsight Emulator”对话框中，输入为 Ambari 配置的密码。

    ![对话框屏幕截图，其中突出显示了密码文本框](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    选择“下一步”继续。

4. 使用“密码”字段输入为 `root` 帐户配置的密码。 将其他字段保留默认值。

    ![对话框屏幕截图，其中突出显示了密码文本框](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    选择“下一步”继续。

5. 等待服务验证完成。 在某些情况下，验证可能失败，并提示更新配置。 如果验证失败，请选择“更新”，然后等待服务的配置和验证完成。

    ![对话框屏幕截图，其中突出显示了“更新”按钮](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > 更新过程使用 Ambari 将 Hortonworks 沙盒配置修改为用于 Visual Studio 的 Data Lake 工具所需的配置。

6. 验证完成后，请选择“完成”以完成配置。
    ![对话框屏幕截图，其中突出显示了“完成”按钮](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > 根据开发环境的速度以及分配给虚拟机的内存量，可能需要几分钟时间才能完成服务的配置和验证。

完成这些步骤后，服务器资源管理器中“HDInsight”部分下面会出现“HDInsight  本地群集”项。

## <a name="write-a-hive-query"></a>编写 Hive 查询

Hive 提供类似于 SQL 的查询语言 (HiveQL) 来处理结构化数据。 按照以下步骤了解如何针对本地群集运行按需查询。

1. 在“服务器资源管理器”中，右键单击前面添加的本地群集所对应的项，然后选择“编写 Hive 查询”。

    ![服务器资源管理器的屏幕截图，其中突出显示了“编写 Hive 查询”](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    此时会显示一个新的查询窗口。 在其中可以快速编写查询并将其提交到本地群集。

2. 在新查询窗口中输入以下命令：

        select count(*) from sample_08;

    要运行查询，请选择窗口顶部的“提交”。 将其他值（“Batch”和服务器名称）保留为默认值。

    ![查询窗口的屏幕截图，其中突出显示了“提交”按钮](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    还可以使用“提交”旁边的下拉菜单选择“高级”。 使用高级选项，可以在提交作业时提供其他选项。

    ![“提交脚本”对话框的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. 提交查询后，会显示作业状态。 作业状态显示 Hadoop 处理作业时有关作业的信息。 “作业状态”提供作业的状态。 状态会定期更新，也可以使用刷新图标手动刷新状态。

    ![“作业视图”对话框的屏幕截图，其中突出显示了“作业状态”](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    “作业状态”更改为“已完成”后，将显示有向无环图 (DAG)。 下图描述了处理 Hive 查询时 Tez 确定的执行路径。 Tez 是本地群集上的 Hive 使用的默认执行引擎。

    > [!NOTE]
    > 使用基于 Linux 的 HDInsight 群集时，Tez 也是默认引擎。 在基于 Windows 的 HDInsight 上，它不是默认引擎。 若要在这种群集上使用 Tez，必须在 Hive 查询的开头添加代码行 `set hive.execution.engine = tez;`。

    使用“作业输出”链接查看输出。 在本例中，输出为 823，即 sample_08 表中的行数。 可以使用“作业日志”和“下载 YARN 日志”链接查看有关作业的诊断信息。

4. 还可以交互方式运行 Hive 作业，方法是将“Batch”字段更改为“交互”。 然后选择“执行”。

    ![突出显示“交互式”和“执行”按钮的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    交互式查询会将处理期间生成的输出日志流式传输到“HiveServer2 输出”窗口。

    > [!NOTE]
    > 此信息与完成作业后使用“作业日志”链接所看到的信息相同。

    ![输出日志的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>创建 Hive 项目

还创建包含多个 Hive 脚本的项目。 当具有相关脚本或希望将脚本存储在版本控制系统中时，请使用该项目。

1. 在 Visual Studio 中，依次选择“文件”、“新建”、“项目”。

2. 在项目列表中，依次展开“模板”、“Azure Data Lake”，然后选择“HIVE (HDInsight)”。 在模板列表中，选择“Hive 示例”。 输入名称和位置，然后选择“确定”。

    ![“新建项目”窗口的屏幕截图，其中已突出显示“Azure Data Lake”、“HIVE”、“Hive 示例”和“确定”](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

**Hive 示例**项目包含两个脚本：**WebLogAnalysis.hql** 和 **SensorDataAnalysis.hql**。 可以使用窗口顶部的同一个“提交”按钮提交这些脚本。

## <a name="create-a-pig-project"></a>创建 Pig 项目

Hive 提供了类似 SQL 的语言来处理结构化数据，而 Pig 的工作方式则是对数据执行转换。 Pig 提供了一种语言 (Pig Latin)，可用于开发转换管道。 若要在本地群集上使用 Pig，请执行以下步骤：

1. 打开 Visual Studio，依次选择“文件”、“新建”、“项目”。 在项目列表中，依次展开“模板”、“Azure Data Lake”，然后选择“Pig (HDInsight)”。 在模板列表中，选择“Pig 应用程序”。 输入名称和位置，然后选择“确定”。

    ![“新建项目”窗口的屏幕截图，其中已突出显示“Azure Data Lake”、“Pig”、“Pig 应用程序”和“确定”](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. 输入以下文本作为使用此项目创建的 **script.pig** 文件内容。

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    尽管 Pig 使用的语言与 Hive 不同，但通过“提交”按钮运行作业的方式在这两种语言之间是一致的。 选择“提交”旁边的下拉列表会显示 Pig 的高级提交对话框。

    ![“提交脚本”对话框的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. 显示的作业状态和输出也与 Hive 查询相同。

    ![已完成的 Pig 作业的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>查看作业

使用 Data Lake 工具还可以轻松查看有关 Hadoop 上运行的作业的信息。 使用以下步骤可以查看已在本地群集上运行的作业。

1. 在“服务器资源管理器”中，右键单击本地群集，然后选择“查看作业”。 此时会显示已提交到群集的作业列表。

    ![服务器资源管理器的屏幕截图，其中突出显示了“查看作业”](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. 在作业列表中，选择一个作业查看其详细信息。

    ![作业浏览器的屏幕截图，其中突出显示了一个作业](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    显示的信息类似于运行 Hive 或 Pig 查询后看到的信息，包括用于查看输出和日志信息的链接。

3. 还可以在此处修改和重新提交作业。

## <a name="view-hive-databases"></a>查看 Hive 数据库

1. 在“服务器资源管理器”中，展开“HDInsight 本地群集”项，然后展开“Hive 数据库”。 此时将显示本地群集上的“默认”和“xademo”数据库。 展开一个数据库可显示该数据库中的表。

    ![服务器资源管理器的屏幕截图，其中已展开数据库](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. 展开一个表可显示该表的列。 若要快速查看数据，请右键单击某个表并选择“查看前 100 行”。

    ![服务器资源管理器的屏幕截图，其中已扩展表并选择了“查看前 100 行”](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>数据库和表属性

可以查看数据库或表的属性。  会在属性窗口中显示选定项的详细信息。 有关示例，请参阅以下屏幕截图中显示的信息：

![“属性”窗口的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>创建表

若要创建表，请右键单击某个数据库，然后选择“创建表”。

![服务器资源管理器的屏幕截图，其中突出显示了“创建表”](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

然后，可以使用表单创建表。 在以下屏幕截图的底部，可以看到用于创建表的原始 HiveQL。

![用于创建表的窗体的屏幕截图](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>后续步骤

* [了解 Hortonworks 沙盒的重要知识](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop 教程 - HDP 入门](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
<!--Update_Description: wording update-->