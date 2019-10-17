---
title: 排查 Azure 数据工厂连接器问题 | Microsoft Docs
description: 了解如何排查 Azure 数据工厂中的连接器问题。
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 08/26/2019
ms.date: 10/14/2019
ms.author: v-jay
ms.reviewer: craigg
ms.openlocfilehash: 5df469e0b3772507daec945e7b59e3c42fe1b54f
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275917"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>排查 Azure 数据工厂连接器问题

本文探讨 Azure 数据工厂中的连接器的常用故障排除方法。

## <a name="azure-sql-data-warehouse"></a>Azure SQL 数据仓库

### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>错误消息：将字符串转换为唯一标识符失败

- **症状**：使用分阶段复制和 PolyBase 将表格数据源（例如 SQL Server）中的数据复制到 Azure SQL 数据仓库时，遇到以下错误：

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **原因：** Azure SQL 数据仓库 PolyBase 无法将空字符串转换为 GUID。

- **解决方法**：在复制活动接收器中的 Polybase 设置下，将“use type default”选项设置为 false。 

### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>错误消息：预期的数据类型：DECIMAL(x,x)，违规值

- **症状**：使用分阶段复制和 PolyBase 将表格数据源（例如 SQL Server）中的数据复制到 SQL 数据仓库时，遇到以下错误：

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **原因：** Azure SQL 数据仓库 Polybase 无法将空字符串（null 值）插入十进制列。

- **解决方法**：在复制活动接收器中的 Polybase 设置下，将“use type default”选项设置为 false。 

### <a name="error-message-java-exception-messagehdfsbridgecreaterecordreader"></a>错误消息：Java 异常消息:HdfsBridge::CreateRecordReader

- **症状**：使用 PolyBase 将数据复制到 Azure SQL 数据仓库时遇到以下错误：

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **原因：** 可能的原因是架构（总列宽）太大（大于 1 MB）。 通过添加所有列的大小来检查目标 SQL 数据仓库表的架构：

    - Int -> 4 字节
    - Bigint -> 8 字节
    - Varchar(n),char(n),binary(n), varbinary(n) -> n 字节
    - Nvarchar(n), nchar(n) -> n*2 字节
    - Date -> 6 字节
    - Datetime/(2), smalldatetime -> 16 字节
    - Datetimeoffset -> 20 字节
    - Decimal -> 19 字节
    - Float -> 8 字节
    - Money -> 8 字节
    - Smallmoney -> 4 字节
    - Real -> 4 字节
    - Smallint -> 2 字节
    - Time -> 12 字节
    - Tinyint -> 1 字节

- **解决方法**：将列宽缩小至 1 MB 以下

- 或者，通过禁用 Polybase 来使用批量插入方法

### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>错误消息：不满足使用 HTTP 条件标头指定的条件

- **症状**：使用 SQL 查询从 Azure SQL 数据仓库提取数据时遇到以下错误：

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **原因：** Azure SQL 数据仓库在查询 Azure 存储中的外部表时遇到问题。

- **解决方法**：在 SSMS 中运行同一查询，检查是否看到相同的结果。 如果是，请开具 Azure SQL 数据仓库的支持票证，并提供 SQL 数据仓库服务器和数据库名称以进一步排查问题。

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>错误消息：请求过大

- **症状**：将数据复制到使用默认写入批大小的 Azure Cosmos DB 时，遇到错误“请求过大”。 **

- **原因：** Cosmos DB 将单个请求的大小限制为 2 MB。 公式为请求大小 = 单个文档大小 * 写入批大小。 如果文档过大，默认行为会导致请求过大。 可以优化写入批大小。

- **解决方法**：在复制活动接收器中，减小“写入批大小”值（默认值为 10000）。

### <a name="error-message-unique-index-constraint-violation"></a>错误消息：唯一索引约束冲突

- **症状**：将数据复制到 Cosmos DB 时遇到以下错误：

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception &#61; Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **原因：** 有两个可能的原因：

    - 如果使用“插入”作为写入行为，则此错误表示源数据包含具有相同 ID 的行/对象。 

    - 如果使用“更新插入”作为写入行为，并设置了容器的另一个唯一键，则此错误表示源数据中的行/对象使用了定义的唯一键的不同 ID，但使用了该键的相同值。 

- **解决方法**： 

    - 对于原因 1，请将“更新插入”设置为写入行为。 
    - 对于原因 2，请确保每个文档使用定义的唯一键的不同值。

### <a name="error-message-request-rate-is-large"></a>错误消息：请求速率太大

- **症状**：将数据复制到 Cosmos DB 时遇到以下错误：

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **原因：** 使用的请求单位大于 Cosmos DB 中配置的可用 RU。 在[此处](../cosmos-db/request-units.md#request-unit-considerations)了解 Cosmos DB 如何计算 RU。

- **解决方法**：下面是两种解决方法：

    1. **增加容器 RU**，使之大于 Cosmos DB 中的值，这可以提高复制活动的性能，不过会增大 Cosmos DB 的费用。 

    2. 将 **writeBatchSize** 减至更小的值（例如 1000），并将 **parallelCopies** 设置为更小的值（例如 1），这会导致复制运行性能比当前更糟，但不会增大 Cosmos DB 的费用。

### <a name="column-missing-in-column-mapping"></a>列映射中缺少列

- **症状**：导入用于列映射的 Cosmos DB 的架构时缺少某些列。 

- **原因：** ADF 从前 10 个 Cosmos DB 文档推断架构。 如果某些列/属性在这些文档中没有值，则它们不会被 ADF 检测到，因此也就不会显示。

- **解决方法**：可按如下所示优化查询，以强制在结果集中显示带有空值的列：（假设前 10 个文档中缺少“impossible”列）。 或者，可以手动添加要映射的列。

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>错误消息：读取器的 GuidRepresentation 为 CSharpLegacy

- **症状**：从包含 UUID 字段的 Cosmos DB MongoAPI/MongoDB 复制数据时遇到以下错误：

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **原因：** 可通过两种方式表示 BSON 中的 UUID - UuidStardard 和 UuidLegacy。 默认使用 UuidLegacy 来读取数据。 如果 MongoDB 中的 UUID 数据是 UuidStandard，则会出现错误。

- **解决方法**：在 MongoDB 连接字符串中，添加选项 "**uuidRepresentation=standard**"。 有关详细信息，请参阅 [MongoDB 连接字符串](connector-mongodb.md#linked-service-properties)。

## <a name="sftp"></a>SFTP

### <a name="error-message-invalid-sftp-credential-provided-for-sshpublickey-authentication-type"></a>错误消息：为“SshPublicKey”身份验证类型提供的 SFTP 凭据无效

- **症状**：使用 `SshPublicKey` 身份验证时遇到以下错误：

    ```
    Invalid Sftp credential provided for 'SshPublicKey' authentication type
    ```

- **原因：** 有 3 种可能的原因：

    1. 如果使用 ADF 创作 UI 来创作 SFTP 链接服务，则此错误表示选择使用的私钥格式不正确。 可以使用 PKCS#8 格式的 SSH 私钥，同时请注意，ADF 仅支持传统的 SSH 密钥格式。 更具体地说，PKCS#8 格式与传统密钥格式的差别在于，PKCS#8 密钥内容以“ *-----BEGIN ENCRYPTED PRIVATE KEY-----* ”开头，而传统密钥格式以“ *-----BEGIN RSA PRIVATE KEY-----* ”开头。
    2. 如果使用 Azure Key Vault 存储私钥内容，或使用编程方式创作 SFTP 链接服务，则此错误表示私钥内容不正确，它可能未经过 base64 编码。
    3. 凭据或私钥内容无效。

- **解决方法**： 

    - 对于原因 #1，请运行以下命令将密钥转换为传统密钥格式，然后在 ADF 创作 UI 中使用它。

        ```
        # Decrypt the pkcs8 key and convert the format to traditional key format
        openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file

        chmod 600 traditional_format_key_file

        # Re-encrypte the key file using passphrase
        ssh-keygen -f traditional_format_key_file -p
        ```

    - 对于原因 #2，若要生成此类字符串，客户可以使用以下两种方法：
    - 使用第三方 base64 转换工具：将整个私钥内容粘贴到 [Base64 Encode and Decode](https://www.base64encode.org/) 等工具中，将其编码为 base64 格式字符串，然后将此字符串粘贴到 Key Vault，或使用此值以编程方式创作 SFTP 链接服务。
    - 使用 C# 代码：

        ```c#
        byte[] keyContentBytes = File.ReadAllBytes(privateKeyPath);
        string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
        ```

    - 对于原因 #3，请使用其他工具仔细检查密钥文件或密码是否正确，并验证是否可以使用它来正常访问 SFTP 服务器。

