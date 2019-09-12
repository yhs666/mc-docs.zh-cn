---
title: 使用 RBAC 和 Azure 资源管理器模板管理对 Azure 资源的访问权限 | Microsoft Docs
description: 了解如何使用基于角色的访问控制 (RBAC) 和 Azure 资源管理器模板来管理用户、组和应用程序对 Azure 资源的访问权限。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 07/19/2019
ms.date: 09/04/2019
ms.author: v-junlch
ms.reviewer: bagovind
ms.openlocfilehash: 184d41537389b23462d5372c874f57660911ce34
ms.sourcegitcommit: 7fcf656522eec95d41e699cb257f41c003341f64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310836"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>使用 RBAC 和 Azure 资源管理器模板管理对 Azure 资源的访问权限

可以通过[基于角色的访问控制 (RBAC)](overview.md) 方式管理对 Azure 资源的访问权限。 除了使用 Azure PowerShell 或 Azure CLI 之外，还可以使用 RBAC 和 [Azure 资源管理器模板](../azure-resource-manager/resource-group-authoring-templates.md)管理对 Azure 资源的访问权限。 如果需要一致且重复地部署资源，模板会很有用。 本文介绍如何使用 RBAC 和模板管理访问权限。

## <a name="assign-role-to-resource-group-or-subscription"></a>将角色分配到资源组或订阅

在 RBAC 中，若要授予访问权限，请创建角色分配。 以下模板演示：
- 如何将角色分配到资源组或订阅范围内的用户、组或应用程序
- 如何将“所有者”、“参与者”和“读者”角色指定为参数

若要使用模板，必须指定以下输入：
- 要为其分配角色的用户、组或应用程序的唯一标识符
- 要分配的角色
- 用于角色分配的唯一标识符；你也可以使用默认的标识符

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "defaultValue": "[newGuid()]",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2018-09-01-preview",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]"
      }
    }
  ]
}
```

下面显示了在部署模板后向资源组用户分配“读取者”角色的示例。

![使用模板的角色分配](./media/role-assignments-template/role-assignment-template.png)

角色分配范围是根据部署级别确定的。 本文将会演示资源组和订阅级别的部署命令。

## <a name="assign-role-to-resource"></a>将角色分配到资源

如果需要在资源级别创建角色分配，角色分配的格式将会不同。 需提供要将角色分配到的资源的资源提供程序命名空间和资源类型。 还需在角色分配名称中包含该资源的名称。

对于角色分配的类型和名称，请使用以下格式：

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

以下模板将部署一个存储帐户并为其分配一个角色。 可以使用资源组命令来部署该模板。

```json
{
  "$schema": "https://schema.management.chinacloudapi.cn/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "defaultValue": "[newGuid()]",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    },
    "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[parameters('location')]",
      "sku": {
          "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    },
    {
      "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
      "apiVersion": "2018-09-01-preview",
      "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleNameGuid'))]",
      "dependsOn": [
          "[variables('storageName')]"
      ],
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]"
      }
    }
  ]
}
```

下面显示了部署该模板后，为存储帐户的用户分配“参与者”角色的示例。

![使用模板的角色分配](./media/role-assignments-template/role-assignment-template-resource.png)

## <a name="deploy-template-using-azure-powershell"></a>使用 Azure PowerShell 部署模板

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

若要使用 Azure PowerShell 将上述模板部署到资源组或订阅，请执行以下步骤。

1. 创建名为 rbac-rg.json 的新文件并复制以前的模板。

1. 登录到 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)。

1. 获取用户、组或应用程序的唯一标识符。 例如，可以使用 [Get-AzADUser](https://docs.microsoft.com/powershell/module/az.resources/get-azaduser) 命令列出 Azure AD 用户。

    ```azurepowershell
    $userid = (Get-AzADUser -DisplayName "{name}").id
    ```

1. 该模板将生成 GUID 的默认值用于标识角色分配。 如果需要指定特定的 GUID，请为 roleNameGuid 参数传入该值。 标识符的格式为：`11111111-1111-1111-1111-111111111111`

若要在资源或资源组级别分配角色，请执行以下步骤：

1. 创建示例资源组。

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "China North"
    ```

1. 使用 [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment) 命令启动部署。

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    ```

    下面显示了输出示例。

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

若要在订阅级别分配角色，请使用 [New-AzDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azdeployment) 命令并指定部署位置。

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
```

该命令的输出类似于资源组部署命令的输出。

## <a name="deploy-template-using-the-azure-cli"></a>使用 Azure CLI 部署模板

若要使用 Azure CLI 将上述模板部署到资源组或订阅，请执行以下步骤。

1. 创建名为 rbac-rg.json 的新文件并复制以前的模板。

1. 登录 [Azure CLI](/cli/authenticate-azure-cli)。

1. 获取用户、组或应用程序的唯一标识符。 例如，可以使用 [az ad user show](/cli/ad/user#az-ad-user-show) 命令显示 Azure AD 用户。

    ```azurecli
    userid=$(az ad user show --upn-or-object-id "{email}" --query objectId --output tsv)
    ```

1. 该模板将生成 GUID 的默认值用于标识角色分配。 如果需要指定特定的 GUID，请为 roleNameGuid 参数传入该值。 标识符的格式为：`11111111-1111-1111-1111-111111111111`

若要在资源或资源组级别分配角色，请执行以下步骤：

1. 创建示例资源组。

    ```azurecli
    az group create --name ExampleGroup --location "China North"
    ```

1. 使用 [az group deployment create](/cli/group/deployment#az-group-deployment-create) 命令启动部署。

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    ```

    下面显示了输出示例。

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```

若要在订阅级别分配角色，请使用 [az deployment create](/cli/deployment#az-deployment-create) 命令并指定部署位置。

```azurecli
az deployment create --location centralus --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
```

该命令的输出类似于资源组部署命令的输出。

## <a name="next-steps"></a>后续步骤

- [快速入门：使用 Azure 门户创建和部署 Azure 资源管理器模板](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [了解 Azure 资源管理器模板的结构和语法](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure 快速启动模板](https://azure.microsoft.com/resources/templates/?term=rbac)

