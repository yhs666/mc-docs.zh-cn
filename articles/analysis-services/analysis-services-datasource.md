---
title: Azure Analysis Services 中支持的数据源 | Azure
description: 介绍 Azure Analysis Services 中数据模型支持的数据源。
author: rockboyfor
manager: digimobile
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 10/18/2018
ms.date: 12/17/2018
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: 75aee3300a001538d37d071a9cffbf458969a334
ms.sourcegitcommit: 833865e1f1e99b3acd10781451eed636cc7cc810
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "53157423"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Azure Analysis Services 中支持的数据源

对于 Azure Analysis Services 和 SQL Server Analysis Services，Visual Studio 中的“获取数据”或“导入向导”中显示的数据源和连接器都会显示。 但是，并非显示的所有数据源和连接器在 Azure Analysis Services 中都受支持。 你可以连接到的数据源的类型取决于许多因素，例如模型兼容性级别、可用的数据连接器、身份验证类型、提供程序和本地数据网关支持。 

## <a name="azure-data-sources"></a>Azure 数据源

|数据源  |内存中  |直接连接  |
|---------|---------|---------|
|Azure SQL 数据库     |   是      |    是      |
|Azure SQL 数据仓库     |   是      |   是       |
|Azure Blob 存储*     |   是       |    否      |
|Azure 表存储*    |   是       |    否      |
|Azure Cosmos DB*     |  是        |  否        |
|Azure HDInsight HDFS*     |     是     |   否       |
|Azure HDInsight Spark*     |   是       |   否       |
||||

<!-- Not Avaiable |Azure Data Lake Store*     |   Yes       |    No      | -->

\* 仅限表格 1400 模型。

**提供程序**   
连接到 Azure 数据源的内存中和 DirectQuery 模型使用用于 SQL Server 的 .NET Framework 数据提供程序。

## <a name="on-premises-data-sources"></a>本地数据源

从 Azure AS 服务器连接到本地数据源需要使用本地网关。 使用网关时，需要 64 位提供程序。

### <a name="in-memory-and-directquery"></a>内存中和 DirectQuery

|数据源 | 内存中提供程序 | DirectQuery 提供程序 |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0、用于 SQL Server 的 Microsoft OLE DB 提供程序、用于 SQL Server 的 .NET Framework 数据提供程序 | 用于 SQL Server 的 .NET Framework 数据提供程序 |
| SQL Server 数据仓库 |SQL Server Native Client 11.0、用于 SQL Server 的 Microsoft OLE DB 提供程序、用于 SQL Server 的 .NET Framework 数据提供程序 | 用于 SQL Server 的 .NET Framework 数据提供程序 |
| Oracle |用于 Oracle 的 Microsoft OLE DB 提供程序、用于 .NET 的 Oracle 数据提供程序 |用于 .NET 的 Oracle 数据提供程序 | |
| Teradata |用于 Teradata 的 OLE DB 提供程序、用于 .NET 的 Teradata 数据提供程序 |用于 .NET 的 Teradata 数据提供程序 | |
| | | |

### <a name="in-memory-only"></a>仅限内存中

|数据源  |  
|---------|---------|
|Access 数据库     |  
|Active Directory*     |  
|Analysis Services     |  
|分析平台系统     |  
|Dynamics CRM*     |  
|Excel 工作簿     |  
|Exchange*     |  
|文件夹*     |
|IBM Informix* (Beta) |
|JSON 文档*     |  
|二进制文件中的行*     | 
|MySQL 数据库     | 
|OData 源*     |  
|ODBC 查询     | 
|OLE DB     |   
|Postgre SQL 数据库*    | 
|Salesforce 对象* |  
|Salesforce 报表* |
|SAP HANA*    |  
|SAP Business Warehouse*    |  
|SharePoint*     |   
|Sybase 数据库     |  
|XML 表*    |  
|||

\* 仅限表格 1400 模型。

## <a name="specifying-a-different-provider"></a>指定不同的提供程序

连接到某些数据源时，Azure Analysis Services 中的数据模型可能需要不同的数据提供程序。 在某些情况下，使用本机提供程序（如 SQL Server Native Client (SQLNCLI11)）连接到数据源的表格模型可能返回错误。 如果使用 SQLOLEDB 之外的本机提供程序，则可能会看到错误消息：**未注册提供程序“SQLNCLI11.1”**。 或者，在某个 DirectQuery 模型连接到本地数据源时，如果使用了本机提供程序，则可能会看到错误消息：**创建 OLE DB 行集时出错。“LIMIT”附近的语法不正确**。

将本地 SQL Server Analysis Services 表格模型迁移到 Azure Analysis Services 时，可能需要更改提供程序。

**指定提供程序**

1. 在 SSDT >“表格模型浏览器” > “数据源”中，右键单击数据源连接，并单击“编辑数据源”。
2. 在“编辑连接”中，单击“高级”，打开“高级属性”窗口。
3. 在“设置高级属性” > “提供程序”中，选择适当的提供程序。

## <a name="impersonation"></a>模拟
某些情况下可能需要指定其他模拟帐户。 可在 Visual Studio (SSDT) 或 SSMS 中指定模拟帐户。

对于本地数据源：

* 如果使用 SQL 身份验证，则模拟应为服务帐户。
* 如果使用 Windows 身份验证，请设置 Windows 用户/密码。 对于 SQL Server，只有内存中数据模型才支持带有特定模拟帐户的 Windows 身份验证。

对于云数据源：

* 如果使用 SQL 身份验证，则模拟应为服务帐户。

## <a name="next-steps"></a>后续步骤
[本地网关](analysis-services-gateway.md)   
[管理服务器](analysis-services-manage.md)

<!--Update_Description: wording update, update link -->