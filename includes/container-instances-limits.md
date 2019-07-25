---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 02/13/2019
ms.author: danlep
ms.openlocfilehash: dbbed7b4a04d81fccf5e6473e2625fa21e8c603f
ms.sourcegitcommit: 1414c787aa13b802e43fc7317af96a9e14889e20
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68332765"
---
| Resource | 默认限制 |
| --- | :--- |
| 每个订阅的容器组数 | 100<sup>1</sup> |
| 每个容器组的容器数 | 60 |
| 每个容器组的卷数 | 20 个 |
| 每个 IP 的端口数 | 5 |
| 容器实例日志大小 - 正在运行的实例 | 4 MB |
| 容器实例日志大小 - 已停止的实例 | 16 KB 或 1,000 行 |
| 每小时创建容器次数 |300<sup>1</sup> |
| 每 5 分钟创建容器次数 | 100<sup>1</sup> |
| 每小时删除容器次数 | 300<sup>1</sup> |
| 每 5 分钟删除容器次数 | 100<sup>1</sup> |


<sup>1</sup>要请求提高上限，请创建一个 [Azure 支持请求][azure-support]。<br />

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
