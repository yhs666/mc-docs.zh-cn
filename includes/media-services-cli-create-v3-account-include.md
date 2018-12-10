---
title: include 文件
description: include 文件
services: media-services
author: WenJason
ms.service: media-services
ms.topic: include
origin.date: 04/13/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.custom: include file
ms.openlocfilehash: cf155a3a17e5c20f8af609cc0c5922cca0c4fe1e
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647560"
---
## <a name="create-a-media-services-account"></a>创建媒体服务帐户

首先需创建媒体服务帐户。 本部分介绍需要些什么才能使用 CLI 2.0 创建帐户。

### <a name="create-a-resource-group"></a>创建资源组

使用以下命令创建资源组。 Azure 资源组是在其中部署和管理资源（例如 Azure 媒体服务帐户和关联的存储帐户）的逻辑容器。

```cli
az group create --name amsResourceGroup --location chinaeast
```

### <a name="create-a-storage-account"></a>创建存储帐户

创建媒体服务帐户时，需要提供 Azure 存储帐户资源的名称。 指定存储帐户会附加到媒体服务帐户。 

必须具有一个主存储帐户，并且可以拥有任意数量的与媒体服务帐户关联的辅助存储帐户。 媒体服务支持常规用途 v2 (GPv2) 或常规用途 v1 (GPv1) 帐户。 不允许将仅限 Blob **的帐户作为主帐户**。 若要了解存储帐户的详细信息，请参阅 [Azure 存储帐户选项](../articles/storage/common/storage-account-options.md)。 

若要详细了解如何在媒体服务中使用存储帐户，请参阅[存储帐户](../articles/media-services/latest/storage-account-concept.md)。

以下命令创建将与媒体服务帐户相关联的存储帐户。 在以下脚本中，可以将 `storageaccountforams` 替换为你的值。 帐户名称的长度必须小于 24。

```cli
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>创建媒体服务帐户

以下 Azure CLI 命令创建新的媒体服务帐户。 可以替换以下值：`amsaccount`  `storageaccountforams`（必须与提供给存储帐户的值相符）和 `amsResourceGroup`（必须与提供给资源组的值相符）。

```cli
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
