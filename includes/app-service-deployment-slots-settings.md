---
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 06/18/2019
ms.date: 09/04/2019
ms.author: v-tawe
ms.openlocfilehash: 202643d51771a24a39b6a2bd962aad1bf205ac08
ms.sourcegitcommit: bc34f62e6eef906fb59734dcc780e662a4d2b0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806765"
---
从另一个部署槽克隆配置时，可以编辑克隆的配置。 某些配置元素在交换时遵循内容（不特定于槽），而其他配置元素会在交换之后保留在同一个槽（特定于槽）。 以下列表显示交换槽时会更改的设置。

**已交换的设置**：

* 常规设置 - 例如 Framework 版本、32/64 位、Web 套接字
* 应用设置（可以配置为停在槽中）
* 连接字符串（可以配置为停在槽中）
* 处理程序映射
* 监视和诊断设置
* 公用证书
* WebJobs 内容
* 混合连接 *
* 虚拟网络集成 *
* 服务终结点 *
* Azure 内容分发网络 *

标有星号 (*) 的功能已计划粘滞到槽。 

**不交换的设置**：

* 发布终结点
* 自定义域名
* 私有证书和 SSL 绑定
* 缩放设置
* Web 作业计划程序
* IP 限制
* Always On
* 协议设置（HTTPS、TLS 版本、客户端证书）
* 诊断日志设置
* 跨域资源共享 (CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->