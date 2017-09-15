---
title: "Azure CLI 脚本示例 - 将应用程序部署到群集"
description: "Azure CLI 脚本示例 - 将应用程序部署到 Service Fabric 群集。"
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
ms.openlocfilehash: 38acaf7c99bf259aa377bcd6aa19c0bff91bb3bc
ms.sourcegitcommit: 76a57f29b1d48d22bb4df7346722a96c5e2c9458
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>将应用程序部署到 Service Fabric 群集

此示例脚本将应用程序包复制到群集映像存储区，在群集中注册应用程序类型，并根据应用程序类型创建应用程序实例。  如果目标应用程序类型的应用程序清单中定义了任何默认服务，则此时还会创建这些服务。

根据需要安装 [Azure CLI](../service-fabric-azure-cli-2-0.md)。

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth2.chinacloudapp.cn:19080

# Upload the application files to the image store
# (note the last folder name, Debug in this example)
sfctl application upload \
    --path  C:\Code\svcfab-vs\svcfab-vs\pkg\Debug \
    --show-progress

# Register the application (manifest files) from the image store
# (Note the last folder from the previous command is used: Debug)
sfctl application provision \
    --application-type-build-path Debug \
    --timeout 500

# Create an instance of the registered application and 
# auto deploy any defined services
sfctl application create \
    --app-name fabric:/MyApp \
    --app-type MyAppType \
    --app-version 1.0.0

```

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用[删除应用程序](cli-remove-application.md)中的脚本删除应用程序实例，注销应用程序类型，并从映像存储区中删除应用程序包。

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [sf cluster select](https://docs.microsoft.com/cli/azure/sf/cluster#select) | 选择要使用的群集。 |
| [sf application upload](https://docs.microsoft.com/cli/azure/sf/application#upload) | 上传应用文件和清单。 |
| [sf application provision](https://docs.microsoft.com/cli/azure/sf/application#provision) | 在群集上注册应用程序。|
| [sf application create](https://docs.microsoft.com/cli/azure/sf/application#create) | 创建应用程序实例并将所有已定义的服务部署到节点。 |

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [Azure CLI 文档](../service-fabric-azure-cli-2-0.md)。

在 [Azure CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Azure CLI 示例。

<!--Update_Description: new articles of deploy application with CLI in service fabric -->