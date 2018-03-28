---
title: 在 Azure Stack 中发布自定义的 Marketplace 项（面向云操作员）| Microsoft Docs
description: 向 Azure Stack 操作员介绍了解如何在 Azure Stack 中发布自定义的 Marketplace 项。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/25/2017
ms.date: 03/02/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: 4b6736d844d1b52a949151683a07330a510bc2f3
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="the-azure-stack-marketplace-overview"></a>Azure Stack Marketplace 概述

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Marketplace 是一组针对 Azure Stack 自定义的服务、应用程序和资源集合，例如网络、虚拟机、存储等等。 用户可在此位置创建新资源和部署新应用程序。 可将 Marketplace 视为购物目录，用户可在此处浏览并选择所需的商品。 若要使用某个 Marketplace 项，用户必须订阅一个可让他们访问该项的产品/服务。

Azure Stack 操作员可以确定要添加（发布）到 Marketplace 的项。 可以发布数据库、应用服务等项。 然后，所有用户都可以看到这些项。 可以发布创建的自定义项。 还可以从不断扩充的 [Azure Marketplace 项列表](azure-stack-marketplace-azure-items.md)发布项。 将某个项发布到 Marketplace 后，用户在五分钟内就能看到它。

若要打开 Marketplace，请单击“新建”。

![](./media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace 项
Azure Stack Marketplace 项是用户可下载并使用的服务、应用程序或资源。 所有用户可以看到所有的 Azure Stack Marketplace 项。

每个 Marketplace 项具有：

- 用于资源预配的 Azure 资源管理器模板
- 元数据，例如字符串、图标和其他营销材料
- 有关在门户中显示该项的格式信息

发布到 Marketplace 的每个项使用名为 Azure 库包 (azpkg) 的格式。 请单独地将部署或运行时资源（例如代码、包含软件的 zip 文件或虚拟机映像）添加到 Azure Stack，而不要将其作为 Marketplace 项的一部分添加。 

## <a name="next-steps"></a>后续步骤
[创建和发布 Marketplace 项](azure-stack-create-and-publish-marketplace-item.md)


