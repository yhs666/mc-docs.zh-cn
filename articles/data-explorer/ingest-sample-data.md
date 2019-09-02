---
title: 将示例数据引入 Azure 数据资源管理器
description: 了解如何将与天气相关的示例数据引入（加载）到 Azure 数据资源管理器。
author: orspod
ms.author: v-biyu
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 05/01/2019
ms.openlocfilehash: 194ef9098d42a3ff05323a864ca10cdf4aaf6ae1
ms.sourcegitcommit: bf3df5d77e5fa66825fe22ca8937930bf45fd201
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59686571"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>将示例数据引入 Azure 数据资源管理器

本文介绍如何将示例数据引入（加载）到 Azure 数据资源管理器数据库。 有[多种方法可以引入数据](ingest-data-overview.md)；本文重点介绍适用于测试目的的基本方法。

> [!NOTE]
> 如果你完成了[快速入门：使用 Azure 数据资源管理器 Python 库引入数据](python-ingest-data.md)，则已拥有此数据。

## <a name="prerequisites"></a>先决条件

[测试群集和数据库](create-cluster-database-portal.md)

## <a name="ingest-data"></a>引入数据

StormEvents 示例数据集包含[美国国家环境信息中心](https://www.ncdc.noaa.gov/stormevents/)中与天气相关的数据。

1. 登录到 [https://dataexplorer.azure.cn](https://dataexplorer.azure.cn)。

1. 在应用程序的左上角，选择“添加群集”。

1. 在“添加群集”对话框中，以 `https://<ClusterName>.<Region>.kusto.chinacloudapi.cn/` 格式输入群集 URL，然后选择“添加”。

1. 粘贴到以下命令中，并选择“运行”。

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.chinacloudapi.cn/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. 引入完成后，粘贴到以下查询中，在窗口中，选择该查询，并选择“运行”。

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    查询从引入的示例数据返回以下结果。

    ![查询结果](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [快速入门：在 Azure 数据资源管理器中查询数据](web-query-data.md)

> [!div class="nextstepaction"]
> [编写查询](write-queries.md)

> [!div class="nextstepaction"]
> [Azure 数据资源管理器数据引入](ingest-data-overview.md)