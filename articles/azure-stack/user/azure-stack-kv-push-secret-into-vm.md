---
title: 使用安全地存放在 Azure Stack 上的证书部署虚拟机 | Microsoft Docs
description: 了解如何在 Azure Stack 中部署虚拟机，并使用密钥保管库将证书推送到该虚拟机
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 08/03/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.openlocfilehash: b0b5278518686519fc638282d2edbc3bf6664646
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-virtual-machine-and-include-certificate-retrieved-from-a-key-vault"></a>创建虚拟机，并加入从密钥保管库检索到的证书

本文可帮助你在 Azure Stack 中创建虚拟机，并将证书推送到该虚拟机。 

## <a name="prerequisites"></a>先决条件

- 必须订阅包含 Key Vault 服务的产品/服务。 
- [安装适用于 Azure Stack 的 PowerShell。](azure-stack-powershell-install.md)  
- [配置 Azure Stack 用户的 PowerShell 环境](azure-stack-powershell-configure-user.md)

Azure Stack 中的密钥保管库可用于存储证书。 证书在许多不同情况下都很有用。 例如，假设你在 Azure Stack 中有一个虚拟机正在运行需要证书的应用程序。 此证书可用于加密、向 Active Directory 进行身份验证或用于网站上的 SSL。 将证书存放在密钥保管库中有助于确保证书是安全的。

本文将引导你完成将证书推送到 Azure Stack 中的 Windows 虚拟机所需的步骤。 可以通过 Azure Stack 开发工具包或者基于 Windows 的外部客户端（如果已通过 VPN 建立连接）执行这些步骤。

以下步骤说明将证书推送到虚拟机所需的过程：

1. 创建 Key Vault 机密。
2. 更新 azuredeploy.parameters.json 文件。
3. 部署模板

## <a name="create-a-key-vault-secret"></a>创建 Key Vault 机密

以下脚本会创建 .pfx 格式的证书、创建密钥保管库，并将该证书作为机密存储在密钥保管库中。 创建密钥保管库时，必须使用 `-EnabledForDeployment` 参数。 此参数可确保能够从 Azure 资源管理器模板引用密钥保管库。

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

运行前面的脚本时，输出会包括机密 URI。 请记下此 URI。 在[将证书推送到 Windows 资源管理器模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate)中，必须引用此 URI。 将 [vm-push-certificate-windows 模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate)文件夹下载到开发计算机上。 此文件夹包含 `azuredeploy.json` 和 `azuredeploy.parameters.json`文件，在后续步骤中将需要这些文件。

根据环境值，修改 `azuredeploy.parameters.json` 文件。 要注意的参数是保管库名称、保管库资源组和机密 URI（由前面的脚本生成）。 以下文件是参数文件的示例：

## <a name="update-the-azuredeployparametersjson-file"></a>更新 azuredeploy.parameters.json 文件

根据环境，以 vaultName、机密 URI、VmName 和其他值更新 azuredeploy.parameters.json 文件。 以下 JSON 文件显示模板参数文件的示例： 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>部署模板

现在，使用以下 PowerShell 脚本部署模板：

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

成功部署模板后，会生成以下输出：

![部署输出](./media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

部署此虚拟机时，Azure Stack 会将证书推送到虚拟机。 在 Windows 中，系统会利用用户提供的证书存储，将证书添加到 LocalMachine 证书位置。 在 Linux 中，证书会置于 /var/lib/waagent 目录下，其中 x509 证书文件的文件名为 &lt;UppercaseThumbprint&gt;.crt，且私钥的文件名为 &lt;UppercaseThumbprint&gt;.prv。

## <a name="retire-certificates"></a>停用证书

在前面的部分中，我们演示了如何将新证书推送到虚拟机。 旧证书仍然在虚拟机上，而且无法删除。 但是，可以使用 `Set-AzureKeyVaultSecretAttribute` cmdlet 禁用旧版的机密。 下面是此 cmdlet 的用法示例。 请确保根据环境替换保管库名称、机密名称和版本值：

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>后续步骤

- [使用密钥保管库密码部署 VM](azure-stack-kv-deploy-vm-with-secret.md)
- [允许应用程序访问 Key Vault](azure-stack-kv-sample-app.md)



