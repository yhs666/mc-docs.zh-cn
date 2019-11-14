---
title: 了解 IoT 中心的 IP 地址 | Microsoft Docs
description: 了解如何查询 IoT 中心的 IP 地址及其属性。 IoT 中心的 IP 地址在某些情况下（例如灾难恢复或区域故障转移）可能会更改。
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 07/29/2019
ms.date: 11/11/2019
ms.openlocfilehash: 2b4ff2bf3303c83fc2061b847b76244f3ec8d9bd
ms.sourcegitcommit: 642a4ad454db5631e4d4a43555abd9773cae8891
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425962"
---
# <a name="understanding-the-ip-address-of-your-iot-hub"></a>了解 IoT 中心的 IP 地址

IoT 中心的 IP 地址是中心的负载均衡的公共终结点，不是专用 IP 地址。 此地址取决于它部署时所在的 Azure 区域的网络地址范围。 此 IP 地址可能会在没有通知的情况下更改。 数据中心网络更新、数据中心灾难恢复或 IoT 中心的区域故障转移可能会更改 IoT 中心 IP 地址。 请参阅 [IoT 中心高可用性和灾难恢复](iot-hub-ha-dr.md)，以便更详细地了解 Azure IoT 中心区域故障转移和可用性。

在故障转移到另一区域以后，IoT 中心的 IP 地址会更改。 可以按[对 IoT 中心执行手动故障转移](tutorial-manual-failover.md)教程中的说明测试此功能。

## <a name="discover-your-iot-hub-ip-address"></a>发现 IoT 中心 IP 地址

在 CNAME ([*iot-hub-name*].azure-devices.cn) 上使用反向 DNS 查找即可发现 IoT 中心的 IP 地址。 可以使用 **nslookup** 验证 IoT 中心实例的 IP 地址：

```cmd/sh
nslookup {YourIoTHubName}.azure-devices.cn
```

此 IP 地址可能会在没有通知的情况下更改。 在进行故障转移或灾难恢复的情况下，必须在次要区域轮询 IoT 中心 IP 地址。

## <a name="outbound-firewall-rules-for-iot-hub"></a>IoT 中心的出站防火墙规则

尝试创建防火墙规则，根据 IoT 中心的主机名或域进行筛选。 如果只能允许出站流量发往特定地址，则请定期轮询 IoT 中心 IP 地址并更新防火墙规则。

## <a name="support-for-ipv6"></a>对 IPv6 的支持 

目前 IoT 中心不支持 IPv6。