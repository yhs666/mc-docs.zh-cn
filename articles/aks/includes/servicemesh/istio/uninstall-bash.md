---
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 10/09/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: 0f26edd9793b667139da2b0fe18ffa5eff66bec5
ms.sourcegitcommit: 642a4ad454db5631e4d4a43555abd9773cae8891
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425885"
---
若要删除 CRD，请运行以下命令：

```bash
kubectl get crds -o name | grep 'istio.io' | xargs -n1 kubectl delete
```

若要删除机密，请运行以下命令：

```bash
kubectl get secret --all-namespaces -o json | jq '.items[].metadata | ["kubectl delete secret -n", .namespace, .name] | join(" ")' -r | fgrep "istio." | xargs -t0 bash -c
```

<!--Update_Description: new articles on istio uninstall bash -->
<!--New.date: 11/04/2019-->