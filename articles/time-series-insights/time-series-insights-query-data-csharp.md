---
title: 使用 C# 代码查询 Azure 时序见解正式版环境中的数据 | Microsoft Docs
description: 本文介绍如何通过编码以 C# (C-sharp) .NET 语言编写的自定义应用来查询 Azure 时序见解环境中的数据。
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: v-yiso
manager: cshankar
reviewer: jasonwhowell, kfile, tsidocs
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
origin.date: 06/05/2019
ms.date: 07/08/2019
ms.custom: seodec18
ms.openlocfilehash: b35b7b667955af86fa894655cfbbcb48299c0fa3
ms.sourcegitcommit: c0f7c439184efa26597e97e5431500a2a43c81a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2019
ms.locfileid: "67456381"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>使用 C# 查询 Azure 时序见解正式版环境中的数据

本 C# 示例演示如何查询 Azure 时序见解正式版环境中的数据。

该示例演示了多个基本的查询 API 使用示例：
1. 准备时，通过 Azure Active Directory API 获取访问令牌。 在每个查询 API 请求的 `Authorization` 标头中传递此令牌。 有关如何设置非交互式应用程序，请参阅[身份验证和授权](time-series-insights-authentication-and-authorization.md)。 此外，请确保正确设置此示例开头定义的所有常量。
2. 已获得该用户有权访问的环境的列表。 将选取一个环境作为感兴趣的环境，并会查询该环境的更多数据。
3. 以 HTTPS 请求为例，可以为感兴趣的环境请求可用性数据。
4. 以 Web 套接字请求为例，可以为感兴趣的环境请求事件聚合数据。 会请求整个可用性时间范围的数据。

> [!NOTE]
> [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample) 中提供了示例代码。

## <a name="project-dependencies"></a>项目依赖项

添加 NuGet 包 `Microsoft.IdentityModel.Clients.ActiveDirectory` 和 `Newtonsoft.Json`。

## <a name="c-example"></a>C# 示例

<!--[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]-->

## <a name="next-steps"></a>后续步骤

- 若要详细了解查询，请阅读[查询 API 参考](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)。

- 了解如何[将 JavaScript 单页应用程序连接](tutorial-create-tsi-sample-spa.md)到时序见解。