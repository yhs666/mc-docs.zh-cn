---
title: "Service Fabric 群集 Resource Manager 中的限制 | Azure"
description: "了解如何配置 Service Fabric 群集 Resource Manager 提供的限制。"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 01/05/2017
ms.date: 02/20/2017
ms.author: v-johch
ms.openlocfilehash: 00a331aff8af92061f2aca694397848dff188dc5
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>限制 Service Fabric 群集 Resource Manager 的行为
即使已正确配置群集 Resource Manager，群集有时也会中断。 例如，可能同时发生节点或容错域故障 - 升级时发生这种情况会怎样？ 群集 Resource Manager 会尽力修复一切，但这可能导致群集中出现变动。 限制可帮助提供停止机制，让群集能够使用资源自行稳定下来，即节点恢复正常，网络分区自行修复、部署已更正的位。

为帮助应对此类情况，Service Fabric 群集 Resource Manager 提供多种限制。 这些限制十分有效。 除非已仔细计算过群集可以并行完成的工作量，否则不应更改这些设置的默认值。

限制的默认值是 Service Fabric 团队从经验中得出的合适默认值。 如果需要更改这些默认值，应根据预期的实际负载进行调优。 即使意味着群集在主线方案中会需要更长的时间才能稳定下来，用户也可能会决定设置某些限制。

## <a name="configuring-the-throttles"></a>配置限制
下面是默认包含的限制：

* GlobalMovementThrottleThreshold - 此设置控制一段时间内（已定义为 GlobalMovementThrottleCountingInterval，以秒为单位的值）群集中移动的总数
* MovementPerPartitionThrottleThreshold - 此设置控制一段时间内（MovementPerPartitionThrottleCountingInterval，以秒为单位的值）针对任何服务分区的移动总数

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

通过 ClusterConfig.json 进行独立部署或将 Template.json 用于 Azure 托管群集：

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThreshold",
          "value": "1000"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      },
      {
          "name": "MovementPerPartitionThrottleThreshold",
          "value": "50"
      },
      {
          "name": "MovementPerPartitionThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

大部分时候，客户使用这些限制是因为他们已处于资源受限环境中。 此类环境的一些示例包括，每个节点的网络带宽受限，或磁盘因吞吐量限制而无法并行生成多个副本。 这些类型的限制意味着，即使没有限制，针对故障触发的操作也无法成功，或会速度缓慢。 在这些情况下，客户知道群集达到稳定状态的时间会延长。 客户还知道，如果受到限制，他们最终会以较低的整体可靠性运行。

## <a name="next-steps"></a>后续步骤
- 若要了解群集 Resource Manager 如何管理和均衡群集中的负载，请查看有关[均衡负载](./service-fabric-cluster-resource-manager-balancing.md)的文章
- 群集 Resource Manager 提供许多用于描述群集的选项。 若要详细了解这些选项，请查看这篇[描述 Service Fabric 群集](./service-fabric-cluster-resource-manager-cluster-description.md)的文章