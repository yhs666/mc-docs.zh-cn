---
title: "通过门户启用 Azure 事件中心捕获 | Azure"
description: "通过 Azure 门户启用事件中心捕获功能。"
services: event-hubs
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 06/28/2017
ms.date: 07/24/2017
ms.author: v-yeche
ms.openlocfilehash: dadf5ec62fcdccf7e60cdccbbca7022913192d73
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>通过 Azure 门户启用事件中心捕获

可以使用 [Azure 门户](https://portal.azure.cn)在创建事件中心时配置捕获。 在“创建事件中心”门户边栏选项卡中单击“启用”按钮即可启用捕获。 然后，单击边栏选项卡的“容器”部分即可配置存储帐户和容器。 由于事件中心捕获对存储使用服务到服务身份验证，因此无需指定存储连接字符串。 资源选取器自动为存储帐户选择资源 URI。 如果使用 Azure Resource Manager，必须以字符串形式显式提供此 URI。

默认时间窗口为 5 分钟。 最小值为 1，最大值为 15。 **大小** 窗口的范围为 10-500 MB。

![][1]

## <a name="adding-capture-to-an-existing-event-hub"></a>将捕获添加到现有的事件中心

可以在事件中心命名空间中的现有事件中心上配置捕获。 较早的“消息”类型或“混合”类型命名空间中未提供此功能。 若要对现有的事件中心启用“捕获”功能，或者要更改“捕获”设置，请单击命名空间以加载“概要”边栏选项卡，然后单击要启用或更改“捕获”设置的事件中心。 最后，单击打开的边栏选项卡的“属性”部分，如下图所示：

![][2]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png

## <a name="next-steps"></a>后续步骤

还可以通过 Azure Resource Manager 模板配置事件中心捕获。
<!-- Not Avialble [Enable Capture using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub-enable-capture.md). -->