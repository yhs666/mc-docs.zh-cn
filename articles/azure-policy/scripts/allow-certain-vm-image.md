---
title: Azure Policy json 示例 - 只允许某个特定的 VM 平台映像 | Microsoft Docs
description: 此 json 示例策略要求虚拟机使用特定版本的 UbuntuServer。
services: azure-policy
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
origin.date: 10/30/2017
ms.date: 07/09/2018
ms.author: v-nany
ms.custom: mvc
ms.openlocfilehash: c03b0b107f91c253e81e46fdba2b486aafe6e2c8
ms.sourcegitcommit: 1742417f2a77050adf80a27c2d67aff4c456549e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46526860"
---
# <a name="only-allow-a-certain-vm-platform-image"></a>只允许某个特定的 VM 平台映像

要求虚拟机使用 UbuntuServer 的特定版本。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

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

[![“部署到 Azure”](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fplatform-image-policy%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>使用 PowerShell 进行部署

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "platform-image-policy" -DisplayName "Only allow a certain VM platform image" -description "This policy ensures that only UbuntuServer, Canonical is allowed from the image repository" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>清理 PowerShell 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```cli
az policy definition create --name 'platform-image-policy' --display-name 'Only allow a certain VM platform image' --description 'This policy ensures that only UbuntuServer, Canonical is allowed from the image repository' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/platform-image-policy/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "platform-image-policy"
```

### <a name="clean-up-azure-cli-deployment"></a>清理 Azure CLI 部署

运行以下命令来删除资源组、VM 和所有相关资源。

```cli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](../json-samples.md)中查看更多示例。