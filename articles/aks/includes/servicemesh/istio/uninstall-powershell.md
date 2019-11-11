---
author: rockboyfor
ms.service: container-service
ms.topic: include
origin.date: 10/09/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: c983551c10258ba928951a1a16041f4407e1ddad
ms.sourcegitcommit: 642a4ad454db5631e4d4a43555abd9773cae8891
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425879"
---
若要删除 CRD，请运行以下命令：

```powershell
kubectl get crds -o name | Select-String -Pattern 'istio.io' |% { kubectl delete $_ }
```

若要删除机密，请运行以下命令：

```powershell
(kubectl get secret --all-namespaces -o json | ConvertFrom-Json).items.metadata |% { if ($_.name -match "istio.") { "Deleting {0}.{1}" -f $_.namespace, $_.name; kubectl delete secret -n $_.namespace $_.name } }
```

<!--Update_Description: new articles on istio uninstall powershell -->
<!--New.date: 11/04/2019-->