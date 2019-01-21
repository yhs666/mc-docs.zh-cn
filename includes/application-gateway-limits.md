---
author: vhorne
ms.service: application-gateway
ms.topic: include
origin.date: 11/09/2018
ms.date: 01/15/2019
ms.author: v-junlch
ms.openlocfilehash: ef041b048854b0339f32c4f26ce2108cae25328d
ms.sourcegitcommit: 04392fdd74bcbc4f784bd9ad1e328e925ceb0e0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2019
ms.locfileid: "54334340"
---
| 资源 | 默认限制 | 注意 |
| --- | --- | --- |
| 应用程序网关 |每个订阅 1000 个 | |
| 前端 IP 配置 |2 |公共和专用各 1 个 |
| 前端端口 |100<sup>1</sup> | |
| 后端地址池 |100<sup>1</sup> | |
| 每个池的后端服务器 |1200 | |
| HTTP 侦听器 |100<sup>1</sup> | |
| HTTP 负载均衡规则 |100<sup>1</sup> | |
| 后端 HTTP 设置 |100<sup>1</sup> | |
| 每个网关的实例 |32 | |
| SSL 证书 |100<sup>1</sup> |每个 HTTP 侦听器 1 个 |
| 身份验证证书 |100 | |
| 受信任的根证书 |100 | |
| 最短请求超时 |1 秒 | |
| 最长请求超时 |24 小时 | |
| 站点数 |100<sup>1</sup> |每个 HTTP 侦听器 1 个 |
| 每个侦听器的 URL 映射 |1 | |
| 每个 URL 映射基于路径的最大规则数|[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)|
| 重定向配置数 |100<sup>1</sup>| |
| 并发 WebSocket 连接 |5000| |
| 最大 URL 长度|[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)|
| 最大文件上传大小标准 |[请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/) | |
| 最大文件上传大小 WAF |中型 WAF 网关 - [请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)<br>大型 WAF 网关 - [请联系支持部门以了解详细信息](https://www.azure.cn/zh-cn/support/contact/)| |
| WAF 正文大小限制（不带文件）|128 KB||

<sup>1</sup> 对于启用了 WAF 的 SKU，建议将资源数限制为 40 以实现最佳性能。

<!-- ms.date: 01/15/2019 -->