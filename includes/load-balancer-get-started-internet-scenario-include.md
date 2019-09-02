---
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/31/2018
ms.author: v-jay
ms.openlocfilehash: 4c540fa357bdbd48a39c1f447c793379764f775e
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806722"
---
会在此方案中完成以下任务：

* 在端口 80 上创建一个接收网络流量的负载均衡器，并将负载均衡流量发送到虚拟机“web1”和“web2”
* 在负载均衡器后面创建虚拟机的远程桌面访问/SSH 的 NAT 规则
* 创建运行状况探测

![负载均衡器方案](./media/load-balancer-get-started-internet-scenario-include/scenario-classic.png)