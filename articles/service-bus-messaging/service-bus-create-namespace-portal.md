---
title: "在 Azure 门户中创建服务总线命名空间 | Azure"
description: "如何使用 Azure 门户创建服务总线命名空间。"
services: service-bus
documentationCenter: .net
authors: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/23/2017
ms.author: v-yiso
ms.openlocfilehash: 82d34cdf570339099ad1788f262f272856f72540
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# 使用 Azure 门户创建服务总线命名空间。
<a id="create-a-service-bus-namespace-using-the-azure-portal" class="xliff"></a>
命名空间是一个适用于所有消息传送组件的公用容器。 多个队列和主题可以位于一个命名空间中，命名空间通常用作应用程序容器。 可以使用两种不同的方法来创建服务总线命名空间：

1. Azure 门户（这篇文章）

2. [Resource Manager 模板][create-namespace-using-arm]

## 在 Azure 门户中创建命名空间
<a id="create-a-namespace-in-the-azure-portal" class="xliff"></a>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

祝贺你！ 现在已创建一个服务总线消息传送命名空间。

## 后续步骤
<a id="next-steps" class="xliff"></a>

签出 [GitHub 示例][github-samples]，它们演示了 Azure 服务总线消息传送中的某些更高级的功能。

[create-namespace-using-arm]: ./service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples