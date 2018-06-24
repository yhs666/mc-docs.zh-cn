---
title: Azure PowerShell 脚本示例 - 加密 Windows VM | Azure
description: Azure PowerShell 脚本示例 - 加密 Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 12/12/2017
ms.date: 06/25/2018
ms.author: v-yeche
ms.openlocfilehash: f59c9a1da7bdd9b36c946b7de9d13a9656c312ad
ms.sourcegitcommit: 092d9ef3f2509ca2ebbd594e1da4048066af0ee3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36315660"
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>使用 Azure PowerShell 加密 Windows 虚拟机

此脚本创建安全的 Azure Key Vault、加密密钥、Azure Active Directory 服务主体和 Windows 虚拟机 (VM)。 然后使用来自 Key Vault 和服务主体凭据的加密密钥对 VM 进行加密。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Edit these global variables with you unique Key Vault name, resource group name and location
$keyVaultName = "myKeyVault"
$rgName = "myResourceGroup"
$location = "China East"

# Register the Key Vault provider and create a resource group
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName

# Create a Key Vault and enable it for disk encryption
New-AzureRmKeyVault `
    -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption

# Create a key in your Key Vault
Add-AzureKeyVaultKey `
    -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"

# Create Azure Active Directory app and service principal
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

# Set permissions to allow your AAD service principal to read keys from Key Vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId  `
    -PermissionsToKeys "all" `
    -PermissionsToSecrets "all"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine
New-AzureRmVM `
  -ResourceGroupName $resourceGroup `
  -Name $vmName `
  -Location $location `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIp" `
  -Credential $cred `
  -OpenPorts 3389

# Define required information for our Key Vault and keys
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name "myKey").Key.kid;

# Encrypt our virtual machine
Set-AzureRmVMDiskEncryptionExtension `
    -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId

# View encryption status
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/new-azurermkeyvault) | 创建 Azure Key Vault 来存储安全的数据，例如加密密钥。 |
| [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | 在 Key Vault 中创建加密密钥。 |
| [New-AzureRmADServicePrincipal](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | 创建 Azure Active Directory 服务主体，安全地进行身份验证并控制对加密密钥的访问。 |
| [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | 设置对 Key Vault 的权限，授予服务主体访问加密密钥的权限。 |
| [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和网络安全组。 此命令还将打开端口 80 并设置管理凭据。 |
| [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault) | 获取有关 Key Vault 的所需信息 |
| [Set-AzureRmVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | 使用服务主体凭据和加密密钥对 VM 进行加密。 |
| [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | 显示 VM 加密过程的状态。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。
<!-- Update_Description: update meta properties, update cmdlet content -->