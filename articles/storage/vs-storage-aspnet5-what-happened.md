---
title: "我的 ASP.NET 5 项目（Visual Studio 连接服务）发生了什么情况？| Azure"
description: "介绍在 Visual Studio ASP.NET 5 项目中使用 Visual Studio 连接服务连接到 Azure 存储帐户后会发生什么情况"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
origin.date: 12/02/2016
ms.date: 01/06/2017
ms.author: v-johch
ms.openlocfilehash: deedd55e37ee9caa316417808bbaec1ff2c03fc2
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>我的 ASP.NET 5 项目（Visual Studio Azure 存储连接服务）发生了什么情况？

## <a name="references-added"></a>已添加引用

Azure 存储 NuGet 包已添加到你的 Visual Studio 项目。  
此包添加了以下 .NET 引用：

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

此外，NuGet 包 **Microsoft.Framework.Configuration.Json** 已添加。

## <a name="connection-string-for-azure-storage-added"></a>已添加 Azure 存储的连接字符串
在项目的 config.json 文件中，已使用选定存储帐户的连接字符串和密钥创建了一个元素。

有关详细信息，请参阅 [ASP.NET 5](http://www.asp.net/vnext)。