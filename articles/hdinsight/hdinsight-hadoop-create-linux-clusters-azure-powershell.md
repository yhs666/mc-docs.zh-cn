---
title: 使用 PowerShell 创建 Hadoop 群集 - Azure HDInsight | Azure
description: 了解如何使用 Azure PowerShell 在 HDInsight 中的 Linux 上创建 Hadoop、HBase、Storm 或 Spark 群集。
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 02/21/2018
ms.date: 11/19/2018
ms.author: v-yiso
ms.openlocfilehash: 9f4b3267f5832219b847399b5c57d1a261b850dc
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52664211"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>使用 Azure PowerShell 在 HDInsight 中创建基于 Linux 的群集

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell 是一个功能强大的脚本编写环境，可用于在 Azure 中控制和自动执行工作负荷的部署和管理。 本文档介绍如何使用 Azure PowerShell 创建基于 Linux 的 HDInsight 群集。 此外，还提供了示例脚本。

> [!NOTE]
> Azure PowerShell 仅在 Windows 客户端上可用。 如果使用的是 Linux、Unix 或 Mac OS X 客户端，请参阅[使用 Azure 经典 CLI 创建基于 Linux 的 HDInsight 群集](hdinsight-hadoop-create-linux-clusters-azure-cli.md)，了解如何使用经典 CLI 创建群集。

## <a name="prerequisites"></a>先决条件
开始执行此过程之前请做好以下准备：

* Azure 订阅。 请参阅[获取 Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)。
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > 使用 Azure Service Manager 管理 HDInsight 资源的 Azure PowerShell 支持**已弃用**，已在 2017 年 1 月 1 日删除。 本文档中的步骤使用的是与 Azure Resource Manager 兼容的新 HDInsight cmdlet。
    >
    > 请按照[安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) 中的步骤安装最新版本的 Azure PowerShell。 如果脚本需要修改后才能使用与 Azure 资源管理器兼容的新 cmdlet，请参阅[迁移到适用于 HDInsight 群集的基于 Azure 资源管理器的开发工具](hdinsight-hadoop-development-using-azure-resource-manager.md)，了解详细信息。

## <a name="create-cluster"></a>创建群集

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

若要使用 Azure PowerShell 创建 HDInsight 群集，必须完成以下过程：

* 创建 Azure 资源组
* 创建 Azure 存储帐户
* 创建 Azure Blob 容器
* 创建 HDInsight 群集

以下脚本演示了如何创建新群集：

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount -EnvironmentName AzureChinaCloud
}

# If you have multiple subscriptions, set the one to use
# $subscriptionID = "<subscription ID to use>"
# Select-AzureRmSubscription -SubscriptionId $subscriptionID

# Get user input/default values
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
$location = Read-Host -Prompt "Enter the Azure region to create resources in"

# Create the resource group
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

$defaultStorageAccountName = Read-Host -Prompt "Enter the name of the storage account"

# Create an Azure storae account and container
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Type Standard_LRS `
    -Location $location
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageContext = New-AzureStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey

# Get information for the HDInsight cluster
$clusterName = Read-Host -Prompt "Enter the name of the HDInsight cluster"
# Cluster login is used to secure HTTPS services hosted on the cluster
$httpCredential = Get-Credential -Message "Enter Cluster login credentials" -UserName "admin"
# SSH user is used to remotely connect to the cluster using SSH clients
$sshCredentials = Get-Credential -Message "Enter SSH user credentials"

# Default cluster size (# of worker nodes), version, type, and OS
$clusterSizeInNodes = "4"
$clusterVersion = "3.5"
$clusterType = "Hadoop"
$clusterOS = "Linux"
# Set the storage container name to the cluster name
$defaultBlobContainerName = $clusterName

# Create a blob container. This holds the default data store for the cluster.
New-AzureStorageContainer `
    -Name $clusterName -Context $defaultStorageContext 

# Create the HDInsight cluster
New-AzureRmHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType $clusterType `
    -OSType $clusterOS `
    -Version $clusterVersion `
    -HttpCredential $httpCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.chinacloudapi.cn" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $clusterName `
    -SshCredential $sshCredentials
```

使用为群集登录指定的值创建群集的 Hadoop 用户帐户。 使用此帐户连接到 Web UI 或 REST API 等群集上托管的服务。

使用为 SSH 用户指定的值创建群集的 SSH 用户。 使用此帐户在群集上启动远程 SSH 会话和运行作业。 有关详细信息，请参阅[将 SSH 与 HDInsight 配合使用](hdinsight-hadoop-linux-use-ssh-unix.md)文档。

> [!IMPORTANT]
> 如果计划使用 32 个以上的辅助角色节点（在创建群集时配置或者是在创建之后通过扩展群集来配置），则还必须指定至少具有 8 个核心和 14 GB RAM 的头节点大小。
>
> 有关节点大小和相关费用的详细信息，请参阅 [HDInsight 定价](https://www.azure.cn/pricing/details/hdinsight/)。

创建群集可能需要 20 分钟。

## <a name="create-cluster-configuration-object"></a>创建群集：配置对象

还可以使用 `New-AzureRmHDInsightClusterConfig` cmdlet 创建 HDInsight 配置对象。 然后，可以修改此配置对象，为群集启用其他配置选项。 最后，使用 `New-AzureRmHDInsightCluster` cmdlet 的 `-Config` 参数以利用该配置。

下面的脚本创建了一个配置对象，用于在 HDInsight 群集类型上配置 ML Services。 该配置支持边缘节点、RStudio 和其他存储帐户。
```powershell
$additionalStorageAccountName = Read-Host -Prompt "Enter the name of the additional storage account"

# Create the additional storage account
New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName `
    -StorageAccountName $additionalStorageAccountName `
    -Location $location `
    -Type Standard_LRS

# Get the additional storage account key
$additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

$config = New-AzureRmHDInsightClusterConfig -ClusterType Hadoop

# Add an additional storage account
Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.chinacloudapi.cn" -StorageAccountKey $additionalStorageAccountKey

# Create a new HDInsight cluster using -Config
New-AzureRmHDInsightCluster `
    -ClusterName $clusterName `
    -ResourceGroupName $resourceGroupName `
    -HttpCredential $httpCredential `
    -Location $location `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.chinacloudapi.cn" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $defaultStorageContainerName  `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -OSType $clusterOS `
    -Version $clusterVersion `
    -SshCredential $sshCredentials `
    -Config $config
```

> [!WARNING]
> 不支持在 HDInsight 群集之外的其他位置使用存储帐户。 使用此示例时，请在与服务器相同的位置上创建其他存储帐户。

## <a name="customize-clusters"></a>自定义群集

* 请参阅[使用 Bootstrap 自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell)。
* 请参阅[使用脚本操作自定义 HDInsight 群集](hdinsight-hadoop-customize-cluster-linux.md)。

## <a name="delete-the-cluster"></a>删除群集

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>故障排除

如果在创建 HDInsight 群集时遇到问题，请参阅[访问控制要求](hdinsight-administer-use-portal-linux.md#create-clusters)。

## <a name="next-steps"></a>后续步骤

成功创建 HDInsight 群集后，请通过以下资源了解如何使用群集。

### <a name="hadoop-clusters"></a>Hadoop 群集

* [将 Hive 与 HDInsight 配合使用](hadoop/hdinsight-use-hive.md)
* [将 Pig 与 HDInsight 配合使用](hadoop/hdinsight-use-pig.md)
* [将 MapReduce 与 HDInsight 配合使用](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase 群集

* [HBase on HDInsight 入门](hbase/apache-hbase-tutorial-get-started-linux.md)
* [为 HBase on HDInsight 开发 Java 应用程序](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm 群集

* [为 Storm on HDInsight 开发 Java 拓扑](storm/apache-storm-develop-java-topology.md)
* [在 Storm on HDInsight 中使用 Python 组件](storm/apache-storm-develop-python-topology.md)
* [使用 Storm on HDInsight 部署和监视拓扑](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark 群集

* [使用 Scala 创建独立的应用程序](spark/apache-spark-create-standalone-application.md)
* [使用 Livy 在 Spark 群集中远程运行作业](spark/apache-spark-livy-rest-interface.md)
* [Spark 和 BI：使用 HDInsight 中的 Spark 和 BI 工具执行交互式数据分析](spark/apache-spark-use-bi-tools.md)
* [Spark 和机器学习：使用 HDInsight 中的 Spark 预测食品检查结果](spark/apache-spark-machine-learning-mllib-ipython.md)

