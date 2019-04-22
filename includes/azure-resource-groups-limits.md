---
author: rockboyfor
ms.service: billing
ms.topic: include
origin.date: 11/09/2018
ms.date: 04/15/2019
ms.author: v-yeche
ms.openlocfilehash: 80d1eca24610b77f6d4fd2ec6e414a2146c92df8
ms.sourcegitcommit: 9f7a4bec190376815fa21167d90820b423da87e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "59532417"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[资源组](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)的资源数（按资源类型） |800 |因资源类型而有所不同 |
| 部署历史记录中每个资源组的部署数 |800 |800 |
| 每个部署的资源数 |800 |800 |
| 管理锁数（按唯一的作用域） |20 个 |20 个 |
| 标记数（按资源或资源组） |15 |15 |
| 标记键长度 |512 |512 |
| 标记值长度 |256 |256 |

#### <a name="template-limits"></a>模板限制

| 值 | 默认限制 | 最大限制 |
| --- | --- | --- |
| parameters |256 |256 |
| 变量 |256 |256 |
| 资源（包括副本计数） |800 |800 |
| Outputs |64 |64 |
| 模板表达式 |24,576 个字符 |24,576 个字符 |
| 已导出模板中的资源 |200 |200 | 
| 模板大小 |1 MB |1 MB |
| 参数文件大小 |64 KB |64 KB |

通过使用嵌套模板，可超出某些模板限制。 有关详细信息，请参阅[部署 Azure 资源时使用链接的模板](../articles/azure-resource-manager/resource-group-linked-templates.md)。 若要减少参数、变量或输出的数量，可以将几个值合并为一个对象。 

<!-- Not Available on Microsoft Azure content  [Objects as parameters](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).-->

如果达到每个资源组的部署数限制 800，则会从历史记录中删除不再需要的部署。 可以使用 Azure CLI 的 [az group deployment delete](https://docs.azure.cn/zh-cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-delete) 删除历史记录中的条目。 也可使用 PowerShell 中的 [Remove-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroupdeployment)。 从部署历史记录中删除条目不会影响部署资源。