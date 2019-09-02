---
title: 适用于 Azure Functions 的 Azure IoT 中心绑定
description: 了解如何在 Azure Functions 中使用 IoT 中心绑定。
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure Functions，函数，事件处理，动态计算，无服务体系结构
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
origin.date: 03/05/2019
ms.date: 06/03/2019
ms.author: v-junlch
ms.openlocfilehash: d58ab31f04d84c59bd2f66e04e56833302c019f5
ms.sourcegitcommit: 9e839c50ac69907e54ddc7ea13ae673d294da77a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491476"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>适用于 Azure Functions 的 Azure IoT 中心绑定

本文介绍如何使用 IoT 中心的 Azure Functions 绑定。 IoT 中心支持基于 [Azure 事件中心绑定](functions-bindings-event-hubs.md)。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>包 - Functions 1.x

对于 Azure Functions 版本 1.x，[Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet 包 2.x 版中提供了 IoT 中心绑定。 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>包 - Functions 2.x

对于 Functions 2.x，请使用 [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) 包 3.x 版。 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

> [!IMPORTANT]
> 虽然下述代码示例使用事件中心 API，但给定的语法适用于 IoT 中心函数。

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解 Azure Functions 触发器和绑定](functions-triggers-bindings.md)

<!-- Update_Description: link update -->