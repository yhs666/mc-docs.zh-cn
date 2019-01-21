---
title: 在 Azure Service Fabric Windows 群集上设置加密证书并对机密进行加密 | Azure
description: 了解如何在 Windows 群集上设置加密证书并对机密进行加密。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 01/04/2019
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: 449e8c3265692c90b0e4d4614c98352b136d6ef7
ms.sourcegitcommit: 35a09a86cbb3d896fa9784471ece41df7728bd71
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396715"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-windows-clusters"></a>在 Windows 群集上设置加密证书并对机密进行加密
本文展示了如何在 Windows 群集上设置加密证书并使用它来加密机密。 对于 Linux 群集，请参阅[在 Linux 群集上设置加密证书并对机密进行加密][secret-management-linux-specific-link]。

[Azure 密钥保管库][key-vault-get-started]在此处用作证书的安全存储位置，可用于将证书安装在 Azure 中的 Service Fabric 群集上。 如果不部署到 Azure，则不需要使用密钥保管库来管理 Service Fabric 应用程序中的机密。 但是，在应用程序中 *使用* 机密的方式不区分云平台，因此可让应用程序部署到托管在任何位置的群集。 

## <a name="obtain-a-data-encipherment-certificate"></a>获取数据加密证书
数据加密证书专门用来对服务 Settings.xml 中的[参数][parameters-link]以及服务 ServiceManifest.xml 中的[环境变量][environment-variables-link]进行加密和解密。 它不用于密码文本的身份验证或签名。 该证书必须满足以下要求：

* 证书必须包含私钥。
* 必须为密钥交换创建证书，并且该证书可导出到个人信息交换 (.pfx) 文件。
* 证书密钥用法必须包括数据加密 (10)，不应包括服务器身份验证或客户端身份验证。 

  例如，使用 PowerShell 创建自签名证书时，`KeyUsage` 标志必须设置为 `DataEncipherment`：

  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>在群集中安装证书
必须在群集中的每个节点上安装此证书。 有关设置说明，请参阅 [how to create a cluster using Azure Resource Manager][service-fabric-cluster-creation-via-arm] （如何使用 Azure Resource Manager 创建群集）。 

## <a name="encrypt-application-secrets"></a>加密应用程序机密
以下 PowerShell 命令用于加密机密。 此命令仅加密值；它并 **不** 对密码文本进行签名。 若要生成机密值的密文，必须使用群集中安装的同一个加密证书：

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

生成的 base-64 编码字符串包含机密密文，以及用来将其加密的证书相关信息。

## <a name="next-steps"></a>后续步骤
了解如何[在应用程序中指定加密的机密。][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application

<!-- Update_Description: new articles on service fabric application secret management windows -->
<!--ms.date: 01/21/2019-->