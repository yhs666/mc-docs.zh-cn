---
title: "在 Azure 中重新部署 Linux 虚拟机 | Azure"
description: "如何通过在 Azure 中重新部署 Linux 虚拟机来解决 SSH 连接问题。"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 12/14/2017
ms.date: 03/19/2018
ms.author: v-yeche
ms.openlocfilehash: 4bd86a8e9e30e3e4a220115fecf4f71bc30b3c55
ms.sourcegitcommit: 5bf041000d046683f66442e21dc6b93cb9d2f772
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2018
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>将 Linux 虚拟机重新部署到新的 Azure 节点
如果在对 SSH 或应用程序访问 Azure 中 Linux 虚拟机 (VM) 进行故障排除时遇到困难，重新部署 VM 可能会有帮助。 重新部署 VM 时，将 VM 移到 Azure 基础结构中的新节点，并重新提供支持。 所有配置选项和关联资源均保留。 本文介绍如何使用 Azure CLI 或 Azure 门户重新部署 VM。

> [!NOTE]
> 重新部署 VM 后，临时磁盘将丢失，与虚拟网络接口关联的动态 IP 地址将更新。 

可以使用以下选项之一重新部署 VM。 只需选择一个选项来重新部署 VM：

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure 门户](#using-azure-portal)
<a name="azure-cli-20"></a>
## <a name="use-the-azure-cli-20"></a>使用 Azure CLI 2.0
安装最新的 [Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-az-cli2?view=azure-cli-latest) 并使用 [az login](https://docs.azure.cn/zh-cn/cli/?view=azure-cli-latest#az_login) 登录到 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

使用 [az vm redeploy](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az_vm_redeploy) 重新部署 VM。 以下示例在名为 *myResourceGroup* 的资源组中重新部署名为 *myVM* 的 VM：

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

<a name="azure-cli-10"></a>
## <a name="use-the-azure-cli-10"></a>使用 Azure CLI 1.0
安装[最新的 Azure CLI 1.0](../../cli-install-nodejs.md) 并登录到 Azure 帐户。 请确保处于资源管理器模式 (`azure config mode arm`)。

以下示例在名为 *myResourceGroup* 的资源组中重新部署名为 *myVM* 的 VM：

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

<a name="using-azure-portal"></a>
[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>后续步骤
如果在连接 VM 时遇到问题，可以在 [SSH 连接故障排除](troubleshoot-ssh-connection.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)或[详细的 SSH 故障排除步骤](detailed-troubleshoot-ssh-connection.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到特定的帮助。 如果无法访问在 VM 上运行的应用程序，还可以阅读 [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)（应用程序故障排除问题）。

<!--Update_Description: update link, wording update -->