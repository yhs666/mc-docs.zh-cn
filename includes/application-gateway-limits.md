---
author: vhorne
ms.service: application-gateway
ms.topic: include
origin.date: 06/05/2019
ms.date: 08/06/2019
ms.author: v-junlch
ms.openlocfilehash: 01150f752070c2bb5bdfd2cd0892f8e640ee5477
ms.sourcegitcommit: 17cd5461e7d99f40b9b1fc5f1d579f82b2e27be9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819694"
---
| Resource | 默认/最大限制 | 注意 |
| --- | --- | --- |
| Azure 应用程序网关 |每个订阅 1,000 个 | |
| 前端 IP 配置 |2 |公共和专用各 1 个 |
| 前端端口 |100<sup>1</sup> | |
| 后端地址池 |100<sup>1</sup> | |
| 每个池的后端服务器 |1,200 | |
| HTTP 侦听器 |100<sup>1</sup> | |
| HTTP 负载均衡规则 |100<sup>1</sup> | |
| 后端 HTTP 设置 |100<sup>1</sup> | |
| 每个网关的实例 |32 | |
| SSL 证书 |100<sup>1</sup> |每个 HTTP 侦听器 1 个 |
| 最大 SSL 证书大小 |[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)| |
| 身份验证证书 |100 | |
| 受信任的根证书 |100 | |
| 请求超时最小值 |1 秒 | |
| 请求超时最大值 |24 小时 | |
| 站点数 |100<sup>1</sup> |每个 HTTP 侦听器 1 个 |
| 每个侦听器的 URL 映射 |1 | |
| 每个 URL 映射基于路径的最大规则数|[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)||
| 重定向配置数 |100<sup>1</sup>| |
| 并发 WebSocket 连接 |中型网关 20k<br> 大型网关 50k| |
| 最大 URL 长度|[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)||
| 最大文件上传大小：标准 |[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/) | |
| 最大文件上传大小 WAF |中型 WAF 网关 - [请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)| |
| WAF 正文大小限制，不带文件|128 KB||
| 最大 WAF 自定义规则|[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)||

<sup>1</sup> 对于启用了 WAF 的 SKU，建议将资源数限制为 40 以实现最佳性能。

