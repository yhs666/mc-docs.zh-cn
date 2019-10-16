---
title: 诊断和排查 Azure IoT 中心 DPS 的连接断开问题
description: 了解如何诊断和排查 Azure IoT 中心 DPS 设备连接的常见错误
author: xujing-ms
manager: nberdy
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
origin.date: 09/09/2019
ms.date: 10/08/2019
ms.author: v-yiso
ms.openlocfilehash: e5eeb2e1fd9e9f73ca1f7422fcd8eb38f8dea2a5
ms.sourcegitcommit: 332ae4986f49c2e63bd781685dd3e0d49c696456
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71341126"
---
# <a name="troubleshooting-with-azure-iot-hub-device-provisioning-service"></a>使用 Azure IoT 中心设备预配服务进行故障排除

由于存在许多可能的故障点（例如证明失败、注册失败等），IoT 设备的连接问题有时很难排查。本文介绍如何通过 [Azure Monitor](/azure-monitor/overview) 检测和排查设备连接问题。

## <a name="using-azure-monitor-to-view-metrics-and-set-up-alerts"></a>使用 Azure Monitor 查看指标并设置警报

以下过程介绍如何查看和设置基于 IoT 中心设备预配服务指标的警报。 

1. 登录到 [Azure 门户](https://portal.azure.cn)。

2. 浏览到 IoT 中心设备预配服务

3. 选择“指标”  。

4. 选择所需指标。 
   <br />目前有三种用于 DPS 的指标：

    | 指标名称 | 说明 |
    |-------|------------|
    | 证明尝试次数 | 尝试使用设备预配服务进行身份验证的设备数|
    | 注册尝试次数 | 尝试在成功进行身份验证后注册到 IoT 中心的设备数|
    | 分配的设备 | 已成功分配给 IoT 中心的设备数|

5. 选择所需的聚合方法，用于创建指标的视图。 

6. 若要设置某个指标的警报，请在指标边栏选项卡的右上角选择“新建警报规则”  。同样，也可以转到“警报”边栏选项卡，然后选择“新建警报规则”。  

7. 选择“添加条件”，  然后按照以下提示选择所需指标和阈值。

有关详细信息，请参阅 [Microsoft Azure 中的经典警报是什么？](../azure-monitor/platform/alerts-overview.md)



## <a name="common-error-codes"></a>常见错误代码
使用下表来了解和解决常见错误。

| 错误代码| 说明 | HTTP 状态代码 |
|-------|------------|------------|
| 400 | 请求正文无效；例如，它不能进行分析，或者对象无法验证。| 400 格式错误 |
| 401 | 授权令牌无法验证；例如，它已过期或无法应用到请求的 URI。 此错误代码也会在 TPM 证明流中返回给设备。 | 401 未授权|
| 404 | 设备预配服务实例或资源（例如注册）不存在。 |404 未找到 |
| 412 | 根据 RFC7232 规范，请求中的 ETag 与现有资源中的 ETag 不匹配。 | 412 先决条件失败 |
| 429 | 服务限制了操作。 对于具体的服务限制，请参阅 [IoT 中心设备预配服务限制](/azure-subscription-service-limits#iot-hub-device-provisioning-service-limits)。 | 429 请求过多 |
| 500 | 发生了内部错误。 | 500 内部服务器错误|
