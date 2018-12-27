---
title: 什么是 Azure 事件中心？ | Azure
description: 了解 Azure 事件中心 - 每秒可引入数百万个事件的大数据流式处理服务。
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: overview
ms.custom: mvc
origin.date: 08/01/2018
ms.date: 12/10/2018
ms.author: v-biyu
ms.openlocfilehash: 26094ef67ad86956b9960b0e3cadd1ebbc82baae
ms.sourcegitcommit: 547436d67011c6fe58538cfb60b5b9c69db1533a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676916"
---
# <a name="what-is-azure-event-hubs"></a>什么是 Azure 事件中心？

Azure 事件中心是一个大数据流式处理平台和事件引入服务，每秒能够接收和处理数百万个事件。 事件中心可以处理和存储分布式软件和设备生成的事件、数据或遥测。 可以使用任何实时分析提供程序或批处理/存储适配器转换和存储发送到数据中心的数据。 

在下面这些常见方案中可以使用事件中心：

- 异常情况检测（欺诈/离群值）
- 应用程序日志记录
- 分析管道，例如点击流
- 实时仪表板
- 存档数据
- 事务处理
- 用户遥测数据处理
- 设备遥测数据流式处理 

## <a name="why-use-event-hubs"></a>为何使用事件中心？

仅当能够轻松处理并且能够从数据源获取即时见解时，数据才有价值。 事件中心提供低延迟、可无缝集成的分布式流处理平台，并在 Azure 的内部和外部提供数据和分析服务用于构建完整的大数据管道。

事件中心充当事件管道的“前门”，在解决方案体系结构中通常称作“事件引入器”。 事件引入器是位于事件发布者与事件使用者之间的组件或服务，可以将事件流的生成与这些事件的使用分离开来。 事件中心提供统一的流式处理平台和时间保留缓冲区，将事件生成者与事件使用者分离开来。 

以下部分介绍 Azure 事件中心服务的重要功能： 

## <a name="fully-managed-paas"></a>完全托管的 PaaS 

事件中心是托管的服务，其配置或管理开销极低，因此你可以专注于业务解决方案。
<!-- Not Available on  [Event Hubs for Apache Kafka ecosystems](event-hubs-for-kafka-ecosystem-overview.md)-->

## <a name="support-for-real-time-and-batch-processing"></a>支持实时处理和批处理

实时引入、缓冲、存储和处理流，以获取可行的见解。 事件中心使用[分区的使用者模型](event-hubs-features.md#partitions)，可让多个应用程序同时处理流，并允许控制处理速度。

在 [Azure Blob 存储](https://www.azure.cn/home/features/storage/) 中近乎实时地[捕获](event-hubs-capture-overview.md)数据，以便进行长期保留或微批处理。 可以基于用于派生实时分析的同一个流实现此目的。 设置捕获极其简单，无需管理费用即可运行它，并且可以使用事件中心 [吞吐量单位](event-hubs-features.md#throughput-units)自动进行缩放。 使用事件中心捕获可以专注于数据处理而不是数据捕获。
<!-- Not Available on [Azure Data Lake Store](https://www.azure.cn/home/features/data-lake-store/)-->

Azure 事件中心还能与 [Azure Functions](/azure-functions/) 集成，以构成无服务器体系结构。

## <a name="scalable"></a>可缩放 

使用事件中心可以从 MB 量级的数据流着手，然后逐步扩展到 GB 甚至 TB 量级的处理。 [自动扩充](event-hubs-auto-inflate.md)功能是用于根据用量需求扩展吞吐量单位数的众多选项之一。 

<!-- Not Available on ## Rich ecosystem-->


## <a name="key-architecture-components"></a>重要的体系结构组件

事件中心提供消息流处理功能，但其特征不同于传统的企业消息传送。 事件中心功能围绕高吞吐量和事件处理方案而构建。 事件中心包含以下[关键组件](event-hubs-features.md)：

- **事件生成者**：向事件中心发送数据的实体。 事件发布者可以使用 HTTPS 或 AMQP 1.0 发布事件。
    <!-- Not Available on Apache Kafka (1.0 and above)-->
- **分区**：每个使用者只读取消息流的特定子集或分区。
- **使用者组**：整个事件中心的视图（状态、位置或偏移量）。 使用者组使多个消费应用程序都有各自独立的事件流视图，并按自身步调和偏移量独立读取流。
- **吞吐量单位**：预先购买的容量单位，控制事件中心的吞吐量容量。
- **事件接收者**：从事件中心读取事件数据的实体。 所有事件中心使用者通过 AMQP 1.0 会话进行连接，事件在可用时通过会话传送。

下图显示了事件中心流处理体系结构：

![事件中心](./media/event-hubs-about/event_hubs_architecture.png)

## <a name="next-steps"></a>后续步骤

若要开始使用事件中心，请参阅以下文章：

1. **创建事件中心**：[Azure 门户](event-hubs-create.md)、[Azure CLI](event-hubs-quickstart-cli.md)、[Azure PowerShell](event-hubs-quickstart-powershell.md)、[Azure 资源管理器模板](event-hubs-resource-manager-namespace-event-hub.md)
2. **将事件发送到事件中心**：[.NET Standard](event-hubs-dotnet-standard-getstarted-send.md)、[.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)、[Java](event-hubs-java-get-started-send.md)、[Python](event-hubs-python-get-started-send.md)、[Node.js](event-hubs-node-get-started-send.md)、[Go](event-hubs-go-get-started-send.md)、[C](event-hubs-c-getstarted-send.md)
3. 从事件中心接收事件：[.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md)、[.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md)、[Java](event-hubs-java-get-started-receive-eph.md)、[Python](event-hubs-python-get-started-receive.md)、[Node.js](event-hubs-node-get-started-receive.md)、[Go](event-hubs-go-get-started-receive-eph.md)、[Apache Storm](event-hubs-storm-getstarted-receive.md)   

若要了解有关事件中心的详细信息，请参阅以下文章：

- [事件中心功能概述](event-hubs-features.md)
- [常见问题解答](event-hubs-faq.md)。

<!-- Update_Description: wording update -->