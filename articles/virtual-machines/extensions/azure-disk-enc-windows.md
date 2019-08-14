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
ms.date: 08/12/2019
ms.author: v-yeche
ms.openlocfilehash: 558110e1f5872222b006a6ff1fc71bc87cc76d3f
ms.sourcegitcommit: 8ac3d22ed9be821c51ee26e786894bf5a8736bfc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913051"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>适用于 Windows 的 Azure 磁盘加密 (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>概述

Azure 磁盘加密利用 BitLocker 在运行 Windows 的 Azure 虚拟机上提供完全磁盘加密。  此解决方案与 Azure Key Vault 集成，以管理 Key Vault 订阅中的磁盘加密密钥和机密。 

## <a name="prerequisites"></a>先决条件

有关先决条件的完整列表，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md)

<!--Pending on (../../security/azure-security-disk-encryption-prerequisites.md)-->

### <a name="operating-system"></a>操作系统

有关当前 Windows 版本的列表，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md)。

<!--Pending on (../../security/azure-security-disk-encryption-prerequisites.md)-->

### <a name="internet-connectivity"></a>Internet 连接

Azure 磁盘加密需要 Internet 连接才能访问 Active Directory、Key Vault、存储和包管理终结点。  有关网络安全设置的详细信息，请参阅 [Azure 磁盘加密先决条件](../../security/azure-security-disk-encryption.md)。

<!--Pending on (../../security/azure-security-disk-encryption-prerequisites.md)-->

## <a name="extension-schemata"></a>扩展架构

Azure 磁盘加密有两种架构：v1.1，一种不使用 Azure Active Directory (AAD) 属性的较新推荐架构；v0.1，一种需要 AAD 属性的较旧架构。 你必须使用与所使用的扩展对应的架构版本：架构 v1.1 用于 AzureDiskEncryption 扩展版本 1.1，架构 v0.1 用于 AzureDiskEncryption 扩展版本 0.1。

### <a name="schema-v11-no-aad-recommended"></a>架构 v1.1：无 AAD（推荐）

建议使用 v1.1 架构，它不需要 Azure Active Directory 属性。

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="schema-v01-with-aad"></a>架构 v0.1：使用 AAD 

0\.1 版架构需要 `aadClientID` 和 `aadClientSecret` 或 `AADClientCertificate`。

使用 `aadClientSecret`：

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

使用 `AADClientCertificate`：

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>属性值

| 名称 | 值/示例 | 数据类型 |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Azure.Security | string |
| type | AzureDiskEncryption | string |
| typeHandlerVersion | 0.1、1.1 | int |
| （0.1 版架构）AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| （0.1 版架构）AADClientSecret | password | string |
| （0.1 版架构）AADClientCertificate | thumbprint | string |
| DiskFormatQuery | {"dev_path":"","name":"","file_system":""} | JSON 字典 |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | string |
| KeyEncryptionKeyURL | url | string |
| KeyVaultURL | url | string |
| （可选）Passphrase | password | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | OS, Data, All | string |

<!--MOONCAKE: CORRECT ON AzureDiskEncryption to windows OS-->

## <a name="template-deployment"></a>模板部署
有关模板部署的示例，请参阅[从库映像创建新的加密 Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)。

## <a name="azure-cli-deployment"></a>Azure CLI 部署

可以在最新 [Azure CLI 文档](https://docs.azure.cn/cli/vm/encryption?view=azure-cli-latest)中找到相关说明。 

<!-- Not Available on ## Troubleshoot and support-->
<!-- Not Available on ### Troubleshoot-->
<!-- Not Available on [Azure Disk Encryption troubleshooting guide](../../security/azure-security-disk-encryption-tsg.md)-->

### <a name="support"></a>支持

如果对本文中的任何观点存在疑问，可以联系 [Azure 支持](https://support.azure.cn/support/contact/)上的 Azure 专家。 或者，也可以提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/support/support-azure/)提交请求。 有关使用 Azure 支持的信息，请阅读 [Azure 支持常见问题](https://www.azure.cn/support/faq/)。

<!--MOONCAKE: Not Available on and select Get support.-->

## <a name="next-steps"></a>后续步骤
有关扩展的详细信息，请参阅[适用于 Windows 的虚拟机扩展和功能](features-windows.md)。

<!-- Update_Description: wording update, update meta propertiess  -->