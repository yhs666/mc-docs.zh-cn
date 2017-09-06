---
title: "将 Windows AWS VM 移到 Azure | Azure"
description: "使用 Azure PowerShell 将 Amazon Web Services (AWS) EC2 Windows 实例移到 Azure 虚拟机。"
services: virtual-machines-windows
documentationcenter: 
author: hayley244
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 06/01/2017
ms.date: 09/04/2017
ms.author: v-haiqya
ms.openlocfilehash: 89fb32666885f6d026c319acc6ef189b69926311
ms.sourcegitcommit: da549f499f6898b74ac1aeaf95be0810cdbbb3ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-azure-using-powershell"></a>使用 PowerShell 将 Windows VM 从 Amazon Web Services (AWS) 移到 Azure

如果你正在评估是否要使用 Azure 虚拟机托管工作负荷，可以导出现有的 Amazon Web Services (AWS) EC2 Windows VM 实例，然后将虚拟硬盘 (VHD) 上传到 Azure。 上传 VHD 后，可以通过该 VHD 在 Azure 中创建新的 VM。 

本主题介绍如何将单个 VM 从 AWS 移到 Azure。 如果想要将 VM 从 AWS 批量迁移到 Azure，请参阅[使用 Azure Site Recovery 将 Amazon Web Services (AWS) 中的虚拟机迁移到 Azure](../../site-recovery/site-recovery-migrate-aws-to-azure.md)。

## <a name="prepare-the-vm"></a>准备 VM 

可以将通用和专用 VHD 上传到 Azure。 每种类型都需要在从 AWS 导出之前准备 VM。 

- **通用 VHD** - 通用 VHD 包含使用 Sysprep 删除所有个人帐户信息。 如果想要使用 VHD 作为映像来创建新的 VM，应该： 

    * [准备 Windows VM](prepare-for-upload-vhd-image.md)  
    * 使用 Sysprep 通用化虚拟机。  

- **专用 VHD** - 专用 VHD 保留原始 VM 中的用户帐户、应用程序和其他状态数据。 如果想要使用当前 VHD 创建新 VM，请确保完成以下步骤。  
    * [准备好要上传到 Azure 的 Windows VHD](prepare-for-upload-vhd-image.md)。 **不要**使用 Sysprep 通用化 VM。 
    * 删除 VM 上安装的所有来宾虚拟化工具和代理（例如 VMware 工具）。 
    * 确保 VM 配置为通过 DHCP 来提取其 IP 地址和 DNS 设置。 这确保服务器在启动时在 VNet 中获取 IP 地址。  

## <a name="export-and-download-the-vhd"></a>导出和下载 VHD 

将 EC2 实例导出到 Amazon S3 存储桶中的 VHD。 遵照 Amazon 文档主题[使用 VM 导入/导出将实例导出为 VM](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) 中所述的步骤，然后运行 [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) 命令将 EC2 实例导出到 VHD 文件。 

导出的 VHD 文件将保存在指定的 Amazon S3 存储桶中。 导出 VHD 的基本语法如下所示，只需将 <brackets> 中的占位符文本替换为自己的信息。

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

导出 VHD 后，按照[如何从 S3 存储桶下载对象？](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html)中的说明从 S3 存储桶下载 VHD 文件。 

> [!IMPORTANT]
> AWS 会收取下载 VHD 所产生的数据传输费用。 有关详细信息，请参阅 [Amazon S3 定价](https://aws.amazon.com/s3/pricing/)。

## <a name="next-steps"></a>后续步骤

现在，可将 VHD 上传到 Azure 并创建新的 VM。 

- 如果导出之前在源上运行了 Sysprep 来将它**通用化**，请参阅[上传已通用化的 VHD 并在 Azure 中使用它来创建新的 VM](upload-generalized-managed.md)
- 如果导出之前未运行 Sysprep，VHD 将被视为**已专用化**。请参阅[将已专用的 VHD 上传到 Azure 并创建新的 VM](create-vm-specialized.md)
