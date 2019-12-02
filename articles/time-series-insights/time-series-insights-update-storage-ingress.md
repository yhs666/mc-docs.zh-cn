---
title: 预览版中的数据存储和引入 - Azure 时序见解 | Microsoft Docs
description: 了解 Azure 时序见解预览版中的数据存储和引入。
author: deepakpalled
ms.author: v-yiso
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/02/2019
ms.custom: seodec18
ms.openlocfilehash: dc2b05b61e6c0a0f41d25a61c32cc7c043ce3ade
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389599"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure 时序见解预览版中的数据存储和引入

本文介绍 Azure 时序见解预览版中对数据存储和入口的更新， 其中包括底层存储结构、文件格式和时序 ID 属性。 本文还介绍了底层引入过程、最佳做法和当前的预览版限制。

## <a name="data-ingress"></a>数据入口

Azure 时序见解环境包含一个用于收集、处理和存储时序数据的引入引擎。 规划环境时，需要考虑一些事项，以确保处理所有传入的数据，实现较高的引入规模，并最大限度地降低引入延迟（TSI 读取和处理事件源中的数据所花费的时间）。 在 Azure 时序见解预览版中，数据引入策略决定了数据可以源自何处，以及应采用哪种格式。

### <a name="ingress-policies"></a>引入策略

时序见解预览版支持以下事件源：

- [Azure IoT 中心](../iot-hub/about-iot-hub.md)
- [Azure 事件中心](../event-hubs/event-hubs-about.md)

时序见解预览版对每个实例最多支持两个事件源。
  
Azure 时序见解支持通过 Azure IoT 中心或 Azure 事件中心提交的 JSON。

> [!WARNING] 
> 将新事件源附加到时序见解预览版环境时，可能会遇到较高的初始引入延迟，具体取决于 IoT 中心或事件中心内当前的事件数。 引入数据时，这种较高的延迟预期应会下降，但如果你遇到的情况并非如此，请通过 Azure 门户提交支持票证来联系我们。

## <a name="ingress-best-practices"></a>有关引入的最佳做法

建议采用以下最佳做法：

* 在同一区域中配置时序见解和 IoT 中心或事件中心。 这可以降低网络问题造成的引入延迟。
* 通过计算预计引入速率并检查计算结果是否处于下面所列的受支持速率范围内，来规划规模需求
* 阅读[如何为引入和查询调整 JSON](./time-series-insights-update-how-to-shape-events.md) 来了解如何优化和调整 JSON 数据，以及当前的预览版限制。

### <a name="ingress-scale-and-limitations-in-preview"></a>预览版中的引入规模和限制

默认情况下，时序见解预览版最高支持每个环境 1 Mbps 的初始引入规模。 如果需要实现最高 16 MB/秒的吞吐量，请通过在 Azure 门户中提交支持票证来联系我们。 此外，每个分区限制为 0.5 MB/秒。 具体而言，这会对使用 IoT 中心的客户造成影响，因为 IoT 中心设备与分区之间存在关联。 如果一个网关设备使用自身的设备 ID 和连接字符串将消息转发到中心，在这些消息进入单个分区的情况下，即使事件有效负载指定不同的 TS ID，也存在达到 0.5 MB/秒限制的风险。 一般而言，引入速率被视为与组织中的设备数、事件发出频率以及事件大小因素有关。 计算引入速率时，IoT 中心用户应使用所用的中心连接数，而不应使用组织中的设备总数。 我们正在增强缩放支持， 此文档将会更新以反映这些改进。 

> [!WARNING]
> 对于使用 IoT 中心作为事件源的环境，请使用所用的中心设备数来计算引入速率。

有关吞吐量单位和分区的详细信息，请参阅以下链接：

* [IoT 中心规模](/iot-hub/iot-hub-scaling)
* [事件中心规模](/event-hubs/event-hubs-scalability#throughput-units)
* [事件中心分区](/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>数据存储

创建时序见解预览版即用即付 SKU 环境时，请创建两个 Azure 资源：

* 可以选择性地包含热存储功能的时序见解预览版环境。
* 用于存储冷数据的 Azure 存储常规用途 V1 Blob 帐户。

只能通过[时序查询](./time-series-insights-update-tsq.md)和 [Azure 时序见解预览版资源管理器](./time-series-insights-update-explorer.md)使用热存储中的数据。 

时序见解预览版以 [Parquet 文件格式](#parquet-file-format-and-folder-structure)将冷存储数据保存到 Azure Blob 存储中。 时序见解预览版以独占方式管理此冷存储数据，但允许将这些数据作为标准 Parquet 文件直接进行读取。

> [!WARNING]
> 冷存储数据所在 Azure Blob 存储帐户的所有者对该帐户中的所有数据拥有完全访问权限。 此访问权限包括“写入”和“删除”权限。 请不要编辑或删除时序见解预览版写入的数据，否则可能导致数据丢失。

### <a name="data-availability"></a>数据可用性

为了实现最佳查询性能，时序见解预览版会将数据分区并为其编制索引。 数据在编制索引后可供查询。 正在引入的数据量可能会影响这种可用性。

> [!IMPORTANT]
> 即将推出的时序见解正式版 (GA) 可在从事件源读取数据后的 60 秒内提供这些数据。 在预览版中，可能需要等待较长的时间才能看到数据。 如果遇到 60 秒以上的较高延迟，请通过 Azure 门户提交支持票证。

## <a name="azure-storage"></a>Azure 存储

本部分介绍与 Azure 时序见解预览版相关的 Azure 存储详细信息。

有关 Azure Blob 存储的全面介绍，请阅读[存储 Blob 简介](../storage/blobs/storage-blobs-introduction.md)。

### <a name="your-storage-account"></a>存储帐户

创建时序见解预览版即用即付环境时，会创建一个 Azure 存储常规用途 V1 Blob 帐户作为长期冷存储。  

时序见解预览版在 Azure 存储帐户中发布每个事件的最多两个副本。 初始副本包含按引入时间排序的事件，并且该副本始终会保留，因此你可以使用其他服务来访问它。 可以使用 Spark、Hadoop 和其他熟悉的工具来处理原始 Parquet 文件。 

时序见解预览版可将 Parquet 文件重新分区，以针对时序见解查询进行优化。 此重新分区的数据副本也会保存。

在公共预览版中，数据无限期存储在 Azure 存储帐户中。

### <a name="writing-and-editing-time-series-insights-blobs"></a>编写和编辑时序见解 Blob

为了确保查询性能和数据可用性，请不要编辑或删除时序见解预览版创建的任何 Blob。

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>从时序见解预览版访问和导出数据

你可能想要访问时序见解预览版资源管理器中显示的数据，以便与其他服务结合使用这些数据。 例如，可以使用数据在 Power BI 中生成报表，或使用 Azure 机器学习工作室训练机器学习模型。 或者，可以在 Jupyter Notebook 中使用数据进行转换、可视化和建模。

可通过三种常规方式访问数据：

* 通过时序见解预览版资源管理器。 可以从资源管理器将数据导出为 CSV 文件。 有关详细信息，请参阅[时序见解预览版资源管理器](./time-series-insights-update-explorer.md)。
* 通过时序见解预览版 API。 可以通过 `/getRecorded` 访问 API 终结点。 有关此 API 的详细信息，请参阅[时序查询](./time-series-insights-update-tsq.md)。
* 从 Azure 存储帐户直接访问。 需要对用于访问时序见解预览版数据的任何帐户拥有读取访问权限。 有关详细信息，请参阅[管理对存储帐户资源的访问权限](../storage/blobs/storage-manage-access-to-resources.md)。

### <a name="data-deletion"></a>数据删除

请不要删除时序见解预览版文件。 请只在时序见解预览版内部管理相关数据。

## <a name="parquet-file-format-and-folder-structure"></a>Parquet 文件格式和文件夹结构

Parquet 是一种开源的列式文件格式，旨在提高存储效率和性能。 出于这些原因，时序见解预览版使用 Parquet。 它按时序 ID 将数据分区，以提高大规模查询的性能。  

有关 Parquet 文件类型的详细信息，请参阅 [Parquet 文档](https://parquet.apache.org/documentation/latest/)。

时序见解预览版按如下方式存储数据的副本：

* 首先，初始副本按引入时间分区，并大致按抵达顺序存储数据。 数据驻留在 `PT=Time` 文件夹中：

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* 其次，重新分区的副本按时序 ID 的分组分区，驻留在 `PT=TsId` 文件夹中：

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

在这两种情况下，时间值对应于 Blob 创建时间。 将保留 `PT=Time` 文件夹中的数据。 `PT=TsId` 文件夹中的数据将不断针对查询进行优化，而不会保持静态。

> [!NOTE]
> * `<YYYY>` 映射到四位数的年份表示形式。
> * `<MM>` 映射到两位数的月份表示形式。
> * `<YYYYMMDDHHMMSSfff>` 映射到时间戳表示形式，其中包括四位数的年份 (`YYYY`)、两位数的月份 (`MM`)、两位数的日 (`DD`)、两位数的小时 (`HH`)、两位数的分钟 (`MM`)、两位数的秒 (`SS`) 和三位数的毫秒 (`fff`)。

时序见解预览版事件按如下所示映射到 Parquet 文件内容：

* 每个事件映射到单个行。
* 每一行包含带有事件时间戳的**时间戳**列。 时间戳属性永远不会为 null。 如果未在事件源中指定时间戳属性，该属性默认为**事件排队时间**。 时间戳始终为 UTC 时间。
* 每一行包含创建时序见解环境时定义的时序 ID 列。 属性名称包含 `_string` 后缀。
* 作为遥测数据发送的所有其他属性映射到以 `_string`（字符串）、`_bool`（布尔值）、`_datetime`（日期时间）或 `_double`（双精度值）结尾的列名称，具体取决于属性类型。
* 此映射方案适用于文件格式的第一个版本（以 **V=1** 表示）。 随着此功能的不断演进，名称可能会递增。

## <a name="next-steps"></a>后续步骤

- 阅读 [Azure 时序见解预览版存储和引入](./time-series-insights-update-storage-ingress.md)。

- 了解新型[数据建模](./time-series-insights-update-tsm.md)。
