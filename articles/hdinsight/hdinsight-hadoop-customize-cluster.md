---
title: 使用脚本操作自定义 HDInsight 群集 - Azure | Azure
description: 了解如何使用脚本操作自定义 HDInsight 群集。
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 10/05/2016
ms.date: 03/04/2019
ms.author: v-yiso
ROBOTS: NOINDEX
ms.openlocfilehash: 9cc649d36da0bde70e26c9f1a562d330e87ab2a6
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665448"
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>使用脚本操作自定义基于 Windows 的 HDInsight 群集
在创建群集的过程中，可以使用脚本操作来调用[自定义脚本](hdinsight-hadoop-script-actions.md)，以便在群集上安装其他软件。

本文中的信息特定于基于 Windows 的 HDInsight 群集。 有关基于 Linux 的群集，请参阅[使用脚本操作自定义基于 Linux 的 HDInsight 群集](hdinsight-hadoop-customize-cluster-linux.md)。

> [!IMPORTANT]
> Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 在 Windows 上停用](hdinsight-component-versioning.md#hdinsight-windows-retirement)。

也可使用多种其他方法来自定义 HDInsight 群集，例如包含其他 Azure 存储帐户、更改 [Apache Hadoop](https://hadoop.apache.org/) 配置文件（core-site.xml、hive-site.xml 等），或者将共享库（例如 [Apache Hive](https://hive.apache.org/)[Apache Oozie](https://oozie.apache.org/)）添加到群集中的共同位置。 这些自定义可以通过使用 Azure PowerShell、Azure HDInsight .NET SDK 或 Azure 门户来完成。 有关详细信息，请参阅[在 HDInsight 中创建 Apache Hadoop 群集][hdinsight-provision-cluster]。

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>群集创建过程中的脚本操作
只能在创建群集过程中使用脚本操作。 下图演示了在创建过程中执行脚本操作的时间：

![群集创建期间的 HDInsight 群集自定义和阶段][img-hdi-cluster-states]

当脚本运行时，群集进入 **ClusterCustomization** 阶段。 在此阶段，脚本在系统管理员帐户下，以并行方式在群集中所有指定的节点上运行，而在节点上提供完全的系统管理员权限。

> [!NOTE]
> 你在 **ClusterCustomization** 阶段中于群集节点上拥有系统管理员权限，所以可以使用脚本来运行作业，例如停止和启动服务，包括 Hadoop 相关服务。 因此，在脚本中，必须在脚本完成运行之前，确定 Ambari 服务及其他 Hadoop 相关服务已启动并且正在运行。 这些服务必须在群集创建时，成功地确定群集的运行状况和状态。 如果更改群集上的任何影响这些服务的配置，必须使用所提供的帮助器函数。 有关帮助器函数的详细信息，请参阅[为 HDInsight 开发脚本操作脚本][hdinsight-write-script]。
>
>

脚本的输出以及错误日志文件存储在为群集指定的默认存储帐户中。 这些日志存储在名为 **u<\cluster-name-fragment><\time-stamp>setuplog** 的表中。 这是从群集中所有节点上（头节点和辅助节点）运行的脚本聚合的日志文件。

每个群集可接受多个脚本操作，这些脚本依其指定顺序被调用。 脚本可在头节点和/或辅助节点上运行。

HDInsight 提供了多个脚本用于在 HDInsight 群集上安装以下组件：

| Name | 脚本 |
| --- | --- |
| **安装 Apache Spark** | `https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1`。 请参阅[在 HDInsight 群集上安装并使用 Apache Spark][hdinsight-install-spark]。 |
| **安装 Apache Solr** | `https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1`。 请参阅[在 HDInsight 群集上安装并使用 Apache Solr](hdinsight-hadoop-solr-install.md)。 |
| **安装 Apache Giraph** | `https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1`。 请参阅[在 HDInsight 群集上安装并使用 Apache Giraph](hdinsight-hadoop-giraph-install.md)。 |
| **预加载 Apache Hive 库** | `https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1`。 请参阅[在 HDInsight 群集上添加 Apache Hive 库](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>使用 Azure 门户调用脚本
**从 Azure 门户**

1. 根据[在 HDInsight 中创建 Apache Hadoop 群集](hdinsight-hadoop-provision-linux-clusters.md)中的说明开始创建群集。
2. 在“脚本操作”边栏选项卡的“可选配置”下，单击“添加脚本操作”，提供有关脚本操作的详细信息，如下所示：

    ![使用脚本操作自定义群集](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "使用脚本操作自定义群集")

      |属性|值|  
      |---|---|
      |Name|指定脚本操作的名称。|
      |脚本 URI|指定要调用以自定义群集的脚本的 URI。|
      |头节点/辅助节点|指定在其上运行自定义脚本的节点（“头节点”或“辅助角色节点”）。|
      |parameters|根据脚本的需要，请指定参数。|

    按 ENTER 可添加多个脚本操作，以在群集上安装多个组件。
3. 单击“选择”可保存脚本操作配置并继续执行群集创建。

## <a name="call-scripts-using-azure-powershell"></a>使用 Azure PowerShell 调用脚本
以下 PowerShell 脚本演示如何在基于 Windows 的 HDInsight 群集上安装 Spark。  

```powershell  
    # Provide values for these variables
    $subscriptionID = "<Azure Subscription ID>" # After "Connect-AzureRmAccount -EnvironmentName AzureChinaCloud", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "CHINA EAST" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.chinacloudapi.cn" `
                -DefaultStorageAccountKey $defaultStorageAccountKey

    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config

To install other software, you will need to replace the script file in the script:

When prompted, enter the credentials for the cluster. It can take several minutes before the cluster is created.

## Call scripts using .NET SDK
The following sample demonstrates how to install Apache Spark on Windows based HDInsight cluster. To install other software, you will need to replace the script file in the code.

**To create an HDInsight cluster with Spark**

1. Create a C# console application in Visual Studio.
2. From the Nuget Package Manager Console, run the following command.

    ```powershell  
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
    ```

1. Use the following using statements in the Program.cs file:

    ```csharp
        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
    ```
4. Place the code in the class with the following:

    ```csharp
        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private static Uri BaseUri = new Uri("https://management.chinacloudapi.cn/");
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "China East";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken, BaseUri);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.chinacloudapi.cn/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.chinacloudapi.cn/",
                ClientId,
                new Uri("urn:ietf:wg:oauth:2.0:oob"),
                PromptBehavior.Always,
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(BaseUri, new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
    ```

5. Press **F5** to run the application.

## Support for open-source software used on HDInsight clusters
The Azure HDInsight service is a flexible platform that enables you to build big-data applications in the cloud by using an ecosystem of open-source technologies formed around Hadoop. Azure provides a general level of support for open-source technologies, as discussed in the **Support Scope** section of the <a href="https://www.azure.cn/support/faq/" target="_blank">Azure Support FAQ website</a>. The HDInsight service provides an additional level of support for some of the components, as described below.

There are two types of open-source components that are available in the HDInsight service:

* **Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster. For example, Apache Hadoop YARN ResourceManager, the Hive query language (HiveQL), and the Apache Mahout library belong to this category. A full list of cluster components is available in [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.
* **Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.

Built-in components are fully supported, and Azure Support will help to isolate and resolve issues related to these components.

> [!WARNING]
> Components provided with the HDInsight cluster are fully supported and Azure Support will help to isolate and resolve issues related to these components.
>
> Custom components receive commercially reasonable support to help you to further troubleshoot the issue. This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found. For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [Azure CSDN](http://azure.csdn.net). Also Apache projects have project sites on [https://apache.org](https://apache.org), for example: [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/).
>
>

The HDInsight service provides several ways to use custom components. Regardless of how a component is used or installed on the cluster, the same level of support applies. Below is a list of the most common ways that custom components can be used on HDInsight clusters:

1. Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.
2. Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on the cluster nodes.
3. Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters. These samples are provided without support.

## Develop Script Action scripts
See [Develop Script Action scripts for HDInsight][hdinsight-write-script].

## See also
* [Create Apache Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how to create an HDInsight cluster by using other custom options.
* [Develop Script Action scripts for HDInsight][hdinsight-write-script]
* [Install and use Apache Spark on HDInsight clusters][hdinsight-install-spark]
* [Install and use Apache Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).
* [Install and use Apache Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: https://docs.microsoft.com/powershell/azureps-cmdlets-docs

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster creation"
