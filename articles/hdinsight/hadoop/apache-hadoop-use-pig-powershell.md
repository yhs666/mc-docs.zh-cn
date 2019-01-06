---
title: 在 HDInsight 中将 Apache Pig 与 PowerShell 配合使用 - Azure
description: 了解如何使用 Azure PowerShell 将 Apache Pig 作业提交到 HDInsight 上的 Apache Hadoop 群集。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 05/09/2018
ms.date: 01/14/2019
ms.author: v-yiso
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 87e3b34d0773d02f4efb45f9a031d5a2a1b13692
ms.sourcegitcommit: 1456ace86f950acc6908f4f5a9c773b93a4d6acc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54029113"
---
# <a name="use-azure-powershell-to-run-apache-pig-jobs-with-hdinsight"></a>使用 Azure PowerShell 通过 HDInsight 运行 Apache Pig 作业

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

本文档提供使用 Azure PowerShell 向 Apache Hadoop on HDInsight 群集提交 Apache Pig 作业的示例。 Pig 允许使用可为数据转换建模的语言 (Pig Latin) 编写 MapReduce 作业，无需使用映射和化简函数。

> [!NOTE]  
> 本文档未详细描述示例中使用的 Pig Latin 语句的作用。 有关此示例中使用的 Pig Latin 的信息，请参阅[将 Apache Pig 与 HDInsight 上的 Apache Hadoop 配合使用](hdinsight-use-pig.md)。

## <a id="prereq"></a>先决条件

* **一个 Azure HDInsight 群集**

  > [!IMPORTANT]
  > Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 在 Windows 上停用](../hdinsight-component-versioning.md#hdinsight-windows-retirement)。

* **配备 Azure PowerShell 的工作站**。

## <a id="powershell"></a>运行 Apache Pig 作业

Azure PowerShell 提供 *cmdlet*，可让你在 HDInsight 上远程运行 Pig 作业。 PowerShell 在内部使用 REST 调用 HDInsight 群集上运行的 [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) 。

在远程 HDInsight 群集上运行 Pig 作业时，使用以下 Cmdlet：

* **Connect-AzureRmAccount**：在 Azure 订阅中进行 Azure PowerShell 身份验证。
* **New-AzureRmHDInsightPigJobDefinition**：使用指定的 Pig Latin 语句创建作业定义。
* **Start-AzureRmHDInsightJob**：将作业定义发送到 HDInsight 并启动作业。 将返回作业对象。
* **Wait-AzureRmHDInsightJob**：使用作业对象来检查作业的状态。 它会等到作业完成或超出等待时间。
* **Get-AzureRmHDInsightJobOutput**：用于检索作业的输出。

以下步骤演示了如何使用这些 Cmdlet 在 HDInsight 群集上运行作业。

1. 使用编辑器将以下代码保存为 **pigjob.ps1**。

    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount -EnvironmentName AzureChinaCloud
    }

    # Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #Store the Pig Latin into $QueryString
    $QueryString =  "LOGS = LOAD '/example/data/sample.log';" +
    "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
    "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
    "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
    "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
    "RESULT = order FREQUENCIES by COUNT desc;" +
    "DUMP RESULT;"

    #Create a new HDInsight Pig Job definition
    $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
        -Query $QueryString `
        -Arguments "-w"

    # Start the Pig job on the HDInsight cluster
    Write-Host "Start the Pig job ..." -ForegroundColor Green
    $pigJob = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $pigJobDefinition `
        -HttpCredential $creds

    # Wait for the Pig job to complete
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $pigJob.JobId `
        -HttpCredential $creds

    # Display the output of the Pig job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ClusterName $clusterName `
        -JobId $pigJob.JobId `
        -HttpCredential $creds
    ```

1. 打开新的 Windows PowerShell 命令提示符。 将目录更改为 **pigjob.ps1** 文件的所在位置，并使用以下命令来运行脚本：

        .\pigjob.ps1

    系统会提示用户登录到 Azure 订阅。 然后，要求输入 HDInsight 群集的 HTTPs/Admin 帐户名和密码。

2. 在作业完成时，它应返回类似于以下文本的信息：

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>故障排除

如果作业完成时未返回任何信息，请查看错误日志。 如果要查看此作业的错误信息，请将以下命令添加到 **pigjob.ps1** 文件的末尾，保存后重新运行该文件。

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

作业处理期间，此 cmdlet 返回写入到 STDERR 中的信息。

## <a id="summary"></a>摘要
Azure PowerShell 提供了一种简单方法，可在 HDInsight 群集上运行 Pig 作业、监视作业状态，以及检索输出。

## <a id="nextsteps"></a>后续步骤
有关 HDInsight 中的 Pig 的一般信息：

* [将 Apache Pig 与 Apache Hadoop on HDInsight 配合使用](hdinsight-use-pig.md)

有关 HDInsight 上 Hadoop 的其他使用方法的信息：

* [将 Apache Hive 与 Apache Hadoop on HDInsight 配合使用](hdinsight-use-hive.md)
* [将 MapReduce 与 HDInsight 上的 Apache Hadoop 配合使用](hdinsight-use-mapreduce.md)
