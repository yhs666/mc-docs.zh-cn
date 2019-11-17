---
title: 为 Windows VM 启用 Azure 磁盘加密
description: 本文提供有关如何为 Windows VM 启用 Azure 磁盘加密的说明。
author: rockboyfor
ms.service: security
ms.topic: article
ms.author: v-yeche
origin.date: 10/05/2019
ms.date: 11/11/2019
ms.custom: seodec18
ms.openlocfilehash: 18165a17c48ab03b5b7437c9e984d8bce9e10490
ms.sourcegitcommit: a89eb0007edd5b4558b98c1748b2bd67ca22f4c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2019
ms.locfileid: "73730676"
---
# <a name="azure-disk-encryption-for-windows-vms"></a>适用于 Windows VM 的 Azure 磁盘加密 

Azure 磁盘加密有助于保护数据，使组织能够信守在安全性与合规性方面作出的承诺。 它使用 Windows 的 [Bitlocker](https://en.wikipedia.org/wiki/BitLocker) 功能为 Azure 虚拟机 (VM) 的操作系统和数据磁盘提供卷加密，并与 [Azure 密钥保管库](../../key-vault/index.yml)集成，以帮助你控制和管理磁盘加密密钥和机密。 

如果使用 [Azure 安全中心](../../security-center/index.yml)，当 VM 未加密时，你会收到警报。 这些警报显示为“高严重性”，建议加密这些 VM。

![Azure 安全中心磁盘加密警报](../media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - 如果之前是使用 Azure 磁盘加密与 Azure AD 来加密 VM，则必须继续使用此选项来加密 VM。 有关详细信息，请参阅 [使用 Azure AD 进行 Azure 磁盘加密（以前版本）](disk-encryption-overview-aad.md)。 
> - 某些建议可能会导致数据、网络或计算资源使用量增加，从而产生额外的许可或订阅成本。 必须具有有效的活动 Azure 订阅，才能在 Azure 的受支持区域中创建资源。

通过[使用 Azure CLI 创建和加密 Windows VM 快速入门](disk-encryption-cli-quickstart.md)或[使用 Azure Powershell 创建和加密 Windows VM 快速入门](disk-encryption-powershell-quickstart.md)，只需几分钟即可了解适用于 Windows 的 Azure 磁盘加密的基本知识。

## <a name="supported-vms-and-operating-systems"></a>支持的 VM 和操作系统

### <a name="supported-vm-sizes"></a>支持的 VM 大小

Windows VM 的大小有[多种](sizes-general.md)。 Azure 磁盘加密在 [A 系列基本 VM](https://www.azure.cn/pricing/details/virtual-machines/) 或内存小于 2 GB 的虚拟机上不可用。

<!--MOONCAKE: CORRECT ON https://www.azure.cn/pricing/details/virtual-machines/-->

Azure 磁盘加密还可用于使用高级存储的 VM。

### <a name="supported-operating-systems"></a>支持的操作系统

- Windows 客户端：Windows 8 和更高版本。
- Windows Server：Windows Server 2008 R2 和更高版本。  

> [!NOTE]
> Windows Server 2008 R2 要求安装 .NET Framework 4.5 以支持加密；请从 Windows 更新安装此组件，并安装适用于 Windows Server 2008 R2 基于 x64 的系统的 Microsoft .NET Framework 4.5.2 可选更新 ([KB2901983](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2901983))。  
>  
> Windows Server 2012 R2 Core 和 Windows Server 2016 Core 要求在 VM 安装 bdehdcfg 组件以支持加密。

<!--MOONCAKE: CORRECT ON Microsoft .NET Framework--
<a name="networking-requirements"></a>
## Networking requirements
To enable Azure Disk Encryption, the VMs must meet the following network endpoint configuration requirements:
  - To get a token to connect to your key vault, the Windows VM must be able to connect to an Azure Active Directory endpoint, \[login.chinacloudapi.cn\].
  - To write the encryption keys to your key vault, the Windows VM must be able to connect to the key vault endpoint.
  - The Windows VM must be able to connect to an Azure storage endpoint that hosts the Azure extension repository and an Azure storage account that hosts the VHD files.
  -  If your security policy limits access from Azure VMs to the Internet, you can resolve the preceding URI and configure a specific rule to allow outbound connectivity to the IPs. For more information, see [Azure Key Vault behind a firewall](../../key-vault/key-vault-access-behind-firewall.md).    

<a name="group-policy-requirements"></a>
## Group Policy requirements

Azure Disk Encryption uses the BitLocker external key protector for Windows VMs. For domain joined VMs, don't push any group policies that enforce TPM protectors. For information about the group policy for "Allow BitLocker without a compatible TPM," see [BitLocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1).

BitLocker policy on domain joined virtual machines with custom group policy must include the following setting: [Configure user storage of BitLocker recovery information -> Allow 256-bit recovery key](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Azure Disk Encryption will fail when custom group policy settings for BitLocker are incompatible. On machines that didn't have the correct policy setting, apply the new policy, force the new policy to update (gpupdate.exe /force), and then restarting may be required.

Azure Disk Encryption will fail if domain level group policy blocks the AES-CBC algorithm, which is used by BitLocker.

<a name="encryption-key-storage-requirements"></a>
## Encryption key storage requirements  

Azure Disk Encryption requires an Azure Key Vault to control and manage disk encryption keys and secrets. Your key vault and VMs must reside in the same Azure region and subscription.

For details, see [Creating and configuring a key vault for Azure Disk Encryption](disk-encryption-key-vault.md).

## Terminology
The following table defines some of the common terms used in Azure disk encryption documentation:

| Terminology | Definition |
| --- | --- |
| Azure Key Vault | Key Vault is a cryptographic, key management service that's based on Federal Information Processing Standards (FIPS) validated hardware security modules. These standards help to safeguard your cryptographic keys and sensitive secrets. For more information, see the [Azure Key Vault](https://www.azure.cn/home/features/key-vault/) documentation and [Creating and configuring a key vault for Azure Disk Encryption](disk-encryption-key-vault.md). |
| Azure CLI | [The Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest) is optimized for managing and administering Azure resources from the command line.|
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is an industry-recognized Windows volume encryption technology that's used to enable disk encryption on Windows VMs. |
| Key encryption key (KEK) | The asymmetric key (RSA 2048) that you can use to protect or wrap the secret.  For more information, see the [Azure Key Vault](https://www.azure.cn/home/features/key-vault/) documentation and [Creating and configuring a key vault for Azure Disk Encryption](disk-encryption-key-vault.md). |
| PowerShell cmdlets | For more information, see [Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/overview). |

<!--Not Available on | Key encryption key (KEK) |  You can provide a hardware security module (HSM)-protected key or software-protected key. |-->

## <a name="next-steps"></a>后续步骤

- [快速入门 - 使用 Azure CLI 创建和加密 Windows VM](disk-encryption-cli-quickstart.md)
- [快速入门 - 使用 Azure Powershell 创建和加密 Windows VM](disk-encryption-powershell-quickstart.md)
- [Windows VM 上的 Azure 磁盘加密方案](disk-encryption-windows.md)
- [Azure 磁盘加密先决条件 CLI 脚本](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure 磁盘加密先决条件 PowerShell 脚本](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [创建和配置用于 Azure 磁盘加密的密钥保管库](disk-encryption-key-vault.md)

<!--Update_Description: new articles on disk encryption overview -->
<!--New.date: 11/04/2019-->
