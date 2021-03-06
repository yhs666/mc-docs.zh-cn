---
title: Azure 安全中心的虚拟机建议 | Azure Docs
description: 本文档介绍了 Azure 安全中心有关如何帮助保护虚拟机和计算机以及 Web 应用和应用服务环境的建议。
services: security-center
documentationcenter: na
author: lingliw
manager: digimobile
editor: ''
ms.assetid: f5ce7f62-7b12-4bc0-b418-4a2f9ec49ca1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/22/2019
ms.author: v-lingwu
ms.openlocfilehash: b658301a93215b458e264513c6e225052bfc68be
ms.sourcegitcommit: 5fc46672ae90b6598130069f10efeeb634e9a5af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67236489"
---
# <a name="understand-azure-security-center-resource-recommendations"></a>了解 Azure 安全中心资源建议


## <a name="recommendations"></a>建议
使用下表作为参考，以帮助你了解可用的计算和应用服务建议以及每个建议在应用时的作用。

### <a name="computers"></a>计算机
| 建议 | 说明 |
| --- | --- |
| 应用恰时网络访问控制| 建议应用实时 VM 访问。 实时功能处于预览状态，并在安全中心的标准层上可用。|


### 应用服务 <a name="app-services"></a>
| 建议 | 说明 |
| --- | --- |
| 应该只能通过 HTTPS 访问应用服务 | 建议你限制为仅通过 HTTPS 访问应用服务。 |
| 应为 Web 应用程序禁用 Web 套接字| 建议你仔细检查 Web 应用程序中 Web 套接字的使用。  Web 套接字协议容易受到不同类型的安全威胁的攻击。 |
| 对 Web 应用程序使用自定义域 | 建议使用自定义域保护 Web 应用程序免受常见攻击（钓鱼和其他 DNS 相关攻击）的威胁。 |
| 为 Web 应用程序配置 IP 限制 | 建议你定义允许访问应用程序的 IP 地址列表。  使用 IP 限制保护 Web 应用程序免受常见攻击的威胁。 |
| 不允许所有 ('*') 资源访问你的应用程序 | 建议不要将 WEBSITE_LOAD_CERTIFICATES 参数设置为“ *”。将参数设置为“* ”表示所有证书都将加载到 Web 应用程序个人证书存储。  这可能导致滥用最小特权原则，因为站点在运行时不太可能需要访问所有证书。 |
| CORS 不应允许所有资源都能访问你的应用程序 | 建议你仅允许必需的域与 Web 应用程序进行交互。 跨源资源共享 (CORS) 不应允许所有域都能访问你的 Web 应用程序。 |
| 对 Web 应用程序使用受支持的最新版 .NET Framework | 建议使用最新的 .NET Framework 版本以使用最新安全类。 使用较旧的类和类型可能会使应用程序易受攻击。 |
| 对 Web 应用程序使用受支持的最新版 Java | 建议使用最新的 Java 版本以使用最新安全类。 使用较旧的类和类型可能会使应用程序易受攻击。 |
| 对 Web 应用程序使用受支持的最新版 PHP | 建议使用最新的 PHP 版本以使用最新安全类。 使用较旧的类和类型可能会使应用程序易受攻击。 |
| [添加 Web 应用程序防火墙](security-center-add-web-application-firewall.md) |建议部署 web 终结点的 Web 应用程序防火墙 (WAF)。 为任何面向公众的 IP（实例级 IP 或负载均衡 IP）显示 WAF 建议，该 IP 具有与开放入站 Web 端口 (80,443) 关联的网络安全组。</br></br>安全中心建议设置 WAF，有助于防范针对虚拟机和应用服务环境上 Web 应用程序的攻击。 应用服务环境 (ASE) 是 Azure 应用服务的[高级](https://www.azure.cn/pricing/details/app-service/)服务计划选项，可提供完全隔离和专用的环境，以便安全地运行 Azure 应用服务应用。 |
| [完成应用程序保护](security-center-add-web-application-firewall.md#finalize-application-protection) |若要完成 WAF 配置，则流量必须重新路由到 WAF 设备。 遵循此建议，完成必要的安装程序更改。 |
| 对 Web 应用程序使用受支持的最新版 Node.js | 建议使用最新的 Node.js 版本以使用最新安全类。 使用较旧的类和类型可能会使应用程序易受攻击。 |
| CORS 不应允许所有资源都能访问函数应用 | 建议你仅允许必需的域与 Web 应用程序进行交互。 跨源资源共享 (CORS) 不应允许所有域都能访问你的函数应用程序。 |
| 对函数应用使用自定义域 | 建议使用自定义域保护函数应用免受常见攻击（钓鱼和其他 DNS 相关攻击）的威胁。 |
| 对函数应用配置 IP 限制 | 建议你定义允许访问应用程序的 IP 地址列表。 使用 IP 限制保护函数应用免受常见攻击的威胁。 |
| 应该只能通过 HTTPS 访问函数应用 | 建议你限制为仅通过 HTTPS 访问函数应用。 |
| 应对函数应用禁用远程调试 | 如果不再需要使用函数应用调试，建议禁用该功能。 远程调试需要在函数应用上打开入站端口。 |
| 应对函数应用禁用 Web 套接字 | 建议你仔细检查函数应用中 Web 套接字的使用。 Web 套接字协议容易受到不同类型的安全威胁的攻击。 |

若要了解有关安全中心的详细信息，请参阅以下文章：

* [在 Azure 安全中心保护计算机和应用程序](security-center-virtual-machine-protection.md)
* [在 Azure 安全中心中设置安全策略](tutorial-security-policy.md) - 了解如何配置 Azure 订阅和资源组的安全策略。
* [Azure 安全中心常见问题解答](security-center-faq.md) - 查找有关使用服务的常见问题。

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png


