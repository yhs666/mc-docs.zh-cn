---
title: 使用 Azure HDInsight 对 Spark 进行故障排除 | Microsoft Docs
description: 获取有关使用 Apache Spark 和 Azure HDInsight 的常见问题的解答。
keywords: Azure HDInsight, Spark, 常见问题解答, 故障排除指南, 常见问题, 应用程序配置, Ambari
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: ''
editor: ''
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/15/2019
ms.date: 09/23/2019
ms.author: v-yiso
ms.openlocfilehash: 2ec773c65f45ed050097f4a43dfdc9dfa7fadd13
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70920975"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>使用 Azure HDInsight 对 Apache Spark 进行故障排除

了解处理 [Apache Ambari](https://ambari.apache.org/) 中的 [Apache Spark](https://spark.apache.org/) 有效负载时的最常见问题及其解决方法。

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a> 如何在群集上使用 Apache Ambari 配置 Apache Spark 应用程序？

### <a name="resolution-steps"></a>解决步骤

可以优化 Spark 配置值，避免出现 Apache Spark 应用程序 OutofMemoryError 异常。 以下步骤显示了 Azure HDInsight 中的默认 Spark 配置值： 

1. 在群集列表中选择“Spark2”。 

    ![从列表中选择群集](./media/apache-troubleshoot-spark/update-config-1.png)

2. 选择“配置”选项卡。 

    ![选择“配置”选项卡](./media/apache-troubleshoot-spark/update-config-2.png)

3. 在配置列表中，选择“Custom-spark2-defaults”。 

    ![选择 custom-spark-defaults](./media/apache-troubleshoot-spark/update-config-3.png)

4. 找到需要调整的值设置，例如 **spark.executor.memory**。 在本例中，值 **4608m** 太大。

    ![选择 spark.executor.memory 字段](./media/apache-troubleshoot-spark/update-config-4.png)

5. 将值设置为建议的设置。 建议为此设置使用值 **2048m**。

    ![将值更改为 2048m](./media/apache-troubleshoot-spark/update-config-5.png)

6. 保存值，并保存配置。 在工具栏上选择“保存”。 

    ![保存设置和配置](./media/apache-troubleshoot-spark/update-config-6a.png)

    如果有任何配置需要引以注意，系统会发出通知。 记下这些项，并选择“仍然继续”。  

    ![选择“仍然继续”](./media/apache-troubleshoot-spark/update-config-6b.png)

    编写有关配置更改的注释，并选择“保存”。 

    ![输入有关所做更改的注释](./media/apache-troubleshoot-spark/update-config-6c.png)

7. 每次保存配置时，系统都会提示重启服务。 选择“重启”。 

    ![选择“重启”](./media/apache-troubleshoot-spark/update-config-7a.png)

    确认重启。

    ![选择“确认全部重启”](./media/apache-troubleshoot-spark/update-config-7b.png)

    可以查看正在运行的进程。

    ![查看正在运行的进程](./media/apache-troubleshoot-spark/update-config-7c.png)

8. 可以添加配置。 在配置列表中，依次选择“Custom-spark2-defaults”、“添加属性”。  

    ![选择“添加属性”](./media/apache-troubleshoot-spark/update-config-8.png)

9. 定义新属性。 可以使用对话框为特定的设置（例如数据类型）定义单个属性。 或者，可以定义多个属性（每行定义一个属性）。 

    在本示例中，使用值 **4g** 定义了 **spark.driver.memory** 属性。

    ![定义新属性](./media/apache-troubleshoot-spark/update-config-9.png)

10. 根据步骤 6 和 7 中所述保存配置并重启服务。

这些更改会应用到整个群集，但可以在提交 Spark 作业时将其覆盖。

### <a name="additional-reading"></a>其他阅读材料

[在 HDInsight 群集上提交 Apache Spark 作业](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a> 如何在群集上使用 Jupyter Notebook 配置 Apache Spark 应用程序？

### <a name="resolution-steps"></a>解决步骤

1. 若要确定需要设置哪些 Spark 配置以及配置值是什么，请参阅导致 Apache Spark 应用程序出现 OutofMemoryError 异常的原因。

2. 在 Jupyter Notebook 的第一个单元格中的 **%%configure** 指令后面，使用有效的 JSON 格式指定 Spark 配置。 根据需要更改实际值：

    ![添加配置](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>其他阅读材料

[在 HDInsight 群集上提交 Apache Spark 作业](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a> 如何在群集上使用 Apache Livy 配置 Apache Spark 应用程序？

### <a name="resolution-steps"></a>解决步骤

1. 若要确定需要设置哪些 Spark 配置以及配置值是什么，请参阅[导致 Apache Spark 应用程序出现 OutofMemoryError 异常的原因](#what-causes-a-spark-application-outofmemoryerror-exception)。 

2. 使用 cURL 等 REST 客户端将 Spark 应用程序提交到 Livy。 使用如下所示的命令。 根据需要更改实际值：

```apache
curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.chinacloudapi.cn/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
```

### <a name="additional-reading"></a>其他阅读材料

[在 HDInsight 群集上提交 Apache Spark 作业](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a> 如何在群集上使用 spark-submit 配置 Apache Spark 应用程序？

### <a name="resolution-steps"></a>解决步骤

1. 若要确定需要设置哪些 Spark 配置以及配置值是什么，请参阅[导致 Apache Spark 应用程序出现 OutofMemoryError 异常的原因](#what-causes-a-spark-application-outofmemoryerror-exception)。

2. 使用如下所示的命令启动 spark-shell。 根据需要更改实际配置值： 

```apache
spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
```

### <a name="additional-reading"></a>其他阅读材料

[在 HDInsight 群集上提交 Apache Spark 作业](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* [Spark 内存管理概述](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview)。

* [在 HDInsight 群集上调试 Spark 应用程序](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)。



* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
