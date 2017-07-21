---
title: "适用于 Windows Server 的 Azure Service Fabric 独立包 | Azure"
description: "适用于 Windows Server 的 Azure Service Fabric 独立包的说明和内容。"
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/10/2017
ms.author: v-johch
ms.openlocfilehash: 84cb5baec1fc1be1df62d53f2d5e263182d3e8b3
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>适用于 Windows Server 的 Service Fabric 独立包的内容
在 [已下载](http://go.microsoft.com/fwlink/?LinkId=730690) 的 Service Fabric 独立包中，找到以下文件：

| **文件名** | **简短说明** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |一个 PowerShell 脚本，它可以使用 ClusterConfig.json 中的设置创建群集。 |
| RemoveServiceFabricCluster.ps1 |一个 PowerShell 脚本，它可以使用 ClusterConfig.json 中的设置删除群集。 |
| AddNode.ps1 |一个 PowerShell 脚本，用于将节点添加到当前计算机上的现有部署群集。 |
| RemoveNode.ps1 |一个 PowerShell 脚本，用于将节点从当前计算机上的现有部署群集中删除。 |
| CleanFabric.ps1 |一个 PowerShell 脚本，用于从当前计算机中清除独立的 Service Fabric 安装。 以前的 MSI 安装应使用其自身关联的卸载程序来删除。 |
| TestConfiguration.ps1 |一个 PowerShell 脚本，用于分析 Cluster.json 中指定的基础结构。 |
| DownloadServiceFabricRuntimePackage.ps1 |一个 PowerShell 脚本，在部署计算机未连接到 Internet 的方案中，用于在带外下载最新的运行时包。 |
| DeploymentComponentsAutoextractor.exe |自解压存档，包含独立包脚本使用的部署组件。 |
| EULA_ENU.txt |有关使用 Microsoft Azure Service Fabric Windows Server 独立包的许可条款。 可以立即 [下载一份 EULA](http://go.microsoft.com/fwlink/?LinkID=733084) 。 |
| Readme.txt |发行说明和基本安装说明的链接。 本文档包含此文件中的一部分说明。 |
| ThirdPartyNotice.rtf |包中第三方软件的声明。 |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |StandaloneLogCollector.exe 按需运行，收集跟踪日志并将其上传到 Microsoft 以提供支持。 |
| Tools\ServiceFabricUpdateService.zip |用来为不具有 Internet 访问权限的群集启用自动代码升级的工具。 在[此处](service-fabric-cluster-upgrade-windows-server.md)可以找到更多详细信息|

**模板** 
| **文件名** | **简短说明** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |群集配置示例文件，其中包含非安全型三节点式单计算机（或虚拟机）开发群集的设置，这些设置包括群集中每个节点的信息。 |
| ClusterConfig.Unsecure.MultiMachine.json |群集配置示例文件，其中包含非安全型多计算机（或虚拟机）群集的设置，这些设置包括群集中每个计算机的信息。 |
| ClusterConfig.Windows.DevCluster.json |群集配置示例文件，其中包含安全型三节点式单计算机（或虚拟机）开发群集的所有设置，这些设置包括群集中每个节点的信息。 此群集使用 [Windows 标识](https://msdn.microsoft.com/library/ff649396.aspx)进行保护。 |
| ClusterConfig.Windows.MultiMachine.json |群集配置示例文件，其中包含使用 Windows 安全性措施的安全型多计算机（或虚拟机）群集的所有设置，这些设置包括安全群集中每个计算机的信息。 此群集使用 [Windows 标识](https://msdn.microsoft.com/library/ff649396.aspx)进行保护。 |
| ClusterConfig.x509.DevCluster.json |群集配置示例文件，其中包含安全型三节点式单计算机（或虚拟机）开发群集的所有设置，这些设置包括群集中每个节点的信息。 此群集使用 x509 证书进行保护。 |
| ClusterConfig.x509.MultiMachine.json |群集配置示例文件，其中包含安全型多计算机（或虚拟机）群集的所有设置，这些设置包括安全群集中每个节点的信息。 此群集使用 x509 证书进行保护。 |
| ClusterConfig.gMSA.Windows.MultiMachine.json |群集配置示例文件，其中包含安全型多计算机（或虚拟机）群集的所有设置，这些设置包括安全群集中每个节点的信息。 使用[组托管服务帐户](https://technet.microsoft.com/en-us/library/jj128431(v=ws.11).aspx)保护该群集。 |

## <a name="cluster-configuration-samples"></a>群集配置示例
可在以下 GitHub 页面找到最新版本的群集配置模板：[独立群集配置示例](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)。

## <a name="related"></a>相关内容
* [创建独立 Azure Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)
* [Service Fabric 群集安全方案](service-fabric-windows-cluster-windows-security.md)
