---
title: "了解 Azure IoT 中心消息传送 | Azure"
description: "开发人员指南 - 使用 IoT 中心进行设备到云和云到设备的消息传送。 包括消息格式和支持的通信协议的相关信息。"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 05/25/2017
ms.author: v-yiso
ms.date: 07/10/2017
ms.openlocfilehash: 8e3c5769d4d3890e8084798268c4677f5f0148b0
ms.sourcegitcommit: b8a5b2c3c86b06015191c712df45827ee7961a64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# 使用 IoT 中心进行设备到云和云到设备的消息传送
<a id="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub" class="xliff"></a>

使用 IoT 中心消息传送功能与设备进行通信，具体方法如下：

* 从设备将[设备到云][lnk-d2c]消息发送到解决方案后端。
* 从解决方案后端将[云到设备][lnk-c2d]消息发送到设备。

IoT 中心消息传送功能的核心属性是消息的可靠性和持久性。 这些属性可在设备端上恢复间歇性连接，以及在云恢复事件处理的负载高峰。 IoT 中心对从设备到云和从云到设备的消息传送实施*至少一次*传送保证。

有关 IoT 中心功能的简介，请参阅文章 [Azure 和物联网][lnk-azure-iot]和 [Azure IoT 中心服务概述][lnk-iot-hub-overview]。

## 何时使用 IoT 中心消息传送功能
<a id="when-to-use-iot-hub-messaging" class="xliff"></a>

使用设备到云消息从设备应用发送时序遥测数据和警报，使用云到设备消息向设备应用发送单向通知。

* 如果在使用设备到云消息、报告属性或文件上传方面有任何疑问，请参阅[设备到云通信指南][lnk-d2c-guidance]。
* 如果在使用云到设备消息、所需属性或直接方法方面有任何疑问，请参阅[云到设备通信指南][lnk-c2d-guidance]。

## 后续步骤
<a id="next-steps" class="xliff"></a>

* 了解有关 IoT 中心[设备到云消息传送][lnk-d2c]的信息。
* 了解有关 IoT 中心[云到设备消息传送][lnk-c2d]的信息。

[lnk-azure-iot]: ./iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: ./iot-hub-what-is-iot-hub.md
[lnk-d2c]: ./iot-hub-devguide-messages-d2c.md
[lnk-c2d]: ./iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: ./iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: ./iot-hub-devguide-d2c-guidance.md