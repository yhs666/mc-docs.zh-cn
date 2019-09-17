---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 07/18/2019
ms.date: 09/02/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 861f98e5c14f47532c6833002cad24e8575b548e
ms.sourcegitcommit: ba87706b611c3fa338bf531ae56b5e68f1dd0cde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2019
ms.locfileid: "70209431"
---
用于.NET 的 [Microsoft Azure 应用身份验证](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)客户端库（预览版）简化了从代码获取和续订令牌的过程。 应用身份验证客户端库自动管理身份验证。 该库使用开发人员的凭据在本地开发期间进行身份验证。 在本地开发期间使用开发人员凭据更安全，因为不需创建 Azure AD 凭据，或者不需在开发人员之间共享凭据。 随后将解决方案部署到 Azure 时，该库会自动切换到使用应用程序凭据。

若要在 Azure 存储应用程序中使用应用身份验证库，请安装 [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) 的最新预览版包、[用于 .NET 的 Azure 存储常用客户端库](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/)的最新版本，以及[用于 .NET 的 Azure Blob 存储客户端库](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/)。 向代码添加以下 **using** 语句：

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.Storage.Auth;
using Microsoft.Azure.Storage.Blob;
```
