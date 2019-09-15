---
title: 有关 Azure Functions 中的网络的常见问题解答
description: 有关 Azure Functions 的网络的一些最常见问题解答和方案。
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: troubleshooting
origin.date: 04/11/2019
ms.date: 09/06/2019
ms.author: v-junlch
ms.reviewer: glenga
ms.openlocfilehash: f5f264c6fbbb16fc260aeeadd146d9f7008963be
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805799"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>有关 Azure Functions 中的网络的常见问题解答

本文列出了有关 Azure Functions 中的网络的常见问题解答。 有关更完整的概述，请参阅 [Functions 网络选项](functions-networking-options.md)。

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>如何限制对我的函数的 Internet 访问？

可以采用多种方式来限制 Internet 访问：

* [IP 限制](../app-service/app-service-ip-restrictions.md)：按 IP 范围限制到您的函数应用的入站流量。
    * 在 IP 限制下，你还能够配置[服务终结点](../virtual-network/virtual-network-service-endpoints-overview.md)，这会将您的函数限制为仅接受来自特定虚拟网络的入站流量。
* 删除所有 HTTP 触发器。 对于某些应用程序，只需要避免使用 HTTP 触发器并使用任何其他事件源来触发您的函数就足够了。

请记住，Azure 门户编辑器需要直接访问你的正在运行的函数。 通过 Azure 门户所做的任何代码更改都将要求你使用的设备浏览门户来将其 IP 列入允许列表。 但是，在实施了网络限制的情况下，你仍然可以使用“平台功能”选项卡下的任何内容。

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>如何将我的函数应用限制到某个虚拟网络？

可以使用[服务终结点](./functions-networking-options.md#private-site-access)将函数应用的**入站**流量限制倒某个虚拟网络。 此配置仍然允许函数应用对 Internet 进行出站调用。

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>如何从函数应用访问虚拟网络中的资源？

你可以使用虚拟网络集成从正在运行的函数访问虚拟网络中的资源。 有关详细信息，请参阅[虚拟网络集成](functions-networking-options.md#virtual-network-integration)。

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>如何访问由服务终结点保护的资源？

使用虚拟网络集成，可以从正在运行的函数访问由服务终结点保护的资源。 有关详细信息，请参阅[虚拟网络集成](functions-networking-options.md#virtual-network-integration)。

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>如何从虚拟网络中的资源触发函数？

可以使用[服务终结点](./functions-networking-options.md#private-site-access)允许从虚拟网络调用 HTTP 触发器。 

## <a name="next-steps"></a>后续步骤

若要详细了解网络和函数，请执行以下操作： 

* [详细了解 Azure Functions 中的网络选项](./functions-networking-options.md)
* [详细了解虚拟网络与应用服务和 Functions 的集成](../app-service/web-sites-integrate-with-vnet.md)
* [详细了解 Azure 中的虚拟网络](../virtual-network/virtual-networks-overview.md)

<!-- Update_Description: wording update -->