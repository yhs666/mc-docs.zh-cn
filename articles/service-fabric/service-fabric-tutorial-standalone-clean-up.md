---
title: 教程：清理 Service Fabric 独立群集 - Azure Service Fabric | Azure
description: 本教程介绍了如何清理独立群集
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 05/11/2018
ms.date: 05/28/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 57584b442063f95e535886b13c7ee164c6994ad5
ms.sourcegitcommit: e50f668257c023ca59d7a1df9f1fe02a51757719
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2018
ms.locfileid: "34554682"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>教程：清理独立群集

作为 Service Fabric 采用的“任何 OS 任何云”方法的一部分，Service Fabric 独立群集允许你选择自己的环境并创建群集。 在本系列教程中，你将创建一个托管在 AWS 上的独立群集，并向其中安装应用程序。

本教程是一个系列中的第四部分， 本部分教程介绍了如何清理所创建的用于托管 Service Fabric 群集的 AWS 资源。

该系列的第 4 部分中介绍了如何：

> [!div class="checklist"]
> * 清理 Service Fabric 群集
> * 清理 AWS 资源

## <a name="clean-up-service-fabric-cluster"></a>清理 Service Fabric 群集

* 通过 RDP 连接到用于安装 Service Fabric 的 EC2 实例
* 打开 PowerShell
* 将目录更改到在第二个教程中提取的文件夹。
* 运行以下命令来删除 Service Fabric 群集：

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

* 出现提示时按 `Y`，如果成功，输出将如下所示，并在其中替换为你自己的 IP 地址：

```powershell
Best Practices Analyzer completed successfully.
Removing configuration from machine 172.31.21.141
Removing configuration from machine 172.31.27.1
Removing configuration from machine 172.31.20.163
Configuration removed from machine 172.31.21.141
Configuration removed from machine 172.31.27.1
Configuration removed from machine 172.31.20.163
The cluster is successfully removed.
```

## <a name="clean-up-aws-resources"></a>清理 AWS 资源

* 登录到 AWS 帐户
* 转到 EC2 控制台。
* 选择在本教程的第一部分中创建的三个节点。
* 单击“操作” > “实例状态” > “终止”

## <a name="next-steps"></a>后续步骤

本系列教程的第 4 部分介绍了如何清理在前面步骤中创建的资源。

> [!div class="checklist"]
> * 清理资源

> [!div class="nextstepaction"]
> [回到开头](service-fabric-tutorial-standalone-create-infrastructure.md)
<!-- Update_Description: new articles on service fabric tutorial standalone clean up -->
<!--ms.date: 05/28/2018-->