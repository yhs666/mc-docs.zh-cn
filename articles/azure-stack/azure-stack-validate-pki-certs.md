---
title: 验证用于 Azure Stack 集成系统部署的 Azure Stack 公钥基础结构证书 | Microsoft Docs
description: 介绍如何验证 Azure Stack 集成系统的 Azure Stack PKI 证书。 介绍如何使用 Azure Stack 证书检查器工具。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/11/2018
ms.date: 04/23/2018
ms.author: v-junlch
ms.reviewer: ppacent
ms.openlocfilehash: 4ce2975a8030e65a37e0dde9fe23b52d7c7d1a38
ms.sourcegitcommit: 85828a2cbfdb58d3ce05c6ef0bc4a24faf4d247b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
# <a name="validate-azure-stack-pki-certificates"></a>验证 Azure Stack PKI 证书

可[从 PowerShell 库](https://aka.ms/AzsReadinessChecker)获取本文所述的 Azure Stack 就绪性检查器工具。 使用该工具可以验证[生成的 PKI 证书](azure-stack-get-pki-certs.md)是否适用于前期部署。 应该留出足够的时间来验证证书，以测试证书并在必要时重新颁发证书。

就绪性检查器工具执行以下证书验证：

- **读取 PFX**  
    检查 PFX 文件是否有效且密码正确，在公开的信息不受到密码保护时发出警告。 
- **签名算法**  
    检查签名算法是否不是 SHA1。
- **私钥**  
    检查私钥是否存在，并且已连同“本地计算机”属性一起导出。 
- **证书链**  
    检查证书链是否完整，包括自签名证书。
- **DNS 名称**  
    检查 SAN 是否包含每个终结点的相关 DNS 名称，或支持性的通配符是否存在。
- **密钥用法**  
    检查密钥用法是否包含数字签名和密钥加密，增强型密钥用法是否包含服务器身份验证和客户端身份验证。
- **密钥大小**  
    检查密钥大小是否为 2048 或更大。
- **链序**  
    检查其他证书的顺序，验证顺序是否正确。
- **其他证书**  
    确保除了相关叶证书及其链以外，PFX 中未打包其他证书。

> [!IMPORTANT]  
> PKI 证书是一个 PFX 文件，其密码应被视为敏感信息。

## <a name="prerequisites"></a>先决条件

在验证用于 Azure Stack 部署的 PKI 证书之前，系统应符合以下先决条件：

- Azure Stack 就绪性检查器
- 遵照[准备说明](azure-stack-prepare-pki-certs.md)导出的 SSL 证书
- DeploymentData.json
- Windows 10 或 Windows Server 2016

## <a name="perform-certificate-validation"></a>执行证书验证

使用以下步骤来准备和验证 Azure Stack PKI 证书：

1. 在 PowerShell 提示符（5.1 或更高版本）下，运行以下 cmdlet 安装 AzsReadinessChecker：

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker 
    ````

2. 创建证书目录结构。 在以下示例中，可将 `<c:\certificates>` 更改为所选的新目录路径。

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal' 

    $destination = 'c:\certificates' 

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
    ````

    - 将证书放入上一步骤中创建的相应目录。 例如：  
        - c:\certificates\ACSBlob\CustomerCertificate.pfx 
        - c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx 
        - c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx 
        - 等等 

3. 在 PowerShell 窗口中运行：

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ````

4. 检查输出，验证所有证书是否都通过了测试。 例如：

    ````PowerShell
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Validation

    Starting Azure Stack Certificate Validation 1.1803.405.3
    Testing: ARM Public\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: ACSBlob\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessReport.json
    AzsReadinessChecker Completed
    ````

### <a name="known-issues"></a>已知问题

**症状**：已跳过测试

**原因**：如果不符合依赖关系，AzsReadinessChecker 会跳过某些测试：

 - 如果证书链出错，则会跳过其他证书。

    ````PowerShell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

**解决方法**：遵循针对每个证书的每组测试下的详细信息部分中的工具指导。

## <a name="using-validated-certificates"></a>使用已验证的证书

通过 AzsReadinessChecker 验证证书后，可在 Azure Stack 部署中使用这些证书，或者将其用于 Azure Stack 机密轮换。 

 - 对于部署，请根据 [Azure Stack PKI 要求文档](azure-stack-pki-certs.md)中的规定，安全地将证书传送给部署工程师，使他们能够将其复制到部署主机。
 - 对于机密轮换，可以遵循 [Azure Stack 机密轮换文档](azure-stack-rotate-secrets.md)，使用这些证书来更新 Azure Stack 环境公共基础结构终结点的旧证书。

## <a name="next-steps"></a>后续步骤

[数据中心标识集成](azure-stack-integrate-identity.md)

