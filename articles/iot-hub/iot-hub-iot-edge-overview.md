---
title: Azure IoT Edge 概述 | Azure
description: 描述 Azure IoT Edge 中的关键体系结构概念，如网关、模块和中转站。
services: iot-hub
documentationcenter: ''
author: Derek1101
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 09/04/2017
ms.author: v-yiso
ms.date: 11/20/2017
ms.openlocfilehash: d44be6bdbf5e277c4088a01e2be264f396350fee
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52653047"
---
# <a name="azure-iot-edge-architectural-concepts"></a>Azure IoT Edge 体系结构概念

在查看任何示例代码或使用 IoT Edge 创建自己的现场网关之前，应该了解支撑 IoT Edge 体系结构的重要概念。

## <a name="iot-edge-modules"></a>IoT Edge 模块

通过创建 *IoT Edge 模块*并对其进行组合，可使用 Azure IoT Edge 生成网关。 模块通过 *消息* 来相互交换数据。 一个模块收到一条消息，对其执行某些操作，选择性地将其转换为新的消息，并将其发布，供其他模块处理。 某些模块可能只生成新消息，不处理传入消息。 一系列的模块会生成一个数据处理管道，每个模块在该管道的某个点对数据进行转换。

![使用 Azure IoT Edge 构建的网关中的模块链][1]

IoT Edge 包含以下组件：

* 预先编写的模块，执行常见的网关功能。
* 供开发人员编写自定义模块的接口。
* 部署和运行一组模块所需的基础结构。

该 SDK 提供一个抽象层，利用该抽象层可以生成在各种操作系统和平台上运行的网关。

![Azure IoT Edge 抽象层][2]

## <a name="messages"></a>消息

虽然可以通过思考模块互相传递消息的方式来了解网关运行过程中的概念，但这种思考过程并不能准确地反映实际发生的情况。 IoT Edge 模块间使用中转站进行通信。 模块通过总线或发布/订阅等消息传递模式将消息发布到中转站，然后让中转站将消息路由到连接到该中转站的模块。

模块使用“Broker_Publish”函数将消息发布到中转站。 中转站通过调用回调函数将消息传递给模块。 一条消息由一组键/值属性以及作为内存块传递的内容组成。

![Azure IoT Edge 中的中转站角色][3]

## <a name="message-routing-and-filtering"></a>消息路由和筛选

可通过两种方法将消息定向到正确的 IoT Edge 模块：

* 可将一组链接传递到中转站，使中转站获知每个模块的源和接收器。
* 模块可以根据消息的属性进行筛选。

模块仅应在消息是其适用对象的情况下对消息执行操作。 链接和消息筛选实际上创建了消息管道。

## <a name="next-steps"></a>后续步骤

若要查看这些适用于可运行示例的概念，请参阅[浏览 Azure IoT Edge 体系结构][lnk-hello-world]。

<!-- Images -->
[1]: ./media/iot-hub-iot-edge-overview/modules.png
[2]: ./media/iot-hub-iot-edge-overview/modules_2.png
[3]: ./media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md