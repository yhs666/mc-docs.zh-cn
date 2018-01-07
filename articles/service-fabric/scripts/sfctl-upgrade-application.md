---
title: "Service Fabric CLI 脚本示例 - 更新群集上的应用程序"
description: "Service Fabric CLI 脚本示例 - 使用新版本更新应用程序。 此示例还使用新位升级已部署的应用程序。"
services: service-fabric
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: 
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
origin.date: 12/06/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.custom: 
ms.openlocfilehash: 6879be515ad13cf2f28fcbb6945c739d664f925b
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>将应用程序证书添加到 Service Fabric 群集

此示例脚本会上传现有应用程序的新版本，然后使用新位升级已部署的应用程序。

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth.cloudapp.chinacloudapi.cn:19080

# Upload the latest bits of an application
sfctl application upload --path ~/app_package_dir_2

# Provision the new application
sfctl application provision --application-type-build-path app_package_dir_2

# Upgrade an existing up with the new version
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [Service Fabric CLI 文档](../service-fabric-cli.md)。

在 [Service Fabric CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Service Fabric CLI 示例。
<!--Update_Description: update meta properties, wording update -->