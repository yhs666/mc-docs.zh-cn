---
title: 为虚拟机规模集启用 Azure 磁盘加密
description: 本文提供了有关为虚拟机规模集启用 Azure 磁盘加密的说明
author: msmbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: article
ms.author: v-junlch
origin.date: 10/10/2019
ms.date: 10/29/2019
ms.custom: seodec18
ms.openlocfilehash: 670a799da72d06e091473dcda4965faccc7c5118
ms.sourcegitcommit: cb2caa72ec0e0922a57f2fa1056c25e32c61b570
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142198"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>适用于虚拟机规模集的 Azure 磁盘加密

Azure 磁盘加密为虚拟机的操作系统和数据磁盘提供卷加密，帮助保护数据，以便符合在组织安全性与符合性方面所做的承诺。 

在以下情况下，Azure 磁盘加密也可以应用于 Windows 和 Linux 虚拟机规模集：
- 使用托管磁盘创建的规模集。 本机（或非托管）磁盘规模集不支持 Azure 磁盘加密。
- Windows 规模集中的操作系统和数据卷。
- Linux 规模集中的数据卷。 目前，Linux 规模集不支持 OS 磁盘加密。

通过[使用 Azure CLI 加密虚拟机规模集](disk-encryption-cli.md)或[使用 Azure CLI 加密虚拟机规模集](disk-encryption-powershell.md)教程，仅在几分钟内即可了解虚拟机规模集的 Azure 磁盘加密基础知识。

## <a name="next-steps"></a>后续步骤

- [使用 Azure 资源管理器加密虚拟机规模集](disk-encryption-azure-resource-manager.md)
- [将 Azure 磁盘加密与虚拟机规模集扩展排序配合使用](disk-encryption-extension-sequencing.md)

