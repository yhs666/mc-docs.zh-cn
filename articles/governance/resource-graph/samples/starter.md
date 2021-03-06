---
title: 初学者查询示例
description: 使用 Azure 资源图来运行一些初学者查询，包括计算资源数、对资源进行排序或按特定标记排序。
author: DCtheGeek
ms.author: v-yiso
origin.date: 11/21/2019
ms.date: 12/16/2019
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 8dde615b8c9ec29a16bba46b8f5a9879466abf88
ms.sourcegitcommit: cf73284534772acbe7a0b985a86a0202bfcc109e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884854"
---
# <a name="starter-resource-graph-query-samples"></a>初学者 Resource Graph 查询示例

了解使用 Azure 资源图表进行查询的第一步是对[查询语言](../concepts/query-language.md)有基本的了解。 如果还不熟悉 [Azure 数据资源管理器](../../../data-explorer/data-explorer-overview.md)，建议查看基础知识，以了解如何撰写所需资源的请求。

我们将逐步介绍以下初学者查询：

> [!div class="checklist"]
> - [对 Azure 资源进行计数](#count-resources)
> - [统计密钥保管库资源](#count-keyvaults)
> - [列出按名称排序的资源](#list-resources)
> - [按降序显示按名称排序的所有虚拟机](#show-vms)
> - [按名称及其 OS 类型显示前五个虚拟机](#show-sorted)
> - [按 OS 类型对虚拟机进行计数](#count-os)
> - [显示包含存储的资源](#show-storage)
> - [列出所有公共 IP 地址](#list-publicip)
> - [对具有由订阅配置的 IP 地址的资源进行计数](#count-resources-by-ip)
> - [列出具有特定标记值的资源](#list-tag)
> - [列出具有特定标记值的所有存储帐户](#list-specific-tag)
> - [显示虚拟机资源的别名](#show-aliases)
> - [显示特定别名的非重复值](#distinct-alias-values)
> - [显示未关联的网络安全组](#unassociated-nsgs)

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="language-support"></a>语言支持

Azure CLI（通过扩展）和 Azure PowerShell（通过模块）支持 Azure 资源图表。 在运行以下任何查询之前，请检查环境是否已准备就绪。 有关安装和验证所选 shell 环境的步骤，请参阅 [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) 和 [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module)。

## <a name="a-namecount-resourcescount-azure-resources"></a><a name="count-resources"/> 对 Azure 资源进行计数

此查询返回有权访问的订阅中存在的 Azure 资源的数量。 这是一个良好查询，用于验证所选 shell 是否已安装适当的 Azure 资源图表组件并处于正常工作状态。

```kusto
Resources
| summarize count()
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | summarize count()"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | summarize count()"
```

## <a name="a-namecount-keyvaultscount-key-vault-resources"></a><a name="count-keyvaults">统计密钥保管库资源

此查询使用 `count` 而不是 `summarize` 来计算返回的记录数。 只有密钥保管库才包括在计数中。

```kusto
Resources
| where type =~ 'microsoft.keyvault/vaults'
| count
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

## <a name="a-namelist-resourceslist-resources-sorted-by-name"></a><a name="list-resources"/> 列出按名称排序的资源

此查询返回任意类型的资源，但只返回“名称”、“类型”和“位置”属性。    它使用 `order by`  以升序 (`asc`) 按“名称”属性对属性排序。

```kusto
Resources
| project name, type, location
| order by name asc
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | project name, type, location | order by name asc"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | project name, type, location | order by name asc"
```

## <a name="a-nameshow-vmsshow-all-virtual-machines-ordered-by-name-in-descending-order"></a><a name="show-vms"/> 按降序显示按名称排序的所有虚拟机

若要只列出虚拟机（类型为 `Microsoft.Compute/virtualMachines`），我们可在结果中匹配属性“类型”  。 与上一查询类似，`desc` 将 `order by` 更改为降序。 类型匹配中的 `=~` 告知资源图表不区分大小写。

```kusto
Resources
| project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="a-nameshow-sortedshow-first-five-virtual-machines-by-name-and-their-os-type"></a><a name="show-sorted"/> 按名称及其 OS 类型显示前五个虚拟机

此查询将使用 `top` 仅检索按名称排序的五条匹配记录。 Azure 资源的类型为 `Microsoft.Compute/virtualMachines`。 `project` 告诉 Azure 资源图表要包含哪些属性。

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="a-namecount-oscount-virtual-machines-by-os-type"></a><a name="count-os"/> 按 OS 类型对虚拟机进行计数

基于前面的查询，我们仍受限于类型 `Microsoft.Compute/virtualMachines` 的 Azure 资源，但不再限制返回的记录数量。
相反，我们使用 `summarize` 和 `count()` 来定义如何按属性对值进行分组和聚合，在此示例中为 `properties.storageProfile.osDisk.osType`。 有关此字符串在完整对象中的外观示例，请参阅[浏览资源 - 虚拟机发现](../concepts/explore-resources.md#virtual-machine-discovery)。

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

编写相同查询的另一种方法是 `extend` 属性，并赋予其临时名称，以供查询使用，在本例中为 os  。 os 然后由 `summarize` 和 `count()` 使用，如上例所示  。

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> 请注意，虽然 `=~` 允许不区分大小写的匹配，但在查询中使用属性（例如 properties.storageProfile.osDisk.osType）要求大小写正确  。 如果属性的大小写不正确，它仍可返回值，但分组或汇总可能不正确。

## <a name="a-nameshow-storageshow-resources-that-contain-storage"></a><a name="show-storage"/> 显示包含存储的资源

此示例查询不显式定义要匹配的类型，而是查找 `contains` 单词“存储”的任何 Azure 资源  。

```kusto
Resources
| where type contains 'storage' | distinct type
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type contains 'storage' | distinct type"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type contains 'storage' | distinct type"
```

## <a name="a-namelist-publiciplist-all-public-ip-addresses"></a><a name="list-publicip"/> 列出所有公共 IP 地址

与上一查询类似，查找包含单词“publicIPAddresses”的所有类型  。
此查询扩展了该模式，以仅包括 properties.ipAddress 为
`isnotempty` 的结果，仅返回 properties.ipAddress，并将结果`limit`为前 100 名  
100. 根据所选 shell，可能需要转义引号。

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
| limit 100
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

## <a name="a-namecount-resources-by-ipcount-resources-that-have-ip-addresses-configured-by-subscription"></a><a name="count-resources-by-ip"/> 对具有由订阅配置的 IP 地址的资源进行计数

使用前面的示例查询并添加 `summarize` 和 `count()`，我们可通过订阅配置了 IP 地址的资源来获取列表。

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| summarize count () by subscriptionId
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

## <a name="a-namelist-taglist-resources-with-a-specific-tag-value"></a><a name="list-tag"/> 列出具有特定标记值的资源

我们可通过 Azure 资源类型以外的属性（如标记）来限制结果。 在此示例中，我们正在筛选 Azure 资源，其标记名为“环境”，其值为 Internal   。

```kusto
Resources
| where tags.environment=~'internal'
| project name
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where tags.environment=~'internal' | project name"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name"
```

如果还要提供资源具有的标记及其值，请将属性“标记”添加到 `project` 关键字  。

```kusto
Resources
| where tags.environment=~'internal'
| project name, tags
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name, tags"
```

## <a name="a-namelist-specific-taglist-all-storage-accounts-with-specific-tag-value"></a><a name="list-specific-tag"/> 列出具有特定标记值的所有存储帐户

组合前面示例的筛选功能，按“类型”属性筛选 Azure 资源类型  。 此查询还使用特定的标记名称和值来限制对 Azure 资源特定类型的搜索。

```kusto
Resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> 此示例使用 `==` 进行匹配，而不是使用 `=~` 条件。 `==` 是区分大小写的匹配项。

## <a name="a-nameshow-aliasesshow-aliases-for-a-virtual-machine-resource"></a><a name="show-aliases"/> 显示虚拟机资源的别名

Azure Policy 使用 [Azure Policy 别名](../../policy/concepts/definition-structure.md#aliases)管理资源符合性。 Azure Resource Graph 可以返回资源类型的别名  。 创建自定义策略定义后，这些值可用于比较别名的当前值。 默认情况下，查询结果中不提供别名数组  。 使用 `project aliases` 将其显式添加到结果中。

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
| project aliases
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)
```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases" | ConvertTo-Json
```

## <a name="a-namedistinct-alias-valuesshow-distinct-values-for-a-specific-alias"></a><a name="distinct-alias-values"/> 显示特定别名的非重复值

在单个资源上查看别名的值，这非常有用，但这不会显示使用 Azure Resource Graph 在订阅中进行查询的真实值。 本示例查看特定别名的所有值，并返回不同的值。

```kusto
Resources
| where type=~'Microsoft.Compute/virtualMachines'
| extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType']
| distinct tostring(alias)
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az graph query -q "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
Search-AzGraph -Query "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

## <a name="a-nameunassociated-nsgsshow-unassociated-network-security-groups"></a><a name="unassociated-nsgs"/>显示未关联的网络安全组

此查询返回未与网络接口或子网关联的网络安全组 (NSG)。

```kusto
Resources
| where type =~ "microsoft.network/networksecuritygroups" and isnull(properties.networkInterfaces) and isnull(properties.subnets)
| project name, resourceGroup
| sort by name asc
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az graph query -q "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
Search-AzGraph -Query "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

## <a name="next-steps"></a>后续步骤

- 详细了解[查询语言](../concepts/query-language.md)。
- 详细了解如何[浏览资源](../concepts/explore-resources.md)。
- 查看[高级查询](advanced.md)的示例。