---
author: WenJason
ms.service: databox
ms.subservice: heavy
ms.topic: include
origin.date: 06/18/2019
ms.date: 07/22/2019
ms.author: v-jay
ms.openlocfilehash: 65ab77b0f74793fbe0e2a7f7b085c4bca41c6cda
ms.sourcegitcommit: 98cc8aa5b8d0e04cd4818b34f5350c72f617a225
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2019
ms.locfileid: "68298150"
---
下面是对复制到存储帐户的数据的大小限制。 请确保上传的数据符合这些限制。 有关这些限制的最新信息，请转到 [Azure blob 存储规模目标](/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets)和 [Azure 文件规模目标](/storage/common/storage-scalability-targets#azure-files-scale-targets)。

| 复制到 Azure 存储帐户的数据的大小                      | 默认限制          |
|---------------------------------------------------------------------|------------------------|
| 块 Blob 和页 blob                                            | 每个存储帐户 500 TiB。 <br> 这包括来自所有源（包括 Data Box）的数据。|
| Azure 文件                                                          | 每个共享 5 TiB。<br> StorageAccount_AzureFiles 下的所有文件夹都须遵循此限制  。       |
