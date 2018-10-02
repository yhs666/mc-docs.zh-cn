---
title: 使用 Azure 门户创建虚拟机规模集 | Microsoft Docs
description: 使用 Azure 门户部署规模集。
keywords: 虚拟机规模集
services: virtual-machine-scale-sets
documentationcenter: ''
author: alexchen2016
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
origin.date: 09/15/2017
ms.date: 12/06/2017
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90258d95bfb152caebfeff93e7892a906392d569
ms.sourcegitcommit: 399060a8d46534abd370693f6282e7343b371634
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2018
ms.locfileid: "47455585"
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>如何使用 Azure 门户创建虚拟机规模集
本教程介绍如何通过 Azure 门户在数分钟内轻松创建虚拟机规模集。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="choose-the-vm-image-from-the-marketplace"></a>从市场中选择 VM 映像
在门户中，你可以使用 CentOS、CoreOS、Debian、Ubuntu 服务器、其他 Linux 映像以及 Windows Server 映像轻松部署规模集。

首先，在 Web 浏览器中导航到 [Azure 门户](https://portal.azure.cn)。 单击“新建”，搜索“规模集”，然后选择“虚拟机规模集”条目：

![azure 虚拟机规模集门户搜索](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>创建规模集
现在可使用默认设置并快速创建规模集。

- 输入规模集的名称。  
此名称会成为规模集前端负载均衡器的 FQDN 的基础，因此请确保在整个 Azure 中，此名称是唯一的。

- 选择所需的操作系统磁盘映像

- 选择自己的订阅

- 输入所需的资源组名称和位置。 

- 输入所需的用户名和密码。 密码长度必须至少为 12 个字符，并且必须满足以下 4 个复杂性要求中的 3 个：1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符。 

- 选择所需的操作实例计数和计算机大小。
请确保选择适当的实例大小。  
有关虚拟机大小的详细信息，请参阅 [Windows VM 大小](..\virtual-machines\windows\sizes.md)或 [Linux VM 大小](..\virtual-machines\linux\sizes.md)。

- 选择所需磁盘类型：托管或非托管。  
有关详细信息，请参阅[此文档](./virtual-machine-scale-sets-managed-disks.md)。 如果选择让规模集跨越多个放置组，则此选项不可用，因为需要托管磁盘才能使规模集跨越放置组。

- 选择“是”或“否”来“启用超出 100 个实例的缩放”。  

- 启用或禁用自动缩放，如果启用，请进行配置。

![azure 虚拟机规模集门户创建提示](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>连接到规模集中的 VM
如果选择将规模集限制在单个放置组，则使用 NAT 规则部署规模集，这些规则已配置为允许轻松连接到规模集（如果未配置，要连接到规模集中的虚拟机，则可能需要在与规模集相同的虚拟网络中创建 jumpbox）。 若要查看它们，请导航到规模集的负载均衡器的 `Inbound NAT Rules` 选项卡：

![azure 虚拟机规模集门户 nat 规则](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

可以使用这些 NAT 规则连接到规模集中的每个 VM。 例如，对于 Windows 规模集，如果传入端口 50000 使用的是 NAT 规则，则可以通过 `<load-balancer-ip-address>:50000`上的 RDP 连接到该计算机。 对于 Linux 规模集，则使用命令 `ssh -p 50000 <username>@<load-balancer-ip-address>`进行连接。

## <a name="next-steps"></a>后续步骤
有关如何从 Visual Studio 部署规模集的文档，请参阅[本文档](virtual-machine-scale-sets-vs-create.md)。

有关常规文档，请参阅[规模集的文档概述页](virtual-machine-scale-sets-overview.md)。

有关一般信息，请参阅[规模集的主要登陆页](/virtual-machine-scale-sets/)。

<!--Update_Description: wording update-->
