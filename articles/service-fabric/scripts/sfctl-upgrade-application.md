---
title: Service Fabric CLI 脚本示例 - 更新群集上的应用程序
description: Service Fabric CLI 脚本示例 - 使用新版本更新应用程序。 此示例还使用新位升级已部署的应用程序。
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
origin.date: 12/06/2017
ms.date: 08/26/2019
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: 79eb0bac2d1e1afa88799e686fd13e167be21c7e
ms.sourcegitcommit: ba87706b611c3fa338bf531ae56b5e68f1dd0cde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2019
ms.locfileid: "70174105"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>将应用程序证书添加到 Service Fabric 群集

此示例脚本会上传现有应用程序的新版本，然后使用新位升级已部署的应用程序。

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth2.cloudapp.chinacloudapi.cn:19080

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

<!--Update_Description: update meta properties -->