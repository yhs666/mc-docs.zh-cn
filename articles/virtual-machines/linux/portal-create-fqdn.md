---
title: "在 Azure 门户中为 Linux VM 创建 FQDN | Azure"
description: "了解如何在 Azure 门户中为基于 Resource Manager 的虚拟机创建完全限定的域名（简称 FQDN）。"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 03/14/2017
ms.date: 04/27/2017
ms.author: v-dazen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dec50929e77e5e20000447aaecf1094cae7ff604
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>在 Azure 门户中为 Linux VM 创建完全限定的域名

在 [Azure 门户](https://portal.azure.cn)中使用 Resource Manager 部署模型创建虚拟机 (VM) 时，该门户会为虚拟机自动创建一个公共 IP 资源。 可以使用此 IP 地址远程访问 VM。 默认情况下，该门户不会创建[完全限定的域名](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)（简称 FQDN），但一旦创建 VM，就可以添加一个 FQDN。 本文演示了创建 DNS 名称或 FQDN 的步骤。

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

现在可以使用此 DNS 名称（例如搭配 `ssh ops@mydns.chinanorth.chinacloudapp.cn`）远程连接至 VM。

## <a name="next-steps"></a>后续步骤
你的 VM 已经有公共 IP 和 DNS 名称，现在可以部署通用应用程序框架或服务，例如 nginx、MongoDB、Docker 等等。

也可以深入了解[使用 Resource Manager](../../azure-resource-manager/resource-group-overview.md)，获取生成 Azure 部署的相关提示。
