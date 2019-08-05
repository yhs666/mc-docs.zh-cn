---
title: Azure Service Fabric 安全性最佳做法 | Azure
description: Azure Service Fabric 安全性最佳做法。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 01/23/2019
ms.date: 08/05/2019
ms.author: v-yeche
ms.openlocfilehash: ba29c4370449439e547997d239fe1c8bc19818dc
ms.sourcegitcommit: 86163e2669a646be48c8d3f032ecefc1530d3b7f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68753173"
---
# <a name="azure-service-fabric-security"></a>Azure Service Fabric 安全 

有关详细信息，请参阅 [Azure 安全最佳做法](/security/)。 

<!--Not Available on [Azure Service Fabric security best practices](/security/azure-service-fabric-security-best-practices)-->

## <a name="key-vault"></a>密钥保管库

[Azure Key Vault](/key-vault/) 是建议用于 Azure Service Fabric 应用程序和群集的机密管理服务。
> [!NOTE]
> 如果将 Key Vault 中的证书/机密以虚拟机规模集机密的形式部署到虚拟机规模集，则必须将 Key Vault 和虚拟机规模集并置。

## <a name="create-certificate-authority-issued-service-fabric-certificate"></a>创建证书颁发机构颁发的 Service Fabric 证书

可以在 Key Vault 中创建或导入 Azure Key Vault 证书。 创建 Key Vault 证书时，私钥在 Key Vault 中创建且不公开给证书所有者。 下面是在 Key Vault 中创建证书的方法：

- 创建自签名证书，以便创建一个公钥-私钥对并将其与证书相关联。 证书将通过其自身的密钥签名。 
- 手动创建新证书，以便创建一个公钥-私钥对并生成 X.509 证书签名请求。 签名请求可以由注册机构或证书颁发机构进行签名。 签名的 x509 证书可以与挂起的密钥对合并，以便完成 Key Vault 中的 KV 证书。 虽然此方法需要更多步骤，但其安全性更高，因为私钥是在 Key Vault 中创建的，其范围局限于 Key Vault。 下图对此进行了说明。 

如需更多详细信息，请参阅 [Azure Keyvault 证书创建方法](/key-vault/create-certificate)。

## <a name="deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets"></a>将 Key Vault 证书部署到 Service Fabric 群集虚拟机规模集

若要将证书从并置的 keyvault 部署到虚拟机规模集，请使用虚拟机规模集 [osProfile](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile)。 下面是资源管理器模板属性：

```json
"secrets": [
   {
       "sourceVault": {
           "id": "[parameters('sourceVaultValue')]"
       },
       "vaultCertificates": [
          {
              "certificateStore": "[parameters('certificateStoreValue')]",
              "certificateUrl": "[parameters('certificateUrlValue')]"
          }
       ]
   }
]
```

> [!NOTE]
> 必须启用保管库才能进行资源管理器模板部署。

## <a name="apply-an-access-control-list-acl-to-your-certificate-for-your-service-fabric-cluster"></a>将访问控制列表 (ACL) 应用到 Service Fabric 群集的证书

[虚拟机规模集扩展](https://docs.azure.cn/zh-cn/cli/vmss/extension?view=azure-cli-latest)发布服务器 Microsoft.Azure.ServiceFabric 用于配置节点安全性。
若要将 ACL 应用到 Service Fabric 群集过程的证书，请使用以下资源管理器模板属性：

```json
"certificate": {
   "commonNames": [
       "[parameters('certificateCommonName')]"
   ],
   "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

## <a name="secure-a-service-fabric-cluster-certificate-by-common-name"></a>通过公用名保护 Service Fabric 群集证书

若要通过证书 `Common Name` 来保护 Service Fabric 群集，请使用资源管理器模板属性 [certificateCommonNames](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterproperties#certificatecommonnames)，如下所示：

```json
"certificateCommonNames": {
    "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
        }
    ],
    "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

> [!NOTE]
> Service Fabric 群集将使用它在主机的证书存储中找到的第一个有效证书。 在 Windows 上，该证书将是具有最晚到期日期且与公用名和颁发者指纹匹配的证书。

Azure 域（例如 *\<你的子域\>.cloudapp.chinacloudapi.cn 或 \<你的子域\>.trafficmanager.cn）由 Microsoft 拥有。 证书颁发机构不会将域的证书颁发给未授权的用户。 大多数用户需要从注册机构购买域，或者需要是经授权的域管理员，否则证书颁发机构不会向其颁发具有该公用名的证书。

若要更详细地确定如何配置 DNS 服务，以便将域解析为 Azure IP 地址，请了解如何配置[用于托管域的 Azure DNS](/dns/dns-delegate-domain-azure-dns)。

> [!NOTE]
> 在将域名服务器委托给 Azure DNS 区域名称服务器以后，请将下面的两个记录添加到 DNS 区域：
> - 一个适用于域 APEX 的“A”记录，该域不是 `Alias record set`（对通过解析自定义域得来的所有 IP 地址而言）。
> - 一个适用于你所预配的 Azure 子域的“C”记录，这些子域不是 `Alias record set`。 例如，可以使用流量管理器或负载均衡器的 DNS 名称。

若要更新门户，以便显示 Service Fabric 群集 `"managementEndpoint"` 的自定义 DNS 名称，请更新以下 Service Fabric 群集资源管理器模板属性：

```json
 "managementEndpoint": "[concat('https://<YOUR CUSTOM DOMAIN>:',parameters('nt0fabricHttpGatewayPort'))]",
```

## <a name="encrypting-service-fabric-package-secret-values"></a>加密 Service Fabric 包机密值

在 Service Fabric 包中加密的常用值包括：Azure 容器注册表 (ACR) 凭据、环境变量、设置，以及 Azure 卷插件存储帐户密钥。

若要[在 Windows 群集上设置加密证书并对机密进行加密](/service-fabric/service-fabric-application-secret-management-windows)，请执行以下操作：

生成用于加密机密的自签名证书：

```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```

按照[将 Key Vault 证书部署到 Service Fabric 群集虚拟机规模集](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets)中的说明操作，将 Key Vault 证书部署到 Service Fabric 群集的虚拟机规模集。

使用以下 PowerShell 命令加密机密，然后使用加密的值更新 Service Fabric 应用程序清单：

``` powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

若要[在 Linux 群集上设置加密证书并对机密进行加密](/service-fabric/service-fabric-application-secret-management-linux)，请执行以下操作：

生成用于加密机密的自签名证书：

```bash
user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
user@linux:~$ cat TestCert.prv >> TestCert.pem
```

按照[将 Key Vault 证书部署到 Service Fabric 群集虚拟机规模集](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets)中的说明操作，将 Key Vault 证书部署到 Service Fabric 群集的虚拟机规模集。

使用以下命令加密机密，然后使用加密的值更新 Service Fabric 应用程序清单：

```bash
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```

在加密受保护的值以后，[在 Service Fabric 应用程序中指定加密的机密](/service-fabric/service-fabric-application-secret-management#specify-encrypted-secrets-in-an-application)，并[解密服务代码中加密的机密](/service-fabric/service-fabric-application-secret-management#decrypt-encrypted-secrets-from-service-code)。

<!--Not Available on ## Authenticate Service Fabric applications to Azure Resources using Managed Service Identity (MSI)-->


## <a name="windows-security-baselines"></a>Windows 安全基线
[我们建议实现广为人知且经过充分测试的业界标准配置，如 Azure 安全基线，而不是自行创建基线](https://docs.microsoft.com/zh-cn/windows/security/threat-protection/windows-security-baselines)；用于在虚拟机规模集上预配这些基线的一个选项是，使用 Azure Desired State Configuration (DSC) 扩展处理程序，以在 VM 处于联机状态时对其进行配置，以便其运行生产软件。

<!--Not Avaialble on ## Azure Firewall-->

## <a name="tls-12"></a>TLS 1.2
[TSG](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides/blob/master/Security/TLS%20Configuration.md)

## <a name="windows-defender"></a>Windows Defender 

默认情况下，Windows Defender 防病毒安装在 Windows Server 2016 上。 有关详细信息，请参阅 [Windows Server 2016 上的 Windows Defender 防病毒](https://docs.microsoft.com/zh-cn/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)。 用户界面默认安装在某些 SKU 上，但不是必需的。 若要降低 Windows Defender 引发的性能影响和资源使用开销，在安全策略允许排除开源软件的进程和路径的情况下，请声明以下虚拟机规模集扩展资源管理器模板属性，将 Service Fabric 群集排除在扫描范围外：

```json
 {
    "name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
    "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
            "AntimalwareEnabled": "true",
            "Exclusions": {
                "Paths": "[concat(parameters('svcFabData'), ';', parameters('svcFabLogs'), ';', parameters('svcFabRuntime'))]",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
            },
            "RealtimeProtectionEnabled": "true",
            "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
            }
        },
        "protectedSettings": null
    }
}
```

> [!NOTE]
> 如果不使用 Windows Defender，请参阅有关配置规则的反恶意软件文档。 Linux 不支持 Windows Defender。

## <a name="platform-isolation"></a>平台隔离
默认情况下，Service Fabric 应用程序会被授予访问 Service Fabric 运行时本身的权限，这本身会通过以下不同形式表明：[环境变量](service-fabric-environment-variables-reference.md)（指向对应于应用程序和 Fabric 文件的主机上的文件路径）、进程间通信终结点（接受应用程序特定请求）和客户端证书（Fabric 希望应用程序使用该证书对自身进行身份验证）。 如果服务托管本身不信任的代码，建议禁用此 SF 运行时访问权限，除非明确需要。 该运行时的访问权限可使用应用程序清单的“策略”部分中的以下声明来删除： 

```xml
<ServiceManifestImport>
    <Policies>
        <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
    </Policies>
</ServiceManifestImport>

```

## <a name="next-steps"></a>后续步骤

* 在运行 Windows Server 的 VM 或计算机上创建群集：[创建适用于 Windows Server 的 Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)。
* 在运行 Linux 的 VM 或计算机上创建群集：[创建 Linux 群集](service-fabric-cluster-creation-via-portal.md)。
* 了解 [Service Fabric 支持选项](service-fabric-support.md)。

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png

<!--Update_Description: wording update -->
