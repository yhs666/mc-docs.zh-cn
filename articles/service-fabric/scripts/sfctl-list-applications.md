---
title: Service Fabric CLI 脚本示例 - 列出群集上的应用程序
description: Service Fabric CLI 脚本示例 - 列出 Service Fabric 群集上预配的应用程序。
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
origin.date: 04/13/2018
ms.date: 08/26/2019
ms.author: v-yeche
ms.custom: ''
ms.openlocfilehash: c263663229efd3fd14a94b18a2ea5557d59e9db7
ms.sourcegitcommit: ba87706b611c3fa338bf531ae56b5e68f1dd0cde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2019
ms.locfileid: "70174115"
---
# <a name="list-applications-running-in-a-service-fabric-cluster"></a>列出在 Service Fabric 群集中运行的应用程序

此脚本示例将连接到 Service Fabric 群集并列出所有预配的应用程序。

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth2.cloudapp.chinacloudapi.cn:19080

# Retrieve all applications from the cluster
sfctl application list
```

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [Service Fabric CLI 文档](../service-fabric-cli.md)。

在 [Service Fabric CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Service Fabric CLI 示例。

<!--Update_Description: update meta properties -->