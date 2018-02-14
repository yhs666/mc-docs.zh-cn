---
title: "如何使用 Packer 创建 Windows Azure VM 映像 | Azure"
description: "了解如何使用 Packer 在 Azure 中创建 Windows 虚拟机映像"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 12/18/2017
ms.date: 02/05/2018
ms.author: v-yeche
ms.openlocfilehash: 968dcb3d6a8f59226c93a1be6648836be7c8ebd1
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>如何使用 Packer 在 Azure 中创建 Windows 虚拟机映像
Azure 中的每个虚拟机 (VM) 都是基于定义 Windows 分发和操作系统版本的映像创建的。 映像可以包括预安装的应用程序和配置。 Azure 应用商店为最常见的操作系统和应用程序环境提供了许多第一和第三方映像，也可以根据需要创建自己的自定义映像。 本文详细介绍如何使用开源工具 [Packer](https://www.packer.io/) 在 Azure 中定义和生成自定义映像。

## <a name="create-azure-resource-group"></a>创建 Azure 资源组
生成过程中，Packer 将在生成源 VM 时创建临时 Azure 资源。 要捕获该源 VM 用作映像，必须定义资源组。 Packer 生成过程的输出存储在此资源组中。

使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建资源组。 以下示例在“chinaeast”位置创建名为“myResourceGroup”的资源组：

```powershell
$rgName = "myResourceGroup"
$location = "China East"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>创建 Azure 凭据
使用服务主体通过 Azure 对 Packer 进行身份验证。 Azure 服务主体是可与应用、服务和自动化工具（如 Packer）结合使用的安全性标识。 用户控制和定义服务主体可在 Azure 中执行的操作的权限。

使用 [New-AzureRmADServicePrincipal](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermadserviceprincipal) 创建服务主体，并为服务主体分配权限，以便使用 [New-AzureRmRoleAssignment](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermroleassignment) 创建和管理资源：

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "Azure Packer" `
    -Password (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

若要向 Azure 进行身份验证，还需使用 [Get-AzureRmSubscription](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermsubscription) 获取 Azure 租户 ID 和订阅 ID：

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId
$sub.SubscriptionId
```

在下一步中将使用这两个 ID。

## <a name="define-packer-template"></a>定义 Packer 模板
若要生成映像，请创建一个模板作为 JSON 文件。 在模板中，定义执行实际生成过程的生成器和设置程序。 Packer 具有 [Azure 设置程序](https://www.packer.io/docs/builders/azure.html)，允许定义 Azure 资源，如在前一步骤中创建的服务主体凭据。

创建名为 windows.json 的文件并粘贴以下内容。 为以下内容输入自己的值：

| 参数                           | 获取位置 |
|-------------------------------------|----------------------------------------------------|
| client_id                         | 通过 `$sp.applicationId` 查看服务主体 ID |
| client_secret                     | 在 `$securePassword` 中指定的密码 |
| tenant_id                         | `$sub.TenantId` 命令的输出 |
| subscription_id                   | `$sub.SubscriptionId` 命令的输出 |
| object_id                         | 通过 `$sp.Id` 查看服务主体对象 ID |
| *managed_image_resource_group_name* | 在第一步中创建的资源组的名称 |
| *managed_image_name*                | 创建的托管磁盘映像的名称 |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": "true",
    "winrm_insecure": "true",
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "cloud_environment_name": "AzureChinaCloud",
    "location": "China East",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force}",
      "& $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /shutdown /quiet"
    ]
  }]
}
```
<!-- "cloud_environment_name": "AzureChinaCloud" -->

此模板生成 Windows Server 2016 VM 并安装 IIS，然后使用 Sysprep 来通用化该 VM。

## <a name="build-packer-image"></a>生成 Packer 映像
如果本地计算机上尚未安装 Packer，请[按照 Packer 安装说明](https://www.packer.io/docs/install/index.html)进行安装。

按如下所述指定 Packer 模板文件，生成映像：

```bash
./packer build windows.json
```

前面命令的输出示例如下所示：

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> Location          : 'China East'
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> DeploymentName    : 'pkrdppq0mthtbtt'
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> DeploymentName    : 'pkrdppq0mthtbtt'
==> azure-arm: Getting the certificate's URL ...
==> azure-arm:  -> Key Vault Name        : 'pkrkvpq0mthtbtt'
==> azure-arm:  -> Key Vault Secret Name : 'packerKeyVaultSecret'
==> azure-arm:  -> Certificate URL       : 'https://pkrkvpq0mthtbtt.vault.azure.cn/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate's URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> DeploymentName    : 'pkrdppq0mthtbtt'
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> DeploymentName    : 'pkrdppq0mthtbtt'
==> azure-arm: Getting the VM's IP address ...
==> azure-arm:  -> ResourceGroupName   : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> PublicIPAddressName : 'packerPublicIP'
==> azure-arm:  -> NicName             : 'packerNic'
==> azure-arm:  -> Network Connection  : 'PublicEndpoint'
==> azure-arm:  -> IP Address          : '40.76.55.35'
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04"><Obj S="progress" RefId="0"><TN RefId="0"><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N="SourceId">1</I64><PR N="Record"><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine's properties ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> ComputeName       : 'pkrvmpq0mthtbtt'
==> azure-arm:  -> Managed OS Disk   : '/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk'
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> ComputeName       : 'pkrvmpq0mthtbtt'
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm:  -> Compute Name              : 'pkrvmpq0mthtbtt'
==> azure-arm:  -> Compute Location          : 'China East'
==> azure-arm:  -> Image ResourceGroupName   : 'myResourceGroup'
==> azure-arm:  -> Image Name                : 'myPackerImage'
==> azure-arm:  -> Image Location            : 'chinaeast'
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : 'packer-Resource-Group-pq0mthtbtt'
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build 'azure-arm' finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: chinaeast
```

Packer 需要几分钟时间来生成 VM、运行设置程序并清理部署。

## <a name="create-vm-from-azure-image"></a>从 Azure 映像创建 VM
现在可使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 从映像创建 VM。 首先，使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 的管理员用户名和密码。

```powershell
$cred = Get-Credential
```

以下示例从 myPackerImage 创建一个名为 myVM 的 VM。

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod "Static" `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Define the image created by Packer
$image = Get-AzureRMImage -ImageName myPackerImage -ResourceGroupName $rgName

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```

基于 Packer 映像创建 VM 需要几分钟时间。

## <a name="test-vm-and-iis"></a>测试 VM 和 IIS
使用 [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 获取 VM 的公共 IP 地址。 以下示例获取前面创建的“myPublicIP”的 IP 地址：

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIP" | select "IpAddress"
```

然后，可将公共 IP 地址输入 Web 浏览器中。

![IIS 默认站点](./media/build-image-with-packer/iis.png) 

## <a name="next-steps"></a>后续步骤
在此示例中，使用 Packer 创建已安装 IIS 的 VM 映像。 可以将此 VM 映像与现有部署工作流配合使用，执行例如将应用部署到在使用 Team Services、Ansible、Chef 或 Puppet 通过映像创建的 VM 中的操作。

有关适用于其他 Windows 发行版本的额外 Packer 模板示例，请参阅此 [GitHub 存储库](https://github.com/hashicorp/packer/tree/master/examples/azure)。
<!--Update_Description: update meta properties, wording update -->