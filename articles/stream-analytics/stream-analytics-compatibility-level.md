---
title: 了解 Azure 流分析作业的兼容性级别
description: 了解如何设置 Azure 流分析作业的兼容性级别，并了解最新兼容性级别中的重大更改
services: stream-analytics
author: lingliw
ms.author: v-lingwu
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/19
ms.openlocfilehash: 98bf58df12621a3b57adcc3fdf141f7b089f9e74
ms.sourcegitcommit: df1adc5cce721db439c1a7af67f1b19280004b2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63824388"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Azure 流分析作业的兼容性级别

兼容性级别是指 Azure 流分析服务特定于版本的行为。 Azure 流分析是托管的服务，会定期进行功能更新和性能改进。 通常情况下，更新自动提供给最终用户。 但是，某些新功能可能会引入重大更改，例如，更改现有作业的行为、更改使用这些作业的数据的进程，等等。兼容性级别用于表示在流分析中引入的重大更改。 重大更改始终随新的兼容性级别一起引入。 

兼容性级别可确保现有作业运行而不会有任何失败。 创建新的流分析作业时，最佳做法是使用最新的兼容性级别来创建它。 
 
## <a name="set-a-compatibility-level"></a>设置兼容性级别 

兼容性级别控制流分析作业的运行时行为。 可以通过门户或通过[创建作业 REST API 调用](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job)来设置流分析作业的兼容性级别。 Azure 流分析目前支持两种兼容性级别 -“1.0”和“1.1”。 默认情况下，兼容性级别设置为“1.0”，这是在 Azure 流分析公开发行期间引入的。 若要更新默认值，请导航到现有的流分析作业 > 在“配置”部分选择“兼容性级别”选项，然后更改该值。 

请确保在更新兼容性级别之前停止作业。 如果作业处于运行状态，则无法更新兼容性级别。 

![Azure 门户中的流分析兼容性级别](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

在更新兼容性级别时，T-SQL 编译器会使用与所选兼容性级别相对应的语法来验证作业。 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>最新兼容性级别 (1.1) 中的重大更改

在兼容性级别 1.1 中引入了以下重大更改：

### <a name="geospatial-functions"></a>地理空间函数 

**以前的版本：** Azure 流分析使用地理计算。

**当前版本：** Azure 流分析允许计算几何投影的地理坐标。 地理空间函数的签名没有变化。 但是，其语义略有不同，与以往相比可以提高计算精度。

Azure 流分析支持地理空间参考数据索引编制。 可为包含地理空间元素的参考数据编制索引，以加快联接计算的速度。

更新的地理空间函数提供已知文本 (WKT) 地理空间格式的完整表达能力。 可以指定以前在 GeoJson 中所不支持的其他地理空间组件。

有关详细信息，请参阅[更新到 Azure 流分析中的地理空间功能 - 云和 IoT Edge](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/)。

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>针对使用多个分区的输入源的并行执行查询 

**以前的版本：** Azure 流分析查询要求使用 PARTITION BY 子句在不同的输入源分区之间并行化查询处理。

**当前版本：** 如果查询逻辑可在不同的输入源分区之间并行化，则 Azure 流分析会创建独立的查询实例，并且并行运行计算。

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>与 CosmosDB 输出的本机批量 API 集成

**以前的版本：** 更新插入行为是“插入或合并”。

**当前版本：** 与 CosmosDB 输出的本机批量 API 集成可以最大程度地提高吞吐量，并有效地处理限制请求。

更新插入行为是“插入或替换”。

### <a name="datetimeoffset-when-writing-to-sql-output"></a>写入到 SQL 输出时的 DateTimeOffset

**以前的版本：**[DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) 类型已调整为 UTC。

**当前版本：** 不再调整 DateTimeOffset。

### <a name="strict-validation-of-prefix-of-functions"></a>严格验证函数的前缀

**以前的版本：** 不对函数前缀执行严格验证。

**当前版本：** Azure 流分析严格验证函数前缀。 将前缀添加到内置函数会导致出错。 例如，`myprefix.ABS(�)` 不受支持。

将前缀添加到内置聚合也会导致出错。 例如，`myprefix.SUM(�)` 不受支持。

对用户定义的任何函数使用前缀“system”会导致出错。

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>不允许在 Cosmos DB 输出适配器中使用数组和对象作为键属性

**以前的版本：** 支持使用数组和对象类型作为键属性。

**当前版本：** 不再支持使用数组和对象类型作为键属性。


## <a name="next-steps"></a>后续步骤
* [Azure 流分析输入的故障排除](stream-analytics-troubleshoot-input.md)
<!-- Update_Description: update meta properties -->

<!--ms.date: 06/18/2018-->

