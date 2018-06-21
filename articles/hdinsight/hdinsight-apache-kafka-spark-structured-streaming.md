---
title: Apache Spark 结构化流式处理与 Kafka
description: 了解如何使用 Apache Spark 流式处理 (DStream) 将数据传入或传出 Apache Kafka。 本示例使用 Spark on HDInsight 中的 Jupyter notebook 流式传输数据。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 04/04/2018
ms.date: 05/28/2018
ms.author: v-yiso
ms.openlocfilehash: eca6b55e63371137627ea0deff650f12407ea6ec
ms.sourcegitcommit: c732858a9dec4902d5aec48245e2d84f422c3fd6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2018
ms.locfileid: "34449990"
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>将 Spark 结构化流式处理与 Kafka on HDInsight 配合使用

了解如何使用 Spark 结构化流式处理读取 Apache Kafka on Azure HDInsight 中的数据。

Spark 结构化流式处理是建立在 Spark SQL 上的流处理引擎。 这允许以与批量计算相同的方式表达针对静态数据的流式计算。 有关结构化流式处理的详细信息，请参阅 Apache.org 上的 [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html)（结构化流式处理编程指南 [Alpha]）。

> [!IMPORTANT]
> 此示例使用 Spark 2.2 on HDInsight 3.6。
>
> 本文档中的步骤创建一个 Azure 资源组，其中同时包含 HDInsight 上的 Spark 和 HDInsight 上的 Kafka 群集。 这些群集都位于一个 Azure 虚拟网络中，这样 Spark 群集便可与 Kafka 群集直接通信。
>
> 完成本文档中的步骤后，请记得删除这些群集，避免支付额外费用。

## <a name="create-the-clusters"></a>创建群集

Apache Kafka on HDInsight 不提供通过公共 Internet 访问 Kafka 中转站的权限。 使用 Kafka 的任何项都必须位于同一 Azure 虚拟网络中。 在本教程中，Kafka 和 Spark 群集位于同一 Azure 虚拟网络。 

下图显示通信在 Spark 和 Kafka 之间的流动方式：

![Azure 虚拟网络中的 Spark 和 Kafka 群集的关系图](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Kafka 服务仅限于虚拟网络内的通信。 通过 Internet 可访问群集上的其他服务，例如 SSH 和 Ambari。 有关可用于 HDInsight 的公共端口的详细信息，请参阅 [HDInsight 使用的端口和 URI](hdinsight-hadoop-port-settings-for-services.md)。

为了方便起见，以下步骤使用 Azure 资源管理器模板在虚拟网络中创建 Kafka 和 Spark 群集。

1. 使用以下按钮登录到 Azure，并在 Azure 门户中打开模板。
    
    <a href="https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.chinacloudapi.cn%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Azure 资源管理器模板位于 **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**。

    此模板可创建以下资源：

    * Kafka on HDInsight 3.6 群集。
    * Spark 2.2.0 on HDInsight 3.6 群集。
    * 包含 HDInsight 群集的 Azure 虚拟网络。

    > [!IMPORTANT]
    > 本示例使用的结构化流式处理笔记本需要 Spark on HDInsight 3.6。 如果使用早期版本的 Spark on HDInsight，则使用笔记本时会收到错误消息。

2. 使用以下信息填充“自定义模板”部分的条目：

    | 设置 | 值 |
    | --- | --- |
    | 订阅 | Azure 订阅 |
    | 资源组 | 包含资源的资源组。 |
    | 位置 | 创建资源时所在的 Azure 区域。 |
    | Spark 群集名称 | Spark 群集的名称。 |
    | Kafka 群集名称 | Kafka 群集的名称。 |
    | 群集登录用户名 | 群集的管理员用户名。 |
    | 群集登录密码 | 群集的管理员用户密码。 |
    | SSH 用户名 | 要为群集创建的 SSH 用户。 |
    | SSH 密码 | 用于 SSH 用户的密码。 |
   
    ![自定义模板的屏幕截图](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

4. 最后，选中“固定到仪表板”，并选择“购买”。 

> [!NOTE]
> 创建群集可能需要长达 20 分钟的时间。

## <a name="get-the-notebook"></a>获取 Notebook

[https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming) 上提供了本文档中所述的示例的代码。

## <a name="upload-the-notebooks"></a>上传 Notebook

若要将项目中的 Notebook 上传到 Spark on HDInsight 群集，请使用下列步骤：

1. 在 Web 浏览器中，连接到 Spark 群集上的 Jupyter Notebook。 在下列 URL 中，将 `CLUSTERNAME` 替换为你的 __Spark__ 群集名：

        https://CLUSTERNAME.azurehdinsight.cn/jupyter

    出现提示时，输入创建群集时使用的群集登录名（管理员）和密码。

2. 在页面右上角，使用“上传”按钮将 __spark-structured-streaming-kafka.ipynb__ 文件上传到群集。 选择“打开”开始上传。

    ![使用“上传”按钮选择并上传 Notebook](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![选择 KafkaStreaming.ipynb 文件](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. 在 Notebook 列表中找到 __spark-structured-streaming-kafka.ipynb__ 项，然后选择其旁边的“上传”按钮。

    ![若要上传笔记本，请使用 KafkaStreaming.ipynb 项的“上传”按钮](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>使用 Notebook

上传文件后，选择 __spark-structured-streaming-kafka.ipynb__ 项，以便打开 Notebook。 若要了解如何将 Spark 结构化流与 Kafka on HDInsight 配合使用，请按 Notebook 中的说明操作。

## <a name="clean-up-resources"></a>清理资源

若要清理本教程创建的资源，可以删除资源组。 删除资源组也会删除相关联的 HDInsight 群集，以及与资源组相关联的任何其他资源。

若要使用 Azure 门户删除资源组，请执行以下操作：

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“资源组”以显示资源组的列表。
2. 找到要删除的资源组，然后右键单击列表右侧的“更多”按钮 (...)。
3. 选择“删除资源组”，然后进行确认。

> [!WARNING]
> HDInsight 群集计费在创建群集之后便会开始，删除群集后才会停止。 HDInsight 群集按分钟收费，因此不再需要使用群集时，应将其删除。
> 
> 删除 Kafka on HDInsight 群集会删除存储在 Kafka 中的任何数据。

## <a name="next-steps"></a>后续步骤

现在你已了解如何使用 Spark 结构化流式处理，请参阅下列文档，深入了解如何使用 Spark 和 Kafka：

* [如何将 Spark 流式处理 (DStream) 与 Kafka 配合使用](hdinsight-apache-spark-with-kafka.md)。
* [开始使用 Jupyter Notebook 和 Spark on HDInsight](spark/apache-spark-jupyter-spark-sql.md)