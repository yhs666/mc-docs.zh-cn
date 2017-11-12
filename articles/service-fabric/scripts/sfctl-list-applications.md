---
title: "Service Fabric CLI 脚本示例 - 列出群集上的应用程序"
description: "Service Fabric CLI 脚本示例 - 列出 Service Fabric 群集上预配的应用程序。"
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
origin.date: 09/19/2017
ms.date: 11/13/2017
ms.author: v-yeche
ms.custom: 
ms.openlocfilehash: 564dfaacb0f811d049079a0a844d2b4d60b47370
ms.sourcegitcommit: 530b78461fda7f0803c27c3e6cb3654975bd3c45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>将应用程序证书添加到 Service Fabric 群集

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
<!--Update_Description: new articles on list application with sfctl-->