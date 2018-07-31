---
title: 适用于 Windows 的 Azure 磁盘加密 | Azure
description: 使用虚拟机扩展将 Azure 磁盘加密部署到 Windows 虚拟机。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 06/12/2018
ms.date: 07/30/2018
ms.author: v-yeche
ms.openlocfilehash: 49df6f9f180fa5b5687560f8938834b6a5b6de66
ms.sourcegitcommit: 35889b4f3ae51464392478a72b172d8910dd2c37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2018
ms.locfileid: "39261966"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>适用于 Windows 的 Azure 磁盘加密 (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>概述

Azure 磁盘加密利用 Bitlocker 在运行 Windows 的 Azure 虚拟机上提供完全磁盘加密。  此解决方案与 Azure Key Vault 集成，以管理 Key Vault 订阅中的磁盘加密密钥和机密。 

## <a name="prerequisites"></a>先决条件

有关先决条件的完整列表，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md#prerequisites)。

### <a name="operating-system"></a>操作系统

有关当前 Windows 版本的列表，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md#prerequisites)。

### <a name="internet-connectivity"></a>Internet 连接

Azure 磁盘加密需要 Internet 连接才能访问 Active Directory、Key Vault、存储和包管理终结点。  有关网络安全设置的详细信息，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md#prerequisites)。

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
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>属性值

| Name | 值/示例 | 数据类型 |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | 日期 |
| 发布者 | Microsoft.Azure.Security | 字符串 |
| type | AzureDiskEncryptionForWindows| 字符串 |
| typeHandlerVersion | 1.0, 2.2 (VMSS) | int |
| （可选）AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| （可选）AADClientSecret | password | 字符串 |
| （可选）AADClientCertificate | thumbprint | 字符串 |
| EncryptionOperation | EnableEncryption | 字符串 | 
| KeyEncryptionAlgorithm | RSA-OAEP | 字符串 |
| KeyEncryptionKeyURL | url | 字符串 |
| KeyVaultURL | url | 字符串 |
| SequenceVersion | uniqueidentifier | 字符串 |
| VolumeType | OS, Data, All | 字符串 |

## <a name="template-deployment"></a>模板部署
有关模板部署的示例，请参阅[从库映像创建新的加密 Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)。

## <a name="azure-cli-deployment"></a>Azure CLI 部署

可以在最新 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/vm/encryption?view=azure-cli-latest)中找到相关说明。 

<!-- Not Available on ## Troubleshoot and support-->
<!-- Not Available on ### Troubleshoot-->
<!-- Not Available on [Azure Disk Encryption troubleshooting guide](../../security/azure-security-disk-encryption-tsg.md)-->

## <a name="support"></a>支持

如果对本文中的任何观点存在疑问，可以联系 [Azure 支持站点](https://www.azure.cn/support/contact/)上的 Azure 专家。 有关使用 Azure 支持的信息，请阅读 [Azure 支持常见问题](https://www.azure.cn/support/faq/)。
<!-- Not Available on [MSDN Azure and CSDN Azure](https://www.azure.cn/support/community/)-->

## <a name="next-steps"></a>后续步骤
有关扩展的详细信息，请参阅[适用于 Windows 的虚拟机扩展和功能](features-windows.md)。
<!-- Update_Description: new articles on azure disk encrypt windows  -->
<!--ms.date: 07/30/2018-->