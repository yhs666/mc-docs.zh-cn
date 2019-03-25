---
author: rockboyfor
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 03/25/2019
ms.author: v-yeche
ms.openlocfilehash: 71804e5fcc33926aa772aab237c6a3dc309b2eec
ms.sourcegitcommit: c70402dacd23ccded50ec6aea9f27f1cf0ec22ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261378"
---
[适用于 .NET 的 Azure Configuration Manager 库](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) 提供用于分析配置文件中连接字符串的类。 [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) 类分析配置设置，而不考虑客户端应用程序是在台式计算机、移动设备、Azure 虚拟机还是 Azure 云服务中运行。

若要引用 CloudConfigurationManager 包，请添加以下 `using` 指令：

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage;
```

以下示例演示如何从配置文件中检索连接字符串：

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

不一定非要使用 Azure 配置管理器。 也可以使用 API，例如 .NET Framework 的 [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) 类。

