---
title: "Apache Kafka on HDInsight 简介 - Azure | Azure"
description: "了解 Apache Kafka on HDInsight：它的涵义和用途以及在何处可找到示例和入门信息。"
services: hdinsight
documentationcenter: 
author: hayley244
manager: digimobile
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 06/15/2017
ms.date: 09/04/2017
ms.author: v-haiqya
ms.openlocfilehash: e2b222daf81ee7a7fdcb2565fb083289911051ce
ms.sourcegitcommit: c2a877dfd2f322f513298306882c7388a91c6226
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Apache Kafka on HDInsight（预览版）简介

[Apache Kafka](https://kafka.apache.org) 是开源分布式流式处理平台，可用于构建实时流数据管道和应用程序。 Kafka 还提供类似于消息队列的消息中转站功能，可在其中向命名的数据流发布和订阅信息。 Kafka on HDInsight 在 Azure 云中提供托管的、高度可缩放且高度可用的服务。

## <a name="why-use-kafka-on-hdinsight"></a>为何使用 Kafka on HDInsight？

Kafka 提供以下功能：

* 发布-订阅消息模式：Kafka 提供生成者 API 用于向 Kafka 主题发布记录。 订阅某个主题时，会用到使用者 API。

* 流处理：Kafka 通常与 Apache Storm 或 Spark 配合使用，以实现实时流式处理。 Kafka 0.10.0.0（HDInsight 版本 3.5）中引入了流式处理 API，可用于构建流式处理解决方案，而无需使用 Storm 或 Spark。

* 水平规模：Kafka 分区可对 HDInsight 群集中节点进行流式处理。 使用者进程可与单个分区相关联，在使用记录时提供负载均衡。

* 有序传送：在每个分区内，记录按接收顺序存储在流中。 通过在使用者进程与分区之间建立一对一的关联，可以保证记录按顺序处理。

* 容错：可在节点之间复制分区以提供容错。

* 与 Azure 托管磁盘集成：托管磁盘为 HDInsight 群集中虚拟机使用的磁盘提供更高的规模和吞吐量。

    默认情况下针对 Kafka on HDInsight 启用托管磁盘，并且可以在创建 HDInsight 的过程中配置每个节点使用的磁盘数。 有关托管磁盘的详细信息，请参阅 [Azure 托管磁盘](../virtual-machines/windows/managed-disks-overview.md)。

    有关为 Kafka on HDInsight 配置托管磁盘的信息，请参阅[提高 Kafka on HDInsight 的可伸缩性](hdinsight-apache-kafka-scalability.md)。

## <a name="use-cases"></a>用例

* **消息传送**：由于支持发布-订阅消息模式，Kafka 通常用作消息中转站。

* **活动跟踪**：由于 Kafka 提供有序的日志记录，因此可用于跟踪和重建活动， 例如，网站上或应用程序内的用户操作。

* 
            **聚合**：使用流处理可以从不同的流聚合信息，并将信息组合并集中化为操作数据。

* **转换**：使用流处理可将多个输入主题的数据组合并扩充为一个或多个输出主题。

## <a name="next-steps"></a>后续步骤

单击以下链接了解如何使用 Apache Kafka on HDInsight：

* [Kafka on HDInsight 入门](hdinsight-apache-kafka-get-started.md)

* [使用 MirrorMaker 创建 Kafka on HDInsight 的副本](hdinsight-apache-kafka-mirroring.md)

* [将 Apache Storm 与 Kafka on HDInsight 结合使用](hdinsight-apache-storm-with-kafka.md)

* [将 Apache Spark 与 Kafka on HDInsight 结合使用](hdinsight-apache-spark-with-kafka.md)

* [通过 Azure 虚拟网络连接到 Kafka](hdinsight-apache-kafka-connect-vpn-gateway.md)