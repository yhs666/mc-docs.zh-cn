---
title: 使用 .NET 和 HDInsight 运行 Apache Sqoop 作业 - Azure
description: 了解如何使用 HDInsight .NET SDK 在 Apache Hadoop 群集和 Azure SQL 数据库之间运行 Apache Sqoop 导入和导出。
keywords: sqoop 作业
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
origin.date: 05/16/2018
ms.date: 01/14/2019
ms.author: v-yiso
ms.openlocfilehash: 281dead6769fdddf15665178e720ef0537032ae0
ms.sourcegitcommit: 1456ace86f950acc6908f4f5a9c773b93a4d6acc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54029149"
---
# <a name="run-apache-sqoop-jobs-by-using-net-sdk-for-apache-hadoop-in-hdinsight"></a>使用 HDInsight 中用于 Apache Hadoop 的 .NET SDK 运行 Apache Sqoop 作业
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

了解如何使用 Azure HDInsight .NET SDK 运行 HDInsight 中的 Apache Sqoop 作业，以在 HDInsight 群集和 Azure SQL 数据库或 SQL Server 数据库之间进行导入和导出。

> [!NOTE]
> 尽管可以对基于 Windows 或 Linux 的 HDInsight 群集使用本文中的步骤，但是，只能从 Windows 客户端执行这些步骤。 若要选择其他方法，使用本文顶部的选项卡选择器。
> 

## <a name="prerequisites"></a>先决条件
开始学习本教程之前，必须具备以下项：

* HDInsight 中的 Apache Hadoop 群集。 有关详细信息，请参阅[创建群集和 SQL 数据库](hdinsight-use-sqoop.md#create-cluster-and-sql-database)。

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>通过 .NET SDK 使用 HDInsight 群集上的 Sqoop
HDInsight .NET SDK 提供 .NET 客户端库，以便可轻易从 .NET 中使用 HDInsight 群集。 本部分创建一个 C# 控制台应用程序，以便将 hivesampletable 导出到本教程前面创建的 Azure SQL 数据库表。

## <a name="submit-a-sqoop-job"></a>提交 Sqoop 作业

1. 在 Visual Studio 中创建 C# 控制台应用程序。

2. 在 Visual Studio 包管理器控制台中，运行以下 NuGet 命令将包导入：
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. 在 Program.cs 文件中使用以下代码：

        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.cn";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitSqoopJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";

                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";

                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.chinacloudapi.cn;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;

                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };

                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }

4. 若要运行程序，请选择 F5 键。 

## <a name="limitations"></a>限制
基于 Linux 的 HDInsight 存在以下限制：

* 批量导出：用于将数据导出到 Microsoft SQL Server 或 Azure SQL 数据库的 Sqoop 连接器目前不支持批量插入。

* 批处理：如果使用 `-batch` 开关，Sqoop 将执行多次插入而不是批处理插入操作。

## <a name="next-steps"></a>后续步骤
现在你已了解如何使用 Sqoop。 若要了解更多信息，请参阅以下文章：

* [将 Apache Oozie 和 HDInsight 配合使用](../hdinsight-use-oozie.md)：在 Oozie 工作流中使用 Sqoop 操作。
* [使用 HDInsight 分析航班延误数据](../hdinsight-analyze-flight-delay-data.md)：使用 Apache Hive 分析航班延误数据，然后使用 Sqoop 将数据导出到 Azure SQL 数据库。
* [将数据上传到 HDInsight](../hdinsight-upload-data.md)：了解将数据上传到 HDInsight 或 Azure Blob 存储的其他方法。

