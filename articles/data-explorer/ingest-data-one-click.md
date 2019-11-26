---
title: 使用一键式引入将数据引入 Azure 数据资源管理器
description: 了解如何使用一键式引入将数据引入（载入）Azure 数据资源管理器。
author: orspod
ms.author: v-tawe
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
origin.date: 10/31/2019
ms.date: 11/18/2019
ms.openlocfilehash: 8c939ea7fa08740cee293b695236126fd099e942
ms.sourcegitcommit: c863b31d8ead7e5023671cf9b58415542d9fec9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74021013"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>使用一键式引入将数据引入 Azure 数据资源管理器

本文介绍如何使用一键式引入，将存储中采用 JSON 或 CSV 格式的新表快速引入 Azure 数据资源管理器。 引入数据后，可以使用 Web UI 编辑该表并运行查询。

## <a name="prerequisites"></a>先决条件

* 如果没有 Azure 订阅，请在开始前创建一个[试用 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。
* 登录到[应用程序](https://dataexplorer.azure.cn/)。
* 创建 [Azure 数据资源管理器群集和数据库](create-cluster-database-portal.md)
* Azure 存储中的数据源。

## <a name="ingest-new-data"></a>引入新数据

1. 在左侧菜单中右键单击数据库或表行，然后选择“引入新数据(预览版)”   

    ![在 Web UI 中选择一键式引入](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. 在“引入新数据(预览版)”窗口中的“源”选项卡上，填写“项目详细信息”：   

    * **Table**：从下拉列表中选择现有的表名称，或选择“新建”以创建新表。 
    * 选择“引入类型” > “从存储”或“从文件”。   
        * 如果选择了“从存储”，请输入“存储的链接”，以添加存储的 URL。   对专用存储帐户使用 [Blob SAS URL](/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container)。 
        * 如果选择了“从文件”，请选择“浏览”并将相应的文件拖到框中。  
    * 选择“编辑架构”以查看并编辑表列配置。 
 
    ![一键式引入源详细信息](media/ingest-data-one-click/one-click-ingestion-source.png) 

    > [!TIP]
    > 如果针对某个表行中选择了“引入新数据(预览版)”，选定的表名称将显示在“项目详细信息”中。   

1. 如果选择了现有的表，将打开“映射列”窗口，可在其中将源数据列映射到目标表列。  
    * 使用“省略列”可从表中删除目标列。  
    * 使用“新建列”可将新列添加到表中。  

    ![“映射列”窗口](media/ingest-data-one-click/one-click-map-columns-window.png)

1. 在“架构”选项卡中： 

    * 从下拉列表中选择“压缩类型”，然后选择“未压缩”或“GZip”。   
    * 从下拉列表中选择“数据格式”，然后选择“JSON”、“CSV”、“TSV”、“SCSV”、“SOHSV”、“TSVE”或“PSV”。         
        * 选择“JSON”格式时，请选择“JSON 级别”：   1-10。 级别会影响表列数据的描述。 
        * 如果选择的格式不是 JSON：请选中“包括列名”复选框以忽略文件的标题行。     
    * “映射名称”会自动设置，但可对其进行编辑。 
    * 如果选择了现有的表，则可以选择“映射列”按钮打开“映射列”窗口。  

    ![一键式引入 CSV 格式架构.png](media/ingest-data-one-click/one-click-csv-format.png)

1. 在“编辑器”中，选择右侧的“V”打开编辑器。   在编辑器中，可以查看和复制基于输入生成的自动查询。 

1.  在表中： 
    * 右键单击新的列标题，并选择“更改数据类型”、“重命名列”、“删除列”、“升序排序”或“降序排序”。      对于现有的列，只能执行数据排序。 
    * 双击新列名称进行编辑。

1. 选择“开始引入”以创建表、创建映射和数据引入。 

    ![一键式引入 JSON 格式架构](media/ingest-data-one-click/one-click-json-format.png) 
 
## <a name="query-data"></a>查询数据

1. 在“数据引入已完成”窗口中，如果数据引入已成功完成，则所有三个步骤都会标有绿色的勾选标记。  
 
    ![一键式数据引入已完成](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. 选择“V”打开查询。  复制到 Web UI 以编辑查询。

1. 右侧菜单包含“快速查询”和“工具”。   

    * “快速查询”包含 Web UI 的链接以及示例查询。 
    * “工具”包含 Web UI 的链接以及“删除命令”，使用这些命令可以通过运行相关的 `.drop` 命令来排查问题。  

    > [!TIP]
    > 使用 `.drop` 命令可能会丢失数据， 因此请慎用。

## <a name="next-steps"></a>后续步骤

* [在 Azure 数据资源管理器 Web UI 中查询数据](web-query-data.md)
* [使用 Kusto 查询语言编写针对 Azure 数据资源管理器的查询](write-queries.md)
