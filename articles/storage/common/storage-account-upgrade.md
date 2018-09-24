---
title: 升级到常规用途 v2 存储帐户- Azure 存储 | Microsoft Docs
description: 升级到常规用途 v2 存储帐户。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 09/11/2018
ms.date: 09/24/2018
ms.author: v-jay
ms.openlocfilehash: 2c163ab2229659f3d1674cccce1d846ae74cf299
ms.sourcegitcommit: 0081fb238c35581bb527bdd704008c07079c8fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523744"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>升级到常规用途 v2 存储帐户

常规用途 v2 存储帐户支持最新的 Azure 存储功能，并纳入了常规用途 v1 存储帐户和 Blob 存储帐户的所有功能。 建议将常规用途 v2 帐户用于大多数存储方案。 常规用途 v2 帐户提供适用于 Azure 存储的最低每 GB 容量价格，以及具有行业竞争力的事务价格。

从常规用途 v1 帐户或 Blob 存储帐户升级到常规用途 v2 存储帐户的过程很简单。 可以使用 Azure 门户、PowerShell 或 Azure CLI 升级。 升级帐户的操作不可逆，且可能产生费用。

## <a name="upgrade-using-the-azure-portal"></a>使用 Azure 门户升级

1. 登录到 [Azure 门户](https://portal.azure.cn)。
2. 导航到存储帐户。
3. 在“设置”部分单击“配置”。
4. 在“帐户类型”下单击“升级”。
5. 在“确认升级”下键入帐户名称。 
6. 单击边栏选项卡底部的“升级”。

## <a name="upgrade-with-powershell"></a>使用 PowerShell 进行升级

若要使用 PowerShell 将常规用途 v1 帐户升级为常规用途 v2 帐户，请先更新 PowerShell，以便使用最新版本的 **AzureRm.Storage** 模块。 请参阅[安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)，了解如何安装 PowerShell。 

接下来，调用以下命令来升级帐户（请使用自己的资源组和存储帐户的名称来代替相应项）：

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>使用 Azure CLI 进行升级

若要使用 Azure CLI 将常规用途 v1 帐户升级为常规用途 v2 帐户，请先安装最新版的 Azure CLI。 请参阅 [Install the Azure CLI 2.0](/cli/install-azure-cli?view=azure-cli-latest)（安装 Azure CLI 2.0），了解如何安装 CLI。 

接下来，调用以下命令来升级帐户（请使用自己的资源组和存储帐户的名称来代替相应项）：

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
``` 

## <a name="specify-an-access-tier-for-blob-data"></a>指定 Blob 数据的访问层

常规用途 v2 帐户支持所有 Azure 存储服务和数据对象，但访问层仅适用于 Blob 存储中的块 Blob。 升级到常规用途 v2 存储帐户时，可以指定 Blob 数据的访问层。 

使用访问层可以根据预期使用模式选择最具经济效益的存储。 块 Blob 可以存储在热层或冷层中。 有关访问层的详细信息，请参阅 [Azure Blob 存储：热、冷和存档存储层](../blobs/storage-blob-storage-tiers.md)。

默认情况下，新存储帐户在热访问层中创建，常规用途 v1 存储帐户将升级到热访问层。 如果你正在探讨要将哪个访问层用于升级后的数据，请考虑具体的场景。 有两种典型的用户场景适合迁移到常规用途 v2 帐户：

* 你已有一个常规用途 v1 存储帐户，并想要使用 Blob 数据的适当存储层来评估对常规用途 v2 存储帐户所做的更改。
* 你决定使用常规用途 v2 存储帐户，或者已有一个此类帐户，但想要评估一下是要对 Blob 数据使用热存储层还是冷存储层。

在这两种情况下，首要任务都是估算对存储在常规用途 v2 存储帐户中的数据进行存储、访问和操作所需的成本，并将该成本与当前成本进行比较。

### <a name="estimate-costs-for-your-current-usage-patterns"></a>估算当前使用模式的成本

若要估算在特定层的常规用途 v2 存储帐户中存储和访问 Blob 数据所需的成本，请评估现有的使用模式，或对预期的使用模式进行大致的估计。 一般情况下，需了解：

* Blob 存储消耗量（以 GB 为单位），包括：
    - 有多少数据存储在存储帐户中？
    - 数据量每月如何变化；新数据是否不断替换旧数据？
* Blob 存储数据的主要访问模式，包括：
    - 要从存储帐户读取多少数据，向其写入了多少数据？ 
    - 针对存储帐户中的数据执行多少次读取和写入操作？

若要确定最符合需求的访问层，确定 Blob 数据当前使用的容量以及当前使用了其中的多少数据，可能会有所帮助。 

若要收集迁移之前存储帐户的用量数据，可以使用 [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) 监视存储帐户。 Azure Monitor 会执行日志记录，并提供 Azure 服务（包括 Azure 存储）的指标数据。 

若要监视存储帐户中 Blob 的消耗量数据，请在 Azure Monitor 中启用容量指标。 容量指标会记录有关帐户中 Blob 每日使用的存储量的数据。 可以使用容量指标来估算在存储帐户中存储数据所需的成本。 若要了解每种帐户类型的 Blob 存储容量定价方式，请参阅[块 Blob 定价](https://azure.cn/pricing/details/storage/blobs/)。

若要监视 Blob 存储的数据访问模式，请在 Azure Monitor 中启用事务指标。 可以根据不同的 Azure 存储操作进行筛选，以估算每种操作的调用频率。 若要了解每种帐户类型的块 Blob 和追加 Blob 的不同事务定价方式，请参阅[块 Blob 定价](https://azure.cn/pricing/details/storage/blobs/)。  

有关从 Azure Monitor 收集指标的详细信息，请参阅 [Azure Monitor 中的 Azure 存储指标](storage-metrics-in-azure-monitor.md)。

## <a name="next-steps"></a>后续步骤

- [创建存储帐户](storage-quickstart-create-account.md)
- [管理 Azure 存储帐户](storage-account-manage.md)