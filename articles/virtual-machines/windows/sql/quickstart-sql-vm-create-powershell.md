---
title: 使用 Azure PowerShell 创建 SQL Server Windows VM | Azure
description: 本教程介绍如何使用 Azure PowerShell 创建 Windows SQL Server 2017 虚拟机。
services: virtual-machines-windows
documentationcenter: na
author: rockboyfor
manager: digimobile
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
origin.date: 02/15/2018
ms.date: 03/19/2018
ms.author: v-yeche
ms.openlocfilehash: 37f9975567ab139b06ccbe35a6f9a7b331d21800
ms.sourcegitcommit: 5bf041000d046683f66442e21dc6b93cb9d2f772
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2018
---
# <a name="quickstart-create-a-sql-server-windows-virtual-machine-with-azure-powershell"></a>快速入门：使用 Azure PowerShell 创建 SQL Server Windows 虚拟机

本快速入门逐步讲解如何使用 Azure PowerShell 创建 SQL Server 虚拟机。

> [!TIP]
> 本快速入门提供的路径适用于快速预配并连接到 SQL VM。 若要详细了解创建 SQL VM 所需的其他 Azure PowerShell 选项，请参阅 [SQL Server VM 预配指南（使用 Azure PowerShell）](virtual-machines-windows-ps-sql-create.md)。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

本快速入门需要 Azure PowerShell 模块 3.6 版或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

## <a name="configure-powershell"></a>配置 PowerShell

1. 打开 PowerShell，然后运行 **Add-AzureRmAccount** 命令访问自己的 Azure 帐户。

   ```PowerShell
   Add-AzureRmAccount -EnvironmentName AzureChinaCloud
   ```

1. 此时应会出现一个用于输入凭据的登录屏幕。 使用登录 Azure 门户时所用的相同电子邮件和密码。

## <a name="create-a-resource-group"></a>创建资源组

1. 使用唯一的资源组名称定义一个变量。 若要简化本快速入门的余下操作，剩余的命令将使用此名称作为其他资源名称的基础。

   ```PowerShell
   $ResourceGroupName = "sqlvm1"
   ```

1. 为所有 VM 资源定义目标 Azure 区域的位置。

   ```PowerShell
   $Location = "China East"
   ```

1. 创建资源组。

   ```PowerShell
   New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
   ```

## <a name="configure-network-settings"></a>配置网络设置

1. 创建虚拟网络、子网和公共 IP 地址。 这些资源用来与虚拟机建立网络连接，以及连接到 Internet。

   ``` PowerShell
   $SubnetName = $ResourceGroupName + "subnet"
   $VnetName = $ResourceGroupName + "vnet"
   $PipName = $ResourceGroupName + $(Get-Random)

   # Create a subnet configuration
   $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix 192.168.1.0/24

   # Create a virtual network
   $Vnet = New-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
      -Name VnetName -AddressPrefix 192.168.0.0/16 -Subnet $SubnetConfig

   # Create a public IP address and specify a DNS name
   $Pip = New-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
      -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name $PipName
   ```

1. 创建网络安全组。 配置允许远程桌面 (RDP) 和 SQL Server 连接的规则。

   ```PowerShell
   # Rule to allow remote desktop (RDP)
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow

   #Rule to allow SQL Server connections on port 1433
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow

   # Create the network security group
   $NsgName = $ResourceGroupName + "nsg"
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

1. 创建网络接口。

   ```PowerShell
   $InterfaceName = $ResourceGroupName + "int"
   $Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
      -ResourceGroupName $ResourceGroupName -Location $Location `
      -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $Pip.Id `
      -NetworkSecurityGroupId $Nsg.Id
   ```

## <a name="create-the-sql-vm"></a>创建 SQL VM

1. 定义用于登录 VM 的凭据。 用户名为“azureadmin”。 请务必在运行该命令之前更改密码。

   ``` PowerShell
   # Define a credential object
   $SecurePassword = ConvertTo-SecureString 'Change.This!000' `
      -AsPlainText -Force
   $Cred = New-Object System.Management.Automation.PSCredential ("azureadmin", $securePassword)
   ```

1. 创建虚拟机配置对象，然后创建 VM。 以下命令在 Windows Server 2016 上创建 SQL Server 2017 Developer Edition VM。

   ```PowerShell
   # Create a virtual machine configuration
   $VMName = $ResourceGroupName + "VM"
   $VMConfig = New-AzureRmVMConfig -VMName $VMName -VMSize Standard_DS13 | `
      Set-AzureRmVMOperatingSystem -Windows -ComputerName $VMName -Credential $Cred -ProvisionVMAgent -EnableAutoUpdate | `
      Set-AzureRmVMSourceImage -PublisherName "MicrosoftSQLServer" -Offer "SQL2017-WS2016" -Skus "SQLDEV" -Version "latest" | `
      Add-AzureRmVMNetworkInterface -Id $Interface.Id

   # Create the VM
   New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VMConfig
   ```

   > [!TIP]
   > 创建 VM 需要几分钟时间。

## <a name="install-the-sql-iaas-agent"></a>安装 SQL IaaS 代理

若要获取门户集成和 SQL VM 功能，必须安装 [SQL Server IaaS 代理扩展](virtual-machines-windows-sql-server-agent-extension.md)。 若要在新 VM 上安装该代理，请在创建 VM 后运行以下命令。

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remote-desktop-into-the-vm"></a>通过远程桌面连接到 VM

1. 使用以下命令检索新 VM 的公共 IP 地址。

   ```PowerShell
   Get-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName | Select IpAddress
   ```

1. 记下返回的 IP 地址，然后将其作为命令行参数传递给 **mstsc**，以便在新 VM 中启动远程桌面会话。

   ```
   mstsc /v:<publicIpAddress>
   ```

1. 出现输入凭据的提示时，请选择输入另一个帐户的凭据。 输入用户名并在其前面添加反斜杠（例如 `\azureadmin`），后接之前在本快速入门中设置的密码。

## <a name="connect-to-sql-server"></a>连接到 SQL Server

1. 登录到远程桌面会话后，通过开始菜单启动 **SQL Server Management Studio 2017**。

1. 在“连接到服务器”对话框中保留默认设置。 服务器名称是 VM 的名称。 身份验证设置为“Windows 身份验证”。 单击“连接” 。

现已在本地连接到 SQL Server。 如果想要远程建立连接，必须通过门户或手动[配置连接](virtual-machines-windows-sql-connect.md)。

## <a name="clean-up-resources"></a>清理资源

如果不需要让 VM 持续运行，可以在不使用它时将它停止，以免产生不必要的费用。 以下命令可停止 VM，但会保留它供将来使用。

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

还可以使用 **Remove-AzureRmResourceGroup** 命令永久删除与虚拟机关联的所有资源。 请小心使用此命令，因为它也会永久删除该虚拟机。

## <a name="next-steps"></a>后续步骤

本快速入门使用 Azure PowerShell 创建了一个 SQL Server 2017 虚拟机。 若要详细了解如何将数据迁移到新的 SQL Server，请参阅以下文章。

> [!div class="nextstepaction"]
> [将数据库迁移到 SQL VM](virtual-machines-windows-migrate-sql.md)
<!-- Update_Description: wording update  -->
