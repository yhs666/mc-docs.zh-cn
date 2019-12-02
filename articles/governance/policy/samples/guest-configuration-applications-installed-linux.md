---
title: 示例 - 在 Linux VM 上审核缺少的应用程序
description: 此示例“策略来宾配置”计划和定义会审核是否未在 Linux 虚拟机中安装指定的应用程序。
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 05/02/2019
ms.date: 10/12/2019
ms.author: dacoulte
ms.openlocfilehash: 375aeee767063f7b0f8188bb1034792ec916a118
ms.sourcegitcommit: 298eab5107c5fb09bf13351efeafab5b18373901
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2019
ms.locfileid: "74658074"
---
# <a name="sample---audit-if-specified-applications-arent-installed-inside-linux-vms"></a>示例 - 审核是否未在 Linux VM 中安装指定的应用程序

当指定的应用程序未安装在 Linux 虚拟机中时，此“策略来宾配置”计划会创建一个审核事件。 此内置计划的 ID 为 `/providers/Microsoft.Authorization/policySetDefinitions/c937dcb4-4398-4b39-8d63-4a6be432252e`。

> [!IMPORTANT]
> 所有来宾配置计划均由 audit 和 deployIfNotExists 策略定义组成   。 如果仅分配其中一个策略定义，则无法正常执行来宾配置。

可使用以下项来分配此示例：

- [Azure 门户](#azure-portal)
- [Azure PowerShell](#azure-powershell)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="components-of-the-initiative"></a>计划的组成部分

此[来宾配置](../concepts/guest-configuration.md)计划由以下策略组成：

- [audit](#audit-definition) - 当应用程序未安装在 Linux VM 中时进行审核
  - ID：`/providers/Microsoft.Authorization/policyDefinitions/fee5cb2b-9d9b-410e-afe3-2902d90d0004`
- [deployIfNotExists](#deployIfNotExists-definition) - 部署 VM 扩展，这样，当应用程序未安装在 Linux VM 中时，就可以进行审核
  - ID：`/providers/Microsoft.Authorization/policyDefinitions/4d1c04de-2172-403f-901b-90608c35c721`

### <a name="initiative-definition"></a>计划定义

通过将 audit 和 deployIfNotExists 定义联接在一起并联接[计划参数](#initiative-parameters)来创建计划   。 这是定义的 JSON。

```json
{
    "properties": {
        "displayName": "Audit Linux VMs that do not have the specified applications installed",
        "description": "This initiative deploys the policy requirements and audits Linux virtual machines that do not have the specified applications installed. For more information on guest configuration policies, please visit https://aka.ms/gcpol",
        "parameters": {
            "applicationName": {
                "type": "String",
                "metadata": {
                    "displayName": "Application names",
                    "description": "A semicolon-separated list of the names of the applications that should be installed. e.g. python; powershell"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/fee5cb2b-9d9b-410e-afe3-2902d90d0004"
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/4d1c04de-2172-403f-901b-90608c35c721",
                "parameters": {
                    "applicationName": {
                        "value": "[parameters('applicationName')]"
                    }
                }
            }
        ]
    }
}
```

### <a name="initiative-parameters"></a>计划参数

|Name |类型 |说明 |
|---|---|---|
|applicationName |String |应用程序名称。 示例：“python”、“powershell”，或者“python,powershell”等以逗号分隔的列表。 使用 \* 进行通配符匹配，例如 'power\*'。 |

通过 PowerShell 或 Azure CLI 创建分配时，可以使用 `-PolicyParameter` (PowerShell) 或 `--params` (Azure CLI) 通过字符串或文件将参数值传递为 JSON。
PowerShell 还支持 `-PolicyParameterObject`，这要求向该 cmdlet 传递一个 Name/Value 哈希表，其中，**Name** 是参数名称，**Value** 是在赋值期间传递的单个值或值数组。

在此示例参数中，审核了 python 和 powershell 应用程序的安装情况   。

```json
{
    "applicationName": {
        "value": "python,powershell"
    }
}
```

只有 deployIfNotExists 策略定义才使用计划参数  。

### <a name="audit-definition"></a>审核定义

定义 audit 策略定义规则的 JSON  。

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.GuestConfiguration/guestConfigurationAssignments"
            },
            {
                "field": "name",
                "equals": "installed_application_linux"
            },
            {
                "field": "Microsoft.GuestConfiguration/guestConfigurationAssignments/complianceStatus",
                "notEquals": "Compliant"
            }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

### <a name="deployifnotexists-definition"></a>deployIfNotExists 定义

定义 deployIfNotExists 策略定义规则的 JSON  。

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines"
            },
            {
                "anyOf": [
                    {
                        "field": "Microsoft.Compute/imagePublisher",
                        "in": [
                            "microsoft-aks",
                            "AzureDatabricks",
                            "qubole-inc",
                            "datastax",
                            "couchbase",
                            "scalegrid",
                            "checkpoint",
                            "paloaltonetworks"
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "OpenLogic"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "like": "CentOS*"
                            },
                            {
                                "field": "Microsoft.Compute/imageSKU",
                                "notLike": "6*"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "RedHat"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "equals": "RHEL"
                            },
                            {
                                "field": "Microsoft.Compute/imageSKU",
                                "notLike": "6*"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "RedHat"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "equals": "osa"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "credativ"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "equals": "Debian"
                            },
                            {
                                "field": "Microsoft.Compute/imageSKU",
                                "notLike": "7*"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "Suse"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "like": "SLES*"
                            },
                            {
                                "field": "Microsoft.Compute/imageSKU",
                                "notLike": "11*"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "Canonical"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "equals": "UbuntuServer"
                            },
                            {
                                "field": "Microsoft.Compute/imageSKU",
                                "notLike": "12*"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "microsoft-dsvm"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "in": [
                                    "linux-data-science-vm-ubuntu",
                                    "azureml"
                                ]
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "cloudera"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "equals": "cloudera-centos-os"
                            },
                            {
                                "field": "Microsoft.Compute/imageSKU",
                                "notLike": "6*"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "cloudera"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "equals": "cloudera-altus-centos-os"
                            }
                        ]
                    },
                    {
                        "allOf": [
                            {
                                "field": "Microsoft.Compute/imagePublisher",
                                "equals": "microsoft-ads"
                            },
                            {
                                "field": "Microsoft.Compute/imageOffer",
                                "like": "linux*"
                            }
                        ]
                    }
                ]
            }
        ]
    },
    "then": {
        "effect": "deployIfNotExists",
        "details": {
            "type": "Microsoft.GuestConfiguration/guestConfigurationAssignments",
            "name": "installed_application_linux",
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ],
            "deployment": {
                "properties": {
                    "mode": "incremental",
                    "parameters": {
                        "vmName": {
                            "value": "[field('name')]"
                        },
                        "location": {
                            "value": "[field('location')]"
                        },
                        "configurationName": {
                            "value": "installed_application_linux"
                        },
                        "ApplicationName": {
                            "value": "[parameters('ApplicationName')]"
                        }
                    },
                    "template": {
                        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                        "contentVersion": "1.0.0.0",
                        "parameters": {
                            "vmName": {
                                "type": "string"
                            },
                            "location": {
                                "type": "string"
                            },
                            "configurationName": {
                                "type": "string"
                            },
                            "ApplicationName": {
                                "type": "string"
                            }
                        },
                        "resources": [
                            {
                                "apiVersion": "2018-11-20",
                                "type": "Microsoft.Compute/virtualMachines/providers/guestConfigurationAssignments",
                                "name": "[concat(parameters('vmName'), '/Microsoft.GuestConfiguration/', parameters('configurationName'))]",
                                "location": "[parameters('location')]",
                                "properties": {
                                    "guestConfiguration": {
                                        "name": "[parameters('configurationName')]",
                                        "version": "1.*",
                                        "configurationParameter": [
                                            {
                                                "name": "[ChefInSpec]InstalledApplicationLinuxResource1;AttributesYmlContent",
                                                "value": "[concat('packages: [', parameters('ApplicationName'), ']')]"
                                            }
                                        ]
                                    }
                                }
                            },
                            {
                                "apiVersion": "2017-03-30",
                                "type": "Microsoft.Compute/virtualMachines",
                                "identity": {
                                    "type": "SystemAssigned"
                                },
                                "name": "[parameters('vmName')]",
                                "location": "[parameters('location')]"
                            },
                            {
                                "apiVersion": "2015-05-01-preview",
                                "name": "[concat(parameters('vmName'), '/AzurePolicyforLinux')]",
                                "type": "Microsoft.Compute/virtualMachines/extensions",
                                "location": "[parameters('location')]",
                                "properties": {
                                    "publisher": "Microsoft.GuestConfiguration",
                                    "type": "ConfigurationforLinux",
                                    "typeHandlerVersion": "1.0",
                                    "autoUpgradeMinorVersion": true
                                },
                                "dependsOn": [
                                    "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'),'/providers/Microsoft.GuestConfiguration/guestConfigurationAssignments/',parameters('configurationName'))]"
                                ]
                            }
                        ]
                    }
                }
            }
        }
    }
}
```

deployIfNotExists 策略定义会定义曾用作策略验证依据的 Azure 图像  ：

|发布者 |产品/服务 |SKU |
|-|-|-|
|OpenLogic |CentOS\* |全部（版本 6 除外）\* |
|RedHat |RHEL |全部（版本 6 除外）\* |
|RedHat |osa | 全部 |
|credativ |Debian | 全部（版本 7 除外）\* |
|Suse |SLES\* |全部（版本 11 除外）\* |
|Canonical| UbuntuServer |全部（版本 12 除外）\* |
|microsoft-dsvm |linux-data-science-vm-ubuntu |全部 |
|microsoft-dsvm |azureml |全部 |
|cloudera |cloudera-centos-os |全部（版本 6 除外）\* |
|cloudera |cloudera-altus-centos-os |全部 |
|microsoft-ads |linux\* |全部 |
|microsoft-aks |全部 |全部 |
|AzureDatabricks |全部 |全部 |
|qubole-inc |全部 |全部 |
|datastax |全部 |全部 |
|couchbase |全部 |全部 |
|scalegrid |全部 |全部 |
|checkpoint |全部 |全部 |
|paloaltonetworks |全部 |全部 |

规则的“部署”部分会将 _installedApplication_ 参数传递给虚拟机上的来宾配置代理  。 借助此配置，代理可执行验证并通过 audit 策略定义回报符合性情况  。

## <a name="azure-portal"></a>Azure 门户

在门户中创建 audit 和 deployIfNotExists 定义后，建议将其分组到[计划](../concepts/definition-structure.md#initiatives)中以供分配   。

### <a name="create-copy-of-audit-definition"></a>创建 audit 定义的副本

[![将策略示例部署到 Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)

如果使用这些按钮通过门户进行部署，则会创建 audit 策略定义的副本  。
如果没有配对的 deployIfNotExists 策略定义，则将无法正常执行来宾配置  。

### <a name="create-copy-of-deployifnotexists-definition"></a>创建 deployIfNotExists 定义的副本

[![将策略示例部署到 Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)

如果使用这些按钮通过门户进行部署，则会创建 deployIfNotExists 策略定义的副本  。 如果没有配对的 audit 策略定义，则将无法正常执行来宾配置  。

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>使用 Azure PowerShell 部署

#### <a name="copy-and-assign-the-initiative"></a>复制并分配计划

下面的步骤将创建包含 audit 和 deployIfNotExists 的内置策略的计划副本，并将计划分配给资源组   。

```powershell
# Create the policy initiative (Subscription scope)
$initDef = New-AzPolicySetDefinition -Name 'guestconfig-installed-application-linux' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This initiative will both deploy the policy requirements and audit that the specified application is installed inside Linux virtual machines.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.definitions.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the initiative parameter (JSON format)
$initParam = '{ "applicationName": { "value": "python,powershell" } }'

# Create the initiative assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicySetDefinition $initDef -PolicyParameter $initParam -AssignIdentity -Location 'chinaeast2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

运行以下命令来删除以前的分配和定义：

```powershell
# Remove the initiative assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the initiative definition
Remove-AzPolicySetDefinition -Id $initDef
```

#### <a name="copy-and-assign-the-audit-definition"></a>复制并分配 audit 定义

以下步骤将创建 audit 定义的副本并将其分配给资源组  。 如果未一并分配配对的 deployIfNotExists 定义，则此定义将无法正常使用  。

```powershell
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-audit' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This policy audits that the specified application is installed inside Linux virtual machines. This policy should only be used along with its corresponding deploy policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-audit-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicyDefinition $definition
```

运行以下命令来删除以前的分配和定义：

```powershell
# Remove the policy definition
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

#### <a name="copy-and-assign-the-deployifnotexists-definition"></a>复制并分配 deployIfNotExists 定义

以下步骤将创建 deployIfNotExists 的副本并将其分配给资源组  。
如果未一并分配配对的 audit 定义，则此定义将无法正常使用  。

```powershell
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-deployIfNotExists' -DisplayName 'GuestConfig - Deploy VM extension to audit that an application is installed inside Linux VMs' -description 'Include this rule to deploy the VM extension for Microsoft Guest Configuration, the VM extension for Microsoft Azure Managed Service Identity, and the content required to audit that an application is installed inside Linux virtual machines. This policy should only be used along with its corresponding audit policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the definition parameter (JSON format)
$policyParam  = '{ "applicationName": { "value": "python,powershell" } }'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-deployIfNotExists-assignment' -DisplayName 'GuestConfig - Deploy VM extension to audit that Python and PowerShell are installed inside Linux VMs' -Scope $scope.ResourceID -PolicyDefinition $definition -PolicyParameter $policyParam -AssignIdentity -Location 'chinaeast2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

运行以下命令来删除以前的分配和定义：

```powershell
# Remove the policy assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell 说明

部署和删除脚本使用以下命令。 下表中的每条命令均链接到特定于命令的文档：

| 命令 | 注释 |
|---|---|
| [New-AzPolicySetDefinition](https://docs.microsoft.com/powershell/module/az.resources/New-AzPolicySetDefinition) | 创建 Azure Policy 计划。 |
| [New-AzPolicyDefinition](https://docs.microsoft.com/powershell/module/az.resources/New-AzPolicyDefinition) | 创建 Azure Policy 定义。 |
| [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroup) | 获取单个资源组。 |
| [New-AzPolicyAssignment](https://docs.microsoft.com/powershell/module/az.resources/New-AzPolicyAssignment) | 为计划或定义创建新的 Azure Policy 分配。 |
| [New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/New-AzRoleAssignment) | 向特定主体赋予现有角色。 |
| [Remove-AzPolicyAssignment](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzPolicyAssignment) | 删除现有的 Azure Policy 分配。 |
| [Remove-AzPolicySetDefinition](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzPolicySetDefinition) | 删除计划。 |
| [Remove-AzPolicyDefinition](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzPolicyDefinition) | 删除定义。 |

## <a name="next-steps"></a>后续步骤

- 查看其他 [Azure Policy 示例](index.md)。
- 详细了解 [Azure Policy 来宾配置](../concepts/guest-configuration.md)。
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)。
