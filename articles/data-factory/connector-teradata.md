---
title: 使用 Azure 数据工厂从 Teradata 复制数据 | Microsoft Docs
description: 了解数据工厂服务的 Teradata 连接器，可通过它将数据从 Teradata 数据库复制到数据工厂支持作为接收器的数据存储中。
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 02/07/2018
ms.date: 08/12/2019
ms.author: v-jay
ms.openlocfilehash: 32ec4b7ff3df9774ab7407820f03d3929d8e0067
ms.sourcegitcommit: 871688d27d7b1a7905af019e14e904fabef8b03d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68908670"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>使用 Azure 数据工厂从 Teradata 复制数据

本文概述了如何使用 Azure 数据工厂中的复制活动从 Teradata 数据库复制数据。 它是基于概述复制活动总体的[复制活动概述](copy-activity-overview.md)一文。

## <a name="supported-capabilities"></a>支持的功能

可以将数据从 Teradata 数据库复制到任何支持的接收器数据存储。 有关复制活动支持作为源/接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

具体而言，此 Teradata 连接器支持：

- Teradata **版本 14.10、15.0、15.10、16.0、16.10 和 16.20**。
- 使用**基本**或 **Windows** 身份验证复制数据。
- 从 Teradata 源进行并行复制。 有关详细信息，请参阅[从 Teradata 进行并行复制](#parallel-copy-from-teradata)部分。

> [!NOTE]
>
> 从自承载集成运行时 v3.18 开始，Azure 数据工厂已升级 Teradata 连接器，该连接器以内置 ODBC 驱动程序为后盾，提供灵活的连接选项以及现成的并行复制功能来提高性能。 仍旧支持使用以 Teradata .NET 数据提供程序为后盾的旧版 Teradata 连接器，不过，我们建议今后使用新的连接器。 请注意，新路径需要一组不同的链接服务/数据集/复制源。 有关配置详细信息，请参阅相应的部分。

## <a name="prerequisites"></a>先决条件

如果 Teradata 数据库是不可公开访问的，则你需要设置自承载集成运行时。 有关集成运行时的详细信息，请参阅[自承载集成运行时](create-self-hosted-integration-runtime.md)。 Integration Runtime 提供内置 Teradata 驱动程序（从版本 3.18 开始），因此无需手动安装任何驱动程序。 该驱动程序需要在自承载 IR 计算机上安装“Visual C++ Redistributable 2012 Update 4”，如果你尚未安装它，请从[此处](https://www.microsoft.com/en-sg/download/details.aspx?id=30679)下载。

对于版本低于 3.18 的自承载 IR，需要在 Integration Runtime 计算机上安装[适用于 Teradata 的 .NET 数据提供程序](https://go.microsoft.com/fwlink/?LinkId=278886)版本 14 或以上。 

## <a name="getting-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 Teradata 连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

Teradata 链接的服务支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为：**Teradata** | 是 |
| connectionString | 指定连接到 Teradata 数据库实例所需的信息。 请参阅以下示例。<br/>还可以将密码放在 Azure 密钥保管库中，并从连接字符串中拉取 `password` 配置。 有关更多详细信息，请参阅[在 Azure Key Vault 中存储凭据](store-credentials-in-key-vault.md)一文。 | 是 |
| username | 指定用于连接到 Teradata 数据库的用户名。 使用 Windows 身份验证时适用。 | 否 |
| password | 指定为用户名指定的用户帐户的密码。 此外，还可以选择[引用 Azure Key Vault 中存储的机密](store-credentials-in-key-vault.md)。 <br>使用 Windows 身份验证时，或引用 Key Vault 中用于基本身份验证的密码时适用。 | 否 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 如[先决条件](#prerequisites)中所述，需要自承载集成运行时。 |是 |

**示例：使用基本身份验证**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>;Uid=<username>;Pwd=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**示例：使用 Windows 身份验证**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>",
            "username": "<username>",
            "password": "<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

> [!NOTE]
>
> 如果使用具有以下有效负载的，以 Teradata .NET 数据提供程序为后盾的 Teradata 链接服务，该服务仍按原样受到支持，但我们建议今后使用新服务。

**先前的有效负载：**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<Basic/Windows>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅数据集一文。 本部分提供 Teradata 数据集支持的属性列表。

从 Teradata 复制数据时，支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为：**TeradataTable** | 是 |
| database | Teradata 数据库的名称。 | 否（如果指定了活动源中的“query”） |
| 表 | Teradata 数据库中的表名。 | 否（如果指定了活动源中的“query”） |

**示例：**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

> [!NOTE]
>
> 如果使用如下所述的“RelationalTable”类型数据集，该数据集仍按原样受支持，但我们建议今后使用新数据集。

**先前的有效负载：**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 Teradata 源支持的属性列表。

### <a name="teradata-as-source"></a>以 Teradata 作为源

> [!TIP]
>
> 有关如何使用数据分区从 Teradata 有效加载数据的详细信息，请参阅[从 Teradata 进行并行复制](#parallel-copy-from-teradata)部分。

从 Teradata 复制数据时，复制活动的 **source** 节支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动源的 type 属性必须设置为：**TeradataSource** | 是 |
| query | 使用自定义 SQL 查询读取数据。 例如：`"SELECT * FROM MyTable"`。<br>启用分区加载时，需要在查询中挂接相应的内置分区参数。 请参阅[从 Teradata 进行并行复制](#parallel-copy-from-teradata)部分中的示例。 | 否（如果指定了数据集中的表） |
| partitionOptions | 指定用于从 Teradata 加载数据的数据分区选项。 <br>允许的值为：**None**（默认值）、**Hash** 和 **DynamicRange**。<br>启用分区选项（不是“None”）时，请在复制活动中配置 **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** 设置（例如 4），用于确定从 Teradata 数据库并行加载数据的并行度。 | 否 |
| partitionSettings | 指定数据分区的设置组。 <br>当分区选项不是 `None` 时适用。 | 否 |
| partitionColumnName | 指定并行复制范围分区使用的源列（**整数类型**）的名称。 如果未指定，系统会自动检测表的主键并将其用作分区列。 <br>当分区选项是 `Hash` 或 `DynamicRange` 时适用。 如果使用查询来检索源数据，请在 WHERE 中挂接 `?AdfHashPartitionCondition` 或 `?AdfRangePartitionColumnName`。 请参阅[从 Teradata 进行并行复制](#parallel-copy-from-teradata)部分中的示例。 | 否 |
| partitionUpperBound | 要从中复制数据的分区列的最大值。 <br>当分区选项是 `DynamicRange` 时适用。 如果使用查询来检索源数据，请在 WHERE 中挂接 `?AdfRangePartitionUpbound`。 请参阅[从 Teradata 进行并行复制](#parallel-copy-from-teradata)部分中的示例。 | 否 |
| PartitionLowerBound | 要从中复制数据的分区列的最小值。 <br>当分区选项是 `DynamicRange` 时适用。 如果使用查询来检索源数据，请在 WHERE 中挂接 `?AdfRangePartitionLowbound`。 请参阅[从 Teradata 进行并行复制](#parallel-copy-from-teradata)部分中的示例。 | 否 |

> [!NOTE]
>
> 如果使用了“RelationalSource”类型复制源，它仍按原样受支持，但不支持从 Teradata 进行并行加载（分区选项）的新内置功能。 建议今后使用此新功能。

**示例：使用基本查询但不使用分区复制数据**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "TeradataSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-teradata"></a>从 Teradata 进行并行复制

数据工厂 Teradata 连接器提供内置的数据分区，用于以极佳的性能从 Teradata 并行复制数据。 可以在“复制活动”>“Teradata 源”中找到数据分区选项：

![分区选项](./media/connector-teradata/connector-teradata-partition-options.png)

启用分区复制时，数据工厂将对 Teradata 源运行并行查询，以按分区加载数据。 可通过复制活动中的 **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** 设置配置和控制并行度。 例如，如果将 `parallelCopies` 设置为 4，则数据工厂会根据指定的分区选项和设置并行生成并运行 4 个查询，每个查询从 Teradata 数据库检索一部分数据。

建议同时启用并行复制和数据分区，尤其是从 Teradata 数据库加载大量数据时。 下面是适用于不同方案的建议配置：

| 方案                                                     | 建议的设置                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 从大型表进行完整加载                                   | **分区选项**：哈希。 <br><br/>在执行期间，数据工厂将自动检测 PK 列，对其应用哈希，然后按分区复制数据。 |
| 使用自定义查询加载大量数据                 | **分区选项**：哈希。<br>**查询**：`SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`。<br>**分区列**：指定用于应用哈希分区的列。 如果未指定，ADF 将自动检测 Teradata 数据集中指定的表的 PK 列。<br><br>在执行期间，数据工厂会将 `?AdfHashPartitionCondition` 替换为哈希分区逻辑，并发送到 Teradata。 |
| 使用自定义查询加载大量数据，某个整数列包含均匀分布的范围分区值 | **分区选项**：动态范围分区。<br>**查询**：`SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`。<br>**分区列**：指定用于对数据进行分区的列。 可以针对整数数据类型的列进行分区。<br>**分区上限**和**分区下限**：指定是否要对分区列进行筛选，以便仅检索介于下限和上限之间的数据。<br><br>在执行期间，数据工厂会将 `?AdfRangePartitionColumnName`、`?AdfRangePartitionUpbound` 和 `?AdfRangePartitionLowbound` 替换为每个分区的实际列名和值范围，并发送到 Teradata。 <br>例如，如果为分区列“ID”设置了下限 1、上限 80，并将并行复制设置为 4，则 ADF 会按 4 个分区检索 ID 介于 [1,20]、[21,40]、[41,60] 和 [61,80] 之间的数据。 |

**示例：使用哈希分区进行查询**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "Hash",
    "partitionSettings": {
        "partitionColumnName": "<hash_partition_column_name>"
    }
}
```

**示例：使用动态范围分区进行查询**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-teradata"></a>Teradata 的数据类型映射

从 Teradata 复制数据时，以下映射用于从 Teradata 数据类型映射到 Azure 数据工厂临时数据类型。 若要了解复制活动如何将源架构和数据类型映射到接收器，请参阅[架构和数据类型映射](copy-activity-schema-and-type-mapping.md)。

| Teradata 数据类型 | 数据工厂临时数据类型 |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |String |
| Clob |String |
| Date |DateTime |
| 小数 |小数 |
| Double |Double |
| Graphic |不支持。 在源查询中应用显式强制转换。 |
| Integer |Int32 |
| Interval Day |不支持。 在源查询中应用显式强制转换。 |
| Interval Day To Hour |不支持。 在源查询中应用显式强制转换。 |
| Interval Day To Minute |不支持。 在源查询中应用显式强制转换。 |
| Interval Day To Second |不支持。 在源查询中应用显式强制转换。 |
| Interval Hour |不支持。 在源查询中应用显式强制转换。 |
| Interval Hour To Minute |不支持。 在源查询中应用显式强制转换。 |
| Interval Hour To Second |不支持。 在源查询中应用显式强制转换。 |
| Interval Minute |不支持。 在源查询中应用显式强制转换。 |
| Interval Minute To Second |不支持。 在源查询中应用显式强制转换。 |
| Interval Month |不支持。 在源查询中应用显式强制转换。 |
| Interval Second |不支持。 在源查询中应用显式强制转换。 |
| Interval Year |不支持。 在源查询中应用显式强制转换。 |
| Interval Year To Month |不支持。 在源查询中应用显式强制转换。 |
| Number |Double |
| 期间（日期） |不支持。 在源查询中应用显式强制转换。 |
| 期间（时间） |不支持。 在源查询中应用显式强制转换。 |
| 期间（带时区的时间） |不支持。 在源查询中应用显式强制转换。 |
| 期间（时间戳） |不支持。 在源查询中应用显式强制转换。 |
| 期间（带时区的时间戳） |不支持。 在源查询中应用显式强制转换。 |
| SmallInt |Int16 |
| 时间 |TimeSpan |
| Time With Time Zone |TimeSpan |
| Timestamp |DateTime |
| Timestamp With Time Zone |DateTime |
| VarByte |Byte[] |
| VarChar |String |
| VarGraphic |不支持。 在源查询中应用显式强制转换。 |
| Xml |不支持。 在源查询中应用显式强制转换。 |


## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。
