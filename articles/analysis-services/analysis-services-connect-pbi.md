---
title: "使用 Power BI 连接到 Azure Analysis Services | Azure"
description: "了解如何使用 Power BI 连接到 Azure Analysis Services 服务器。"
services: analysis-services
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
origin.date: 06/01/2017
ms.date: 08/07/2017
ms.author: v-yeche
ms.openlocfilehash: fad62fd926069eb9f71d6acd9fed4a4449942744
ms.sourcegitcommit: 0ae1832a7d337618605b0c50cc25265b472f569c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2017
---
# <a name="connect-with-power-bi"></a>使用 Power BI 进行连接

在 Azure 中创建服务器并向其部署表格模型后，组织中的用户便可以连接并开始浏览数据。 

> [!TIP]
> 请务必使用最新版本的 [Power BI Desktop](https://powerbi.microsoft.com/desktop/)。
> 
> 

## <a name="connect-in-power-bi-desktop"></a>在 Power BI Desktop 中连接

1. 在 Power BI Desktop 中，单击“获取数据” > “Azure” > “Azure Analysis Services 数据库”。

2. 在“服务器”中，输入服务器名称。 

    请确保包含完整的 URL。 例如，asazure://chinaeast.asazure.chinacloudapi.cn/advworks。

3. 在“数据库”中，如果知道要连接到的表格模型数据库或透视的名称，请将其粘贴在此处。 如果不知道，可以将此字段留空，并在稍后选择数据库或透视。

4. 保留默认的“实时连接”选项，并按“连接”。 

5. 如果出现提示，请输入登录凭据。 

6. 在“导航器”中，展开服务器，选择要连接到的模型或透视，并单击“连接”。 单击某个模型或透视可显示该视图的所有对象。

    Power BI Desktop 中会打开模型，并且在“报表”视图中显示空白报表。 “字段”列表中会显示所有非隐藏的模型对象。 连接状态将显示在右下角。

## <a name="connect-in-power-bi-service"></a>在 Power BI（服务）中进行连接

1. 在服务器上创建一个与模型具有实时连接的 Power BI Desktop 文件。
2. 在 [Power BI](https://powerbi.microsoft.com) 中，单击“获取数据” > “文件”。 找到并选择文件。

## <a name="see-also"></a>另请参阅
[连接到 Azure Analysis Services](analysis-services-connect.md)   
[客户端库](analysis-services-data-providers.md)

<!--Update_Description: new articles on how to connect analysis serices server with Power BI-->