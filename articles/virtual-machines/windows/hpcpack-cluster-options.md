---
title: "Azure 中的 Windows HPC Pack 群集选项 | Azure"
description: "了解在 Azure 云中使用 Microsoft HPC Pack 创建和管理 Windows 高性能计算 (HPC) 群集时可用的选项"
services: virtual-machines-windows,cloud-services,batch
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
origin.date: 10/26/2017
ms.date: 12/18/2017
ms.author: v-yeche
ms.openlocfilehash: 5465e981cb4705bcba38b98743de5febafcbc8cd
ms.sourcegitcommit: 408c328a2e933120eafb2b31dea8ad1b15dbcaac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-windows-hpc-workloads-in-azure"></a>使用 HPC Pack 为 Azure 中的 Windows HPC 工作负荷创建和管理群集的选项
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

本文重点介绍用于创建 HPC Pack 群集以便运行 Windows 工作负荷的选项。 
<!-- Not Available on [Linux HPC workloads](../linux/hpcpack-cluster-options.md)-->

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>Azure VM 和 VM 规模集中的 HPC Pack 群集
### <a name="azure-templates"></a>Azure 模板

>[!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.net”替换为“chinacloudapp.cn”）；更改某些不受支持的 VM 映像；更改某些不受支持的 VM 大小。

* （快速入门）[创建 HPC 群集](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* （快速入门）[使用自定义计算节点映像创建 HPC 群集](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

<!-- Not Available on ### Azure VM images -->
### <a name="powershell-deployment-script-for-hpc-pack-2012-r2"></a>适用于 HPC Pack 2012 R2 的 PowerShell 部署脚本
* [使用 HPC Pack IaaS 部署脚本创建 HPC 群集](classic/hpcpack-cluster-powershell-script.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="manual-deployment-with-the-azure-portal"></a>使用 Azure 门户手动部署
* [在 Azure VM 中设置 HPC Pack 群集的头节点](hpcpack-cluster-headnode.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a>群集管理
* [在 Azure 中管理 HPC Pack 群集的计算节点](classic/hpcpack-cluster-node-manage.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [增加和减少 HPC Pack 群集中的 Azure 计算资源](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [将作业提交到 Azure 中的 HPC Pack 群集](hpcpack-cluster-submit-jobs.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)
* [HPC Pack 中的作业管理](https://technet.microsoft.com/library/jj899585.aspx)
* [使用 Azure Active Directory 在 Azure 中管理 HPC Pack 2016 群集](hpcpack-cluster-active-directory.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="burst-with-worker-role-nodes"></a>使用辅助角色节点迸发 
* [使用 HPC Pack 迸发到 Azure 辅助角色实例](https://technet.microsoft.com/library/gg481749.aspx)
* [教程：使用 Azure 中的 HPC Pack 设置混合群集](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [将 Azure“突发”节点添加到 Azure 中的 HPC Pack 头节点](classic/hpcpack-cluster-node-burst.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="burst-with-azure-batch"></a>使用 Azure Batch 迸发
* [使用 HPC Pack 迸发到 Azure Batch](https://technet.microsoft.com/library/mt612877.aspx)
<!--Update_Description: update meta properties, wording update-->