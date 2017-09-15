---
title: "Azure CLI 脚本示例 - 从群集中删除应用程序"
description: "Azure CLI 脚本示例 - 从 Service Fabric 群集中删除应用程序。"
services: service-fabric
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
origin.date: 07/21/2017
ms.date: 09/11/2017
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: df9d4005e50ce77810d4d25bce47d937a864d7b9
ms.sourcegitcommit: 76a57f29b1d48d22bb4df7346722a96c5e2c9458
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>从 Service Fabric 群集中删除应用程序

此示例脚本会删除正在运行的 Service Fabric 应用程序实例，并从群集中注销应用程序类型和版本。  删除应用程序实例时也会删除与该应用程序关联的所有正在运行的服务实例。 接下来，会从映像存储区中删除应用程序文件。 

根据需要安装 [Azure CLI](../service-fabric-azure-cli-2-0.md)。

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth2.cloudapp.chinacloudapi.cn:19080

# Delete the application
sfctl application delete \
    --application-id svcfab_app \
    --timeout 500

# Unprovision the application type
sfctl application unprovision \
    --application-type-name svcfab_appType \
    --application-type-version 1.0.0 \
    --timeout 500

# Delete the application files from the image store
sfctl store delete \
    --content-path myappfolder
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [sf cluster select](https://docs.microsoft.com/cli/azure/sf/cluster#select) | 选择要使用的群集。 |
| [sf application delete](https://docs.microsoft.com/cli/azure/sf/application#delete) | 从群集中删除应用程序实例。 |
| [sf application unprovision](https://docs.microsoft.com/cli/azure/sf/application#unprovision) | 从群集中注销 Service Fabric 应用程序类型和版本。|
| [sf application package-delete](https://docs.microsoft.com/cli/azure/sf/application#package-delete) | 从映像存储区中删除 Service Fabric 应用程序包。 |

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [Azure CLI 文档](../service-fabric-azure-cli-2-0.md)。

在 [Azure CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Azure CLI 示例。

<!--Update_Description: new articles of remove application with CLI in service fabric -->