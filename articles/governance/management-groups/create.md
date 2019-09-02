---
title: 创建管理组以组织 Azure 资源 - Azure 治理
description: 了解如何使用门户、Azure PowerShell 和 Azure CLI 创建 Azure 管理组以管理多个资源。
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 10/10/2018
ms.date: 04/22/2019
ms.author: v-biyu
ms.topic: conceptual
ms.openlocfilehash: b7dd99c30e9f53b5abacc3e50de14f9a81277a1d
ms.sourcegitcommit: 5a7034098baffcc7979769b13790c1b487f073b0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471973"
---
<!--Verify successfully-->
# <a name="create-management-groups-for-resource-organization-and-management"></a>创建用来组织和管理资源的管理组

管理组是一些容器，可以帮助你跨多个订阅管理访问权限、策略和符合性。 可以创建这些容器来构建可以与 [Azure Policy](../policy/overview.md) 和 [Azure 基于角色的访问控制](../../role-based-access-control/overview.md)配合使用的有效且高效的层次结构。 若要详细了解管理组，请参阅[使用 Azure 管理组整理资源](index.md)。

在目录中创建的第一个管理组可能需要最多 15 分钟才能完成。 一些进程会首次运行以在 Azure 中为目录设置管理组服务。 在进程完成后将显示通知。

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="create-a-management-group"></a>创建管理组

可以使用门户、PowerShell 或 Azure CLI 创建管理组。 目前无法使用资源管理器模板创建管理组。

### <a name="create-in-portal"></a>在门户中创建

1. 登录到 [Azure 门户](http://portal.azure.cn)。

1. 选择“所有服务” > “管理组”。

1. 在主页上，选择“新建管理组”。

   ![管理组操作页](./media/main.png)

1. 填写管理组 ID 字段。

   - “管理组 ID”是用来在此管理组上提交命令的目录唯一标识符。 此标识符在创建后不可编辑，因为它用来在整个 Azure 系统中标识此组。
   - 显示名称字段是在 Azure 门户中显示的名称。 创建管理组时，单独的显示名称是一个可选字段，并且可以随时更改。  

   ![用于创建新管理组的“选项”窗格](./media/create_context_menu.png)  

1. 选择“其他安全性验证” 。

### <a name="create-in-powershell"></a>在 PowerShell 中创建

在 PowerShell 中，使用 [New-AzManagementGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/new-azmanagementgroup) cmdlet 创建新的管理组。

```azurepowershell
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** 是要创建的唯一标识符。 此 ID 由其他命令用来引用此组，并且以后无法更改。

如果希望管理组在 Azure 门户中显示一个不同的名称，请添加 **DisplayName** 参数。 例如，若要创建 GroupName 为 Contoso 且显示名称为“Contoso Group”的管理组，请使用以下 cmdlet：

```azurepowershell
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

在上述示例中，新的管理组是在根管理组下创建的。 若要指定一个不同的管理组作为父级，请使用 **ParentId** 参数。

```azurepowershell
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>在 Azure CLI 中创建

在 Azure CLI 中，使用 [az account management-group create](/cli/account/management-group?view=azure-cli-latest#az-account-management-group-create) 命令创建新的管理组。

```azurecli
az account management-group create --name Contoso
```

**name** 是要创建的唯一标识符。 此 ID 由其他命令用来引用此组，并且以后无法更改。

如果希望管理组在 Azure 门户中显示一个不同的名称，请添加 **display-name** 参数。 例如，若要创建 GroupName 为 Contoso 且显示名称为“Contoso Group”的管理组，请使用以下命令：

```azurecli
az account management-group create --name Contoso --display-name 'Contoso Group'
```

在上述示例中，新的管理组是在根管理组下创建的。 若要指定一个不同的管理组作为父级，请使用 **parent** 参数并提供父组的名称。

```azurecli
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>后续步骤

若要了解有关管理组的详细信息，请参阅：

- [创建用于整理 Azure 资源的管理组](create.md)
- [如何更改、删除或管理管理组](manage.md)
- [在 Azure PowerShell 资源模块中查看管理组](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/?view=azurermps-6.13.0&viewFallbackFrom=azurermps-6.12.0#resources)
- [在 REST API 中查看管理组](https://docs.microsoft.com/en-us/rest/api/resources/managementgroups)
- [在 Azure CLI 中查看管理组](https://docs.azure.cn/zh-cn/cli/account/management-group?view=azure-cli-latest)
