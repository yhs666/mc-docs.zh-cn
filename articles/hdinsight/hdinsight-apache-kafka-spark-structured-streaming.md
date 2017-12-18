---
title: "Apache Spark 结构化流式处理与 Kafka - Azure HDInsight | Microsoft Docs"
description: "了解如何使用 Apache Spark 流式处理 (DStream) 将数据传入或传出 Apache Kafka。 本示例使用 Spark on HDInsight 中的 Jupyter notebook 流式传输数据。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 11/07/2017
ms.date: 12/25/2017
ms.author: v-yiso
ms.openlocfilehash: 90a6dfa964ba93bbd8bed6c75eef9b3d313642c4
ms.sourcegitcommit: 25dbb1efd7ad6a3fb8b5be4c4928780e4fbe14c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>将 Spark 结构化流式处理与 Kafka on HDInsight 配合使用

了解如何使用 Spark 结构化流式处理读取 Apache Kafka on Azure HDInsight 中的数据。

Spark 结构化流式处理是建立在 Spark SQL 上的流处理引擎。 这允许以与批量计算相同的方式表达针对静态数据的流式计算。 有关结构化流式处理的详细信息，请参阅 Apache.org 上的 [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)（结构化流式处理编程指南 [Alpha]）。

> [!IMPORTANT]
> 此示例使用了 Spark 2.1 on HDInsight 3.6。 结构化流式处理在 Spark 2.1 上被视作“alpha”。
>
> 本文档中的步骤创建了一个包含 Spark on HDInsight 和 Kafka on HDInsight 群集的 Azure 资源组。 这些群集都位于一个 Azure 虚拟网络中，这样 Spark 群集便可与 Kafka 群集直接通信。
>
> 完成本文档中的步骤后，请记得删除这些群集，避免支付额外费用。

## <a name="create-the-clusters"></a>创建群集

Apache Kafka on HDInsight 不提供通过公共 Internet 访问 Kafka 中转站的权限。 若要与 Kafka 通信，必须与 Kafka 群集中的节点在同一 Azure 虚拟网络中。 在此示例中，Kafka 群集和 Spark 群集都位于一个 Azure 虚拟网络中。 下图显示了这两个群集之间通信的流动方式：

![Azure 虚拟网络中的 Spark 和 Kafka 群集图表](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Kafka 服务仅限于虚拟网络内的通信。 通过 Internet 可访问群集上的其他服务，例如 SSH 和 Ambari。 有关可用于 HDInsight 的公共端口的详细信息，请参阅 [HDInsight 使用的端口和 URI](hdinsight-hadoop-port-settings-for-services.md)。

虽然可以手动创建 Azure 虚拟网络、Kafka 和 Spark 群集，但是使用 Azure Resource Manager 模板会更容易。 使用以下步骤将 Azure 虚拟网络、Kafka 和 Spark 群集部署到 Azure 订阅。

1. 使用以下按钮登录到 Azure，并在 Azure 门户中打开模板。
    
    <a href="https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.chinacloudapi.cn%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    如需 Azure 资源管理器模板，请访问 **https://hditutorialdata.blob.core.chinacloudapi.cn/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**。

    此模板可创建以下资源：

    * Kafka on HDInsight 3.6 群集。
    * Spark on HDInsight 3.6 群集。
    * 包含 HDInsight 群集的 Azure 虚拟网络。

    > [!IMPORTANT]
    > 本示例使用的结构化流式处理笔记本需要 Spark on HDInsight 3.6。 如果使用早期版本的 Spark on HDInsight，则使用笔记本时会收到错误消息。

2. 使用以下信息填充“自定义部署”部分中的条目：
   
    ![HDInsight 自定义部署](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **资源组**：创建一个组或选择现有组。 此组包含 HDInsight 群集。

    * **位置**：选择在地理上邻近的位置。

    * **基群集名称**：此值用作 Spark 和 Kafka 群集的基名称。 例如，输入 **hdi** 创建名为 spark-hdi__ 的 Spark 群集和名为 **kafka-hdi** 的 Kafka 群集。

    * **群集登录用户名**：Spark 和 Kafka 群集的管理员用户名。

    * **群集登录密码**：Spark 和 Kafka 群集的管理员用户密码。

    * **SSH 用户名**：要为 Spark 和 Kafka 群集创建的 SSH 用户。

    * **SSH 密码**：Spark 和 Kafka 群集的 SSH 用户密码。

3. 阅读“条款和条件”，并选择“我同意上述条款和条件”。

4. 最后，选中“固定到仪表板”，并选择“购买”。 创建群集大约需要 20 分钟时间。

创建资源后，会显示摘要页面。

![VNet 和群集的资源组信息](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> 请注意，HDInsight 群集的名称为 spark-BASENAME 和 kafka-BASENAME，其中 BASENAME 是为模板提供的名称。 在连接到群集的后续步骤中，会用到这些名称。

## <a name="get-the-kafka-brokers"></a>获取 Kafka 中转站

本示例中的代码连接到 Kafka 群集中的 Kafka 中转站主机。 若要查找两个 Kafka 中转站主机的地址，请使用以下 PowerShell 或 Bash 示例：

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.cn/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

出现提示时，输入群集登录（管理员）帐户的密码

> [!NOTE]
> 此示例需要 `$CLUSTERNAME` 包含 Kafka 群集的名称。
>
> 本示例使用 [jq](https://stedolan.github.io/jq/) 实用程序来分析 JSON 文档外的数据。

输出与以下文本类似：

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.chinacloudapp.cn:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

保存此信息，因为本文档的后面部分还将用到此信息。

## <a name="get-the-notebooks"></a>获取 Notebook

本文档中描述的示例代码位于 [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming)。

## <a name="upload-the-notebooks"></a>上传 Notebook

使用下列步骤，将项目中的 Notebook 上传到 Spark on HDInsight 群集：

1. 在 Web 浏览器中，连接到 Spark 群集上的 Jupyter Notebook。 在下列 URL 中，将 `CLUSTERNAME` 替换为 Kafka 群集名：

        https://CLUSTERNAME.azurehdinsight.cn/jupyter

    出现提示时，输入创建群集时使用的群集登录名（管理员）和密码。

2. 在页面右上角，使用“上传”按钮将“Stream-Tweets-To_Kafka.ipynb”文件上传到群集。 选择“打开”开始上传。

    ![使用“上传”按钮来选择并上传笔记本](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![选择 KafkaStreaming.ipynb 文件](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. 在笔记本列表中找到“Stream-Tweets-To_Kafka.ipynb”项，然后选择其旁边的“上传”按钮。

    ![若要上传笔记本，请使用 KafkaStreaming.ipynb 项的“上传”按钮](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. 重复步骤 1-3，加载“Spark-Structured-Streaming-From-Kafka.ipynb”Notebook。

## <a name="load-tweets-into-kafka"></a>将推文加载到 Kafka

上传文件后，选择“Stream-Tweets-To_Kafka.ipynb”项打开 Notebook。 在 Notebook 中按照步骤将推文加载到 Kafka。

## <a name="process-tweets-using-spark-structured-streaming"></a>使用 Spark 结构化流式处理来处理推文

在 Jupyter Notebook 主页上，选择“Spark-Structured-Streaming-From-Kafka.ipynb”项。 在笔记本中，按照步骤使用 Spark 结构化流式处理加载 Kafka 中的推文。

## <a name="next-steps"></a>后续步骤

现在你已了解如何使用 Spark 结构化流式处理，请参阅下列文档，深入了解如何使用 Spark 和 Kafka：

* [如何将 Spark 流式处理 (DStream) 与 Kafka 配合使用](hdinsight-apache-spark-with-kafka.md)。
* [开始使用 Jupyter Notebook 和 Spark on HDInsight](spark/apache-spark-jupyter-spark-sql.md)