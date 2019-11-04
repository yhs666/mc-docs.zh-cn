---
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 10/09/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: 20de0d413962f9fd46b8a14a3ffb770e3fdfda19
ms.sourcegitcommit: 642a4ad454db5631e4d4a43555abd9773cae8891
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426399"
---
```bash
kubectl describe pod -l "app=voting-analytics, version=1.0" -n voting | egrep "istio-proxy:|voting-analytics:" -A2
```

Istio 已自动注入 `istio-proxy` 容器，来管理组件的往返网络流量，如以下示例输出所示：

```console
  voting-analytics:
    Container ID:   docker://35efa1f31d95ca737ff2e2229ab8fe7d9f2f8a39ac11366008f31287be4cea4d
    Image:          mcr.microsoft.com/aks/samples/voting/analytics:1.0
--
  istio-proxy:
    Container ID:  docker://1fa4eb43e8d4f375058c23cc062084f91c0863015e58eb377276b20c809d43c6
    Image:         docker.io/istio/proxyv2:1.3.2
```

<!--Update_Description: new articles on istio scenario routing show proxy bash -->
<!--New.date: 11/04/2019-->