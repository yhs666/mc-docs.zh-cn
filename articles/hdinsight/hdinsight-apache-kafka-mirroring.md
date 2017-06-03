---
title: "镜像 Apache Kafka on HDInsight 群集 | Azure"
description: "了解如何使用 Kafka 的镜像功能，通过将主题镜像到辅助群集来维护 Kafka on HDInsight 群集的副本。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/13/2017
wacn.date: 
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 08618ee31568db24eba7a7d9a5fc3b079cf34577
ms.openlocfilehash: ebbb8d370eb9ffcb7bd23bd048203ef2150ad007
ms.contentlocale: zh-cn
ms.lasthandoff: 05/26/2017

---
# <a name="use-mirrormaker-to-create-a-replica-of-a-kafka-on-hdinsight-cluster-preview"></a>使用 MirrorMaker 创建 Kafka on HDInsight 群集的副本（预览）

Apache Kafka 包含镜像功能，可用于在不同的 Kafka 群集之间复制主题。 例如，在不同 Azure 区域中的 Kafka 群集之间复制记录。

镜像可以作为连续的进程运行，或者间接用作将数据从一个群集复制到另一个群集的方法。

> [!WARNING]
> 不应将镜像视为一种实现容错的方式。 主题中项的偏移在源群集与目标群集之间有所不同，因此客户端不能换用这两种群集。
> 
> 如果关心容错能力，应该为群集中的主题设置复制。 有关详细信息，请参阅 [Kafka on HDInsight 入门](hdinsight-apache-kafka-get-started.md)。

## <a name="prerequisites"></a>先决条件

* Azure 虚拟网络：源与目标 Kafka 群集必须能够直接相互通信。 HDInsight 不会在 Internet 上公开 Kafka API，因此源和目标群集必须在同一个 Azure 虚拟网络中。

* 两个 Kafka 群集：本文档使用 Azure Resource Manager 模板在 Azure 虚拟网络中创建两个 Kafka on HDInsight 群集。

## <a name="how-does-mirroring-work"></a>镜像的工作原理

镜像通过使用 MirrorMaker 工具（Apache Kafka 的一部分）来使用源群集上主题中的记录，然后在目标群集上创建本地副本。 MirrorMaker 使用一个或多个*使用者*从源群集读取记录，使用*生成者*将记录写入本地（目标）群集。

下图演示了镜像过程：

![镜像过程示意图](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

源群集与目标群集在节点和分区数目方面可以不同，主题中的偏移也可以不同。 镜像将维护用于分区的键值，因此，将会根据键保留记录顺序。

### <a name="mirroring-between-networks"></a>网络之间的镜像

如果需要在不同网络中的 Kafka 群集之间镜像，请注意以下附加事项：

* **网关**：网络必须能够在 TCPIP 级别通信。

* **名称解析**：每个网络中的 Kafka 群集必须能够使用主机名相互连接。 这可能需要在每个网络中设置一台域名系统 (DNS) 服务器，并将其配置为向其他网络转发请求。 

    创建 Azure 虚拟网络时，请不要使用网络提供的自动 DNS，必须指定一台自定义 DNS 服务器以及该服务器的 IP 地址。 创建虚拟网络后，必须创建使用该 IP 地址的 Azure 虚拟机，然后在该虚拟机上安装并配置 DNS 软件。

    > [!WARNING]
    > 请先创建并配置自定义 DNS 服务器，然后再将 HDInsight 安装到虚拟网络中。 无需对 HDInsight 进行其他配置即可使用针对虚拟网络配置的 DNS 服务器。

有关连接两个 Azure 虚拟网络的详细信息，请参阅[配置 VNet 到 VNet 的连接](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)。

## <a name="create-kafka-clusters"></a>创建 Kafka 群集

Apache Kafka on HDInsight 不提供通过公共 Internet 访问 Kafka 服务的权限。 与 Kafka 通信的所有组件必须与 Kafka 群集中的节点在同一个 Azure 虚拟网络中。 在本示例中，Kafka 源群集和目标群集位于同一个 Azure 虚拟网络中。 下图显示了这两个群集之间的通信流：

![Azure 虚拟网络中的 Kafka 源群集和目标群集示意图](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

> [!NOTE]
> 尽管 Kafka 本身的通信限于虚拟网络中，但可以通过 Internet 访问群集上的其他服务（如 SSH 和 Ambari）。 有关可用于 HDInsight 的公共端口的详细信息，请参阅 [HDInsight 使用的端口和 URI](hdinsight-hadoop-port-settings-for-services.md)。

尽管可以手动创建 Azure 虚拟网络和 Kafka 群集，但使用 Azure Resource Manager 模板会更容易。 使用以下步骤将 Azure 虚拟网络和两个 Kafka 群集部署到 Azure 订阅。

1. 使用以下按钮登录到 Azure，然后在 Azure 门户预览中打开模板。

    <a href="https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Azure Resource Manager 模板位于 **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet.json**。

2. 使用以下信息填充“自定义部署”  边栏选项卡上的条目：

    * **资源组**：创建一个组或选择现有组。 此组包含 HDInsight 群集。

    * **位置**：选择离你近的地理位置。 此位置必须匹配“设置”部分中的位置。

    * **基群集名称**：此值用作 Kafka 群集的基名称。 例如，输入 **hdi** 会创建名为 **source-hdi** 和 **dest-hdi** 的群集。

    * **群集登录用户名**：Kafka 源群集和目标群集的管理员用户名。

    * **群集登录密码**：Kafka 源群集和目标群集的管理员用户密码。

    * **SSH 用户名**：要为 Kafka 源群集和目标群集创建的 SSH 用户。

    * **SSH 密码**：Kafka 源群集和目标群集的 SSH 用户的密码。

    * **位置**：在其中创建群集的区域。

3. 单击“法律条款”，然后单击“创建”。

4. 确认已选中“固定到仪表板”复选框，然后单击“创建”。

创建资源后，系统会将用户重定向到包含群集和 Web 仪表板的资源组的边栏选项卡。

![虚拟网络和群集的“资源组”边栏选项卡](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> 请注意，HDInsight 群集的名称为 **source-BASENAME** 和 **dest-BASENAME**，其中 BASENAME 是为模板提供的名称。 在连接到群集的后续步骤中，会用到这些名称。

## <a name="create-topics"></a>创建主题

1. 使用 SSH 连接到 **源** 群集：

        ssh sshuser@source-BASENAME-ssh.azurehdinsight.cn

    将 **sshuser** 替换为创建群集时使用的 SSH 用户名。 将 **BASENAME** 替换为创建群集时使用的基名称。

    有关信息，请参阅[将 SSH 与 HDInsight 配合使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

2. 使用以下命令查找 Zookeeper 主机，设置 `SOURCE_ZKHOSTS` 变量，然后创建数个名为 `testtopic` 的新主题：

    ```bash
    SOURCE_ZKHOSTS=`grep -R zk /etc/hadoop/conf/yarn-site.xml | grep 2181 | grep -oPm1 "(?<=<value>)[^<]+"`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. 使用以下命令验证是否已创建主题：

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    响应包含 `testtopic`。

4. 使用以下命令查看此（**源**）群集的 Zookeeper 主机信息：

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    此命令将返回类似于以下文本的信息：

        zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:2181,zk6-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:2181

    请保存此信息。 下一部分会用到它。

## <a name="configure-mirroring"></a>配置镜像

1. 使用不同的 SSH 会话连接到 **目标** 群集：

        ssh sshuser@dest-BASENAME-ssh.azurehdinsight.cn

    将 **sshuser** 替换为创建群集时使用的 SSH 用户名。 将 **BASENAME** 替换为创建群集时使用的基名称。

    有关信息，请参阅[将 SSH 与 HDInsight 配合使用](hdinsight-hadoop-linux-use-ssh-unix.md)。

2. 使用以下命令创建一个 `consumer.properties` 文件，用于说明如何与 **源** 群集通信：

    ```bash
    nano consumer.properties
    ```

    使用以下文本作为 `consumer.properties` 文件的内容：

        zookeeper.connect=SOURCE_ZKHOSTS
        group.id=mirrorgroup

    将 **SOURCE_ZKHOSTS** 替换为**源**群集中的 Zookeeper 主机信息。

    此文件说明从 Kafka 源群集读取记录时要使用的使用者信息。 有关使用者配置的详细信息，请参阅 kafka.apache.org 上的 [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) （使用者配置）。

    依次按 **Ctrl + X**、**Y** 和 Enter 保存文件。

3. 在配置用来与目标群集通信的生成者之前，必须查找 **目标** 群集的中转站主机。 使用以下命令检索此信息：

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`sudo bash -c 'ls /var/lib/ambari-agent/data/command-[0-9]*.json' | tail -n 1 | xargs sudo cat | jq -r '["\(.clusterHostInfo.kafka_broker_hosts[]):9092"] | join(",")'`
    echo $DEST_BROKERHOSTS
    ```

    这些命令将返回类似于下面的信息：

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092

4. 使用以下命令创建一个 `producer.properties` 文件，用于说明如何与 **目标** 群集通信：

    ```bash
    nano producer.properties
    ```

    使用以下文本作为 `producer.properties` 文件的内容：

        bootstrap.servers=DEST_BROKERS
        compression.type=none

    将 **DEST_BROKERS** 替换为在上一步骤中获取的中转站信息。 

    有关生成者配置的详细信息，请参阅 kafka.apache.org 上的 [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) （生成者配置）。

## <a name="start-mirrormaker"></a>启动 MirrorMaker

1. 与 **目标** 群集建立 SSH 连接后，使用以下命令启动 MirrorMaker 进程：

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    本示例中使用的参数为：

    * **--consumer.config**：指定包含使用者属性的文件。 这些属性用于创建可从 *源* Kafka 群集读取记录的使用者。

    * **--producer.config**：指定包含生成者属性的文件。 这些属性用于创建可向 *目标* Kafka 群集写入记录的生成者。

    * **--whitelist**：MirrorMaker 从源群集复制到目标的主题列表。

    * **--num.streams**：要创建的使用者线程数。

    启动后，MirrorMaker 将返回类似于以下文本的信息：

    ```
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.chinacloudapp.cn:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. 与 **源** 群集建立 SSH 连接后，使用以下命令启动生成者，然后向主题发送消息：

    ```bash
    sudo apt -y install jq
    SOURCE_BROKERHOSTS=`sudo bash -c 'ls /var/lib/ambari-agent/data/command-[0-9]*.json' | tail -n 1 | xargs sudo cat | jq -r '["\(.clusterHostInfo.kafka_broker_hosts[]):9092"] | join(",")'`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    出现带有光标的空行时，请键入几条文本消息。 这些消息将发送到 **源** 群集上的主题。 完成后，按 **Ctrl + C** 结束生成者进程。

3. 从**目标**群集的 SSH 连接开始，使用 **Ctrl + C** 结束 MirrorMaker 进程。 然后使用以下命令验证是否已创建 `testtopic` 主题，以及该主题中的数据是否已复制到此镜像：

    ```bash
    DEST_ZKHOSTS=`grep -R zk /etc/hadoop/conf/yarn-site.xml | grep 2181 | grep -oPm1 "(?<=<value>)[^<]+"`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    主题列表现包含 `testtopic`，其在 MirrorMaster 将主题从源群集镜像到目标时创建。 从主题检索到的消息与在源群集上输入的消息相同。

## <a name="delete-the-cluster"></a>删除群集

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

由于本文档中的步骤在同一 Azure 资源组中创建两个群集，因此可以在 Azure 门户预览中删除该资源组。 删除资源组会删除按照本文档创建的所有资源（Azure 虚拟网络和群集使用的存储帐户）。

## <a name="next-steps"></a>后续步骤

本文档已介绍如何使用 MirrorMaker 创建 Kafka 群集的副本。 请使用以下链接探索 Kafka 的其他用法：

* [Apache Kafka MirrorMaker 文档](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) 。
* [Apache Kafka on HDInsight 入门](hdinsight-apache-kafka-get-started.md)
* [将 Apache Spark 与 Kafka on HDInsight 结合使用](hdinsight-apache-spark-with-kafka.md)
* [将 Apache Storm 与 Kafka on HDInsight 结合使用](hdinsight-apache-storm-with-kafka.md)
* [通过 Azure 虚拟网络连接到 Kafka](hdinsight-apache-kafka-connect-vpn-gateway.md)

