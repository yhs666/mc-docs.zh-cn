---
title: "为逻辑组织标记 Azure 资源 | Azure"
description: "演示如何应用标记来组织 Azure 资源进行计费和管理。"
services: azure-resource-manager
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: article
origin.date: 07/17/2017
ms.date: 08/21/2017
ms.author: v-yeche
ms.openlocfilehash: 798b8a551744931aea52cad313b4b39c528bfcfc
ms.sourcegitcommit: ece23dc9b4116d07cac4aaaa055290c660dc9dec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2017
---
# <a name="use-tags-to-organize-your-azure-resources"></a>使用标记整理 Azure 资源
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> 只能将标记应用到支持 Resource Manager 操作的资源。 如果通过经典部署模型（如通过经典管理门户）创建虚拟机、虚拟网络或存储，则无法向该资源应用标记。 若要支持标记，需通过 Resource Manager 重新部署这些资源。 所有其他资源均支持标记。
> 
> 

## <a name="ensure-tag-consistency-with-policies"></a>确保标记与策略一致

使用资源策略，可以为组织创建标准规则。 可以创建策略，以确保使用适当的值标记资源。
<!-- Not Available [Apply resource policies for tags](resource-manager-policy-tags.md) -->

## <a name="powershell"></a>PowerShell
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

若要查看**资源组**的现有标记，请使用：

```azurecli
az group show -n examplegroup --query tags
```

会返回以下格式：

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

若要查看**具有指定资源 ID 的资源**的现有标记，请使用：

```azurecli
az resource show --id {resource-id} --query tags
```

或者，若要查看**具有指定名称、类型的资源以及资源组**的现有标记，请使用：

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

若要获取具有特定标记的资源组，请使用 `az group list`。

```azurecli
az group list --tag Dept=IT
```

若要获取具有特定标记和值的所有资源，请使用 `az resource list`。

```azurecli
az resource list --tag Dept=Finance
```

每次将标记应用到资源或资源组时，都会覆盖该资源或资源组上的现有标记。 因此，必须根据该资源或资源组是否包含现有标记来使用不同的方法。 

若要将标记添加到**不包含现有标记的资源组**，请使用：

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

若要将标记添加到**不包含现有标记的资源**，请使用：

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

若要将标记添加到已包含标记的资源，请检索现有标记，重新格式化该值，并使用新标记重新应用这些标记： 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

要将资源组中的所有标记应用于其资源，并且 **不保留资源上的现有标记**，请使用以下脚本：

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    az resource tag --tags $t --id $resid
  done 
done
```

若要将资源组中的所有标记应用于其资源，并且**保留资源上的现有标记**，请使用以下脚本：

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done 
done
```

## <a name="templates"></a>模板

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>门户
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST API
门户和 PowerShell 在幕后都使用 [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) 。 如果需要将标记集成到另一个环境中，可以对资源 ID 使用 GET 以获取标记，并使用 PATCH 调用更新标记集。

## <a name="tags-and-billing"></a>标记和计费
使用标记可以对计费数据进行分组。 例如，如果要针对不同组织运行多个 VM，可以使用标记，对使用情况按成本中心进行分组。 还可以使用标记根据运行时环境对成本进行分类；例如，在生产环境中运行的虚拟机的计费使用情况。

可以下载使用情况逗号分隔值 (CSV) 文件。 可以从 [Azure 帐户门户](https://account.windowsazure.cn/)下载使用情况文件。 有关 REST API 操作，请参阅 [Azure 计费 REST API 参考](https://msdn.microsoft.com/zh-cn/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)。
<!-- Not supported billing-usage-rate-card-overview on Azure.cn -->

为支持标记和计费的服务下载使用情况 CSV 时，标记将显示在“标记”列中。 有关详细信息，请参阅[了解 Azure 帐单](../billing-understand-your-bill.md)。

![在计费中查看标记](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>后续步骤
* 可以使用自定义策略对订阅应用限制和约定。 定义的策略可能要求所有资源都拥有针对特定标记的值。 有关详细信息，请参阅 [使用策略来管理资源和控制访问](resource-manager-policy.md)。
* 有关部署资源时使用 Azure PowerShell 的说明，请参阅[将 Azure PowerShell 与 Azure 资源管理器配合使用](powershell-azure-resource-manager.md)。
* 有关部署资源时使用 Azure CLI 的说明，请参阅[将适用于 Mac、Linux 和 Windows 的 Azure CLI 与 Azure 资源管理配合使用](xplat-cli-azure-resource-manager.md)。
* 有关使用门户的说明，请参阅[使用 Azure 门户管理 Azure 资源](resource-group-portal.md)  
* 有关企业可如何使用 Resource Manager 有效管理订阅的指南，请参阅 [Azure 企业基架 - 出于合规目的监管订阅](resource-manager-subscription-governance.md)。

<!--Update_Description: wording update, add new sample code on Azure CLI-->