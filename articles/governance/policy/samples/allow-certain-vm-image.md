---
title: 示例 - 只允许某个特定的 VM 平台映像
description: 此示例策略定义要求虚拟机使用特定版本的 UbuntuServer。
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 01/23/2019
ms.date: 10/12/2019
ms.author: v-tawe
ms.openlocfilehash: fd6d0f4e3fb49f67bf2ea52327d7a37fb1bc6bf1
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526690"
---
# <a name="sample---only-allow-a-certain-vm-platform-image"></a>示例 - 只允许某个特定的 VM 平台映像

要求虚拟机使用 UbuntuServer 的特定版本。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>示例模板
 ```json
 {
    "type": "Microsoft.Authorization/policyDefinitions",
    "name": "platform-image-policy",
    "properties": {
        "displayName": "Only allow a certain VM platform image",
        "description": "This policy ensures that only UbuntuServer, Canonical is allowed from the image repository",
        "parameters": {},
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "in": [
                            "Microsoft.Compute/disks",
                            "Microsoft.Compute/virtualMachines",
                            "Microsoft.Compute/VirtualMachineScaleSets"
                        ]
                    },
                    {
                        "not": {
                            "allOf": [
                                {
                                    "field": "Microsoft.Compute/imagePublisher",
                                    "in": [
                                        "Canonical"
                                    ]
                                },
                                {
                                    "field": "Microsoft.Compute/imageOffer",
                                    "in": [
                                        "UbuntuServer"
                                    ]
                                },
                                {
                                    "field": "Microsoft.Compute/imageSku",
                                    "in": [
                                        "14.04.2-LTS"
                                    ]
                                },
                                {
                                    "field": "Microsoft.Compute/imageVersion",
                                    "in": [
                                        "latest"
                                    ]
                                }
                            ]
                        }
                    }
                ]
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```
可将 [Azure 门户](#deploy-with-the-portal)与 [PowerShell](#deploy-with-powershell) 或 [Azure CLI](#deploy-with-azure-cli) 配合使用来部署此模板。

## <a name="deploy-with-the-portal"></a>使用门户进行部署

[![将策略示例部署到 Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fplatform-image-policy%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = New-AzPolicyDefinition -Name "platform-image-policy" -DisplayName "Only allow a certain VM platform image" -description "This policy ensures that only UbuntuServer, Canonical is allowed from the image repository" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli
az policy definition create --name 'platform-image-policy' --display-name 'Only allow a certain VM platform image' --description 'This policy ensures that only UbuntuServer, Canonical is allowed from the image repository' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "platform-image-policy"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](index.md)中查看更多示例