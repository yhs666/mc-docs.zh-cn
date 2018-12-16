---
title: 适用于 Linux 的 Azure 磁盘加密 | Azure
description: 使用虚拟机扩展将适用于 Linux 的 Azure 磁盘加密部署到虚拟机。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 06/12/2018
ms.date: 08/27/2018
ms.author: v-yeche
ms.openlocfilehash: d4ebba9ef76943afb5049307ac4f66199010a221
ms.sourcegitcommit: 6cd0a8d22061aba7390579a80e19cb9d2f7faf12
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "53233771"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>适用于 Linux 的 Azure 磁盘加密 (Microsoft.Azure.Security.AzureDiskEncryptionForLinux)

## <a name="overview"></a>概述

Azure 磁盘加密利用 Linux 中的 dm-crypt 子系统在[选择 Azure Linux 发行版](https://aka.ms/adelinux)上提供完整磁盘加密。  此解决方案与 Azure Key Vault 集成，用于管理磁盘加密密钥和机密。

## <a name="prerequisites"></a>先决条件

有关先决条件的完整列表，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md#prerequisites)。
<!--Pending on (../../security/azure-security-disk-encryption-prerequisites.md)-->

### <a name="operating-system"></a>操作系统

目前，选择的发行版和版本支持 Azure 磁盘加密。

<!-- Not Available on [Azure Disk Encryption FAQ](../../security/azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport)-->
### <a name="internet-connectivity"></a>Internet 连接

适用于 Linux 的 Azure 磁盘加密需要 Internet 连接才能访问 Active Directory、Key Vault、存储和包管理终结点。  有关详细信息，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md#prerequisites)。
<!--Pending on (../../security/azure-security-disk-encryption-prerequisites.md)-->

## <a name="extension-schema"></a>扩展架构

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>属性值

| 名称 | 值/示例 | 数据类型 |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | 日期 |
| 发布者 | Microsoft.Azure.Security | 字符串 |
| type | AzureDiskEncryptionForLinux | 字符串 |
| typeHandlerVersion | 0.1, 1.1 (VMSS) | int |
| AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| AADClientSecret | password | 字符串 |
| AADClientCertificate | thumbprint | 字符串 |
| DiskFormatQuery | {"dev_path":"","name":"","file_system":""} | JSON 字典 |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | 字符串 | 
| KeyEncryptionAlgorithm | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | 字符串 |
| KeyEncryptionKeyURL | url | 字符串 |
| KeyVaultURL | url | 字符串 |
| 通行短语 | password | 字符串 | 
| SequenceVersion | uniqueidentifier | 字符串 |
| VolumeType | OS, Data, All | 字符串 |

## <a name="template-deployment"></a>模板部署

有关模板部署的示例，请参阅[在正在运行的 Linux VM 上启用加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)。

## <a name="azure-cli-deployment"></a>Azure CLI 部署

可以在最新 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/vm/encryption?view=azure-cli-latest)中找到相关说明。 

<!-- Not Available on ## Troubleshoot and support
<!-- Not Available on ### Troubleshoot-->
<!-- Not Available on [Azure Disk Encryption troubleshooting guide](../../security/azure-security-disk-encryption-tsg.md)-->

## <a name="support"></a>支持

如果对本文中的任何观点存在疑问，可以联系 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/contact/) 上的 Azure 专家。 有关使用 Azure 支持的信息，请阅读 [Azure 支持常见问题](https://www.azure.cn/support/faq/)。
<!-- Not Available on [MSDN Azure and CSDN Azure](https://www.azure.cn/support/community/)-->

## <a name="next-steps"></a>后续步骤

有关 VM 扩展的详细信息，请参阅[适用于 Linux 的虚拟机扩展和功能](features-linux.md)。
<!-- Update_Description: wording update, update meta properties -->
