---
title: 使用安全地存放在 Azure Stack 上的密码部署 VM | Microsoft Docs
description: 了解如何使用存储在 Azure Stack Key Vault 中的密码部署 VM
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 23322a49-fb7e-4dc2-8d0e-43de8cd41f80
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 08/08/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.openlocfilehash: fe4bab8f5e5642671d8e75976b1629ecc54fc726
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-virtual-machine-by-retrieving-the-password-stored-in-a-key-vault"></a>通过检索 Key Vault 中存储的密码创建虚拟机

需要在部署期间传递安全值（例如密码）时，可以将该值存储为 Azure Stack Key Vault 中的机密，然后在其他 Azure 资源管理器模板中引用该值。 不需要在每次部署资源时手动输入机密，还可以指定哪些用户或服务主体可以访问机密。 

在本文中，我们引导你完成通过检索 Key Vault 中存储的密码在 Azure Stack 中部署 Windows 虚拟机所需的步骤。 因此，密码绝不会以纯文本的形式放在模板参数文件中。 可以通过 Azure Stack 开发工具包或者外部客户端（如果已通过 VPN 建立连接）执行这些步骤。

## <a name="prerequisites"></a>先决条件
 
- 必须订阅包含 Key Vault 服务的产品/服务。  
- [安装适用于 Azure Stack 的 PowerShell。](azure-stack-powershell-install.md)  
- [配置 Azure Stack 用户的 PowerShell 环境。](azure-stack-powershell-configure-user.md)

以下步骤说明通过检索 Key Vault 中存储的密码创建虚拟机所需的过程：

1. 创建 Key Vault 机密。
2. 更新 azuredeploy.parameters.json 文件。
3. 部署模板。

## <a name="create-a-key-vault-secret"></a>创建 Key Vault 机密

以下脚本创建密钥保管库，并将密码作为机密存储在密钥保管库中。 创建密钥保管库时，请使用 `-EnabledForDeployment` 参数。 此参数可确保能够从 Azure 资源管理器模板引用密钥保管库。

```powershell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

运行前面的脚本时，输出会包括机密 URI。 请记下此 URI。 在[使用密钥保管库模板中的密码部署 Windows 虚拟机](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv)中，必须引用此 URI。 将 [101-vm-secure-password](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) 文件夹下载到开发计算机上。 此文件夹包含 `azuredeploy.json` 和 `azuredeploy.parameters.json`文件，在后续步骤中将需要这些文件。

根据环境值，修改 `azuredeploy.parameters.json` 文件。 要注意的参数是保管库名称、保管库资源组和机密 URI（由前面的脚本生成）。 以下文件是参数文件的示例：

## <a name="update-the-azuredeployparametersjson-file"></a>更新 azuredeploy.parameters.json 文件

根据环境，以 KeyVault URI、secretName、虚拟机 adminUsername 的值更新 azuredeploy.parameters.json 文件。 以下 JSON 文件显示模板参数文件的示例： 

```json
{
    "$schema":  "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>模板部署

现在，使用以下 PowerShell 脚本部署模板：

```powershell
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```
成功部署模板后，会生成以下输出：

![部署输出](./media/azure-stack-kv-deploy-vm-with-secret/deployment-output.png)


## <a name="next-steps"></a>后续步骤
[使用 Key Vault 部署示例应用](azure-stack-kv-sample-app.md)

[使用 Key Vault 证书部署 VM](azure-stack-kv-push-secret-into-vm.md)


