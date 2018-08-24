---
title: 引发的常见 FabricClient 异常 | Azure
description: 描述在执行应用程序和群集管理操作时，FabricClient API 可能会引发的常见异常和错误。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 06/20/2018
ms.date: 08/20/2018
ms.author: v-yeche
ms.openlocfilehash: 623c758920f61269cad79050145746ef7d238828
ms.sourcegitcommit: 6174eee82d2df8373633a0790224c41e845db33c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2018
ms.locfileid: "41704468"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>使用 FabricClient API 时常见的异常和错误
[FabricClient](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricclient?view=azure-dotnet#System_Fabric_FabricClient) API 可让群集和应用程序管理员对 Service Fabric 造应用程序、服务或群集执行管理任务。 例如，部署、升级和删除应用程序、检查群集的运行状况或测试服务。 应用程序开发人员和群集管理员可以使用 FabricClient API 来开发用于管理 Service Fabric 群集和应用程序的工具。

使用 FabricClient 可以执行许多不同类型的操作。  由于输入错误、运行时错误或暂时性基础结构问题，每种方法都可能会引发异常或错误。  请参阅 API 参考文档，了解特定的方法会引发哪些异常。 但是，某些异常可能会由许多不同的 [FabricClient](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricclient?view=azure-dotnet#System_Fabric_FabricClient) API 引发。 下表列出了 FabricClient API 中常见的异常。

| 异常 | 引发时机 |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricobjectclosedexception?view=azure-dotnet#System_Fabric_FabricObjectClosedException) |[FabricClient](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricclient?view=azure-dotnet#System_Fabric_FabricClient) 对象处于关闭状态。 释放正在使用的 [FabricClient](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricclient?view=azure-dotnet#System_Fabric_FabricClient) 对象，然后实例化新的 [FabricClient](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricclient?view=azure-dotnet#System_Fabric_FabricClient) 对象。 |
| [System.TimeoutException](https://docs.microsoft.com/zh-cn/dotnet/api/system.timeoutexception?view=netframework-4.7.2#System_TimeoutException) |操作超时。[OperationTimedOut](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricerrorcode?view=azure-dotnet#System_Fabric_FabricErrorCode) 。 |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |对操作的访问权限检查失败。 返回了 E_ACCESSDENIED。 |
| [System.Fabric.FabricException](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricexception?view=azure-dotnet#System_Fabric_FabricException) |执行操作时发生运行时错误。 任何 FabricClient 方法都可能引发 [FabricException](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricexception?view=azure-dotnet#System_Fabric_FabricException)，[ErrorCode](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricexception?view=azure-dotnet#System_Fabric_FabricException_ErrorCode) 属性指明了引发异常的确切原因。 [FabricErrorCode](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricerrorcode?view=azure-dotnet#System_Fabric_FabricErrorCode) 枚举中定义了错误代码。 |
| [System.Fabric.FabricTransientException](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet#System_Fabric_FabricTransientException) |由于某种暂时性错误状态，操作失败。 例如，由于副本的仲裁暂时不可访问，某项操作可能会失败。 暂时性异常对应于可重试的失败操作。 |
<!-- Line 29 URL is correct of https://docs.microsoft.com/zh-cn/dotnet/api/system.timeoutexception?view=netframework-4.7.2#System_TimeoutException -->
<!-- Line 30 URL is correct of https://docs.microsoft.com/zh-cn/dotnet/api/system.unauthorizedaccessexception?view=netframework-4.7.2#System_UnauthorizedAccessException -->

可能在 [FabricException](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricexception?view=azure-dotnet#System_Fabric_FabricException) 中返回的某些常见 [FabricErrorCode](https://docs.azure.cn/zh-cn/dotnet/api/system.fabric.fabricerrorcode?view=azure-dotnet#System_Fabric_FabricErrorCode) 错误：

| 错误 | 条件 |
| --- |:--- |
| CommunicationError |通信错误导致操作失败，请重试操作。 |
| InvalidCredentialType |凭据类型无效。 |
| InvalidX509FindType |X509FindType 无效。 |
| InvalidX509StoreLocation |X509 存储位置无效。 |
| InvalidX509StoreName |X509 存储名称无效。 |
| InvalidX509Thumbprint |X509 证书指纹字符串无效。 |
| InvalidProtectionLevel |保护级别无效。 |
| InvalidX509Store |无法打开 X509 证书存储。 |
| InvalidSubjectName |使用者名称无效。 |
| InvalidAllowedCommonNameList |公用名字符串列表的格式无效。 应为逗号分隔的列表。 |

<!--Update_Description: update meta properties, update link -->