---
title: "升级 Windows Server 上的独立 Azure Service Fabric 群集 | Azure"
description: "升级运行独立 Service Fabric 群集的 Azure Service Fabric 代码和/或配置，其中包括设置群集更新模式。"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 10/15/2017
ms.date: 11/13/2017
ms.author: v-yeche
ms.openlocfilehash: 80d4f278e18d1d039bdf049fbdd532047ba61b6f
ms.sourcegitcommit: 530b78461fda7f0803c27c3e6cb3654975bd3c45
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a>升级 Windows Server 群集上的独立 Azure Service Fabric
> [!div class="op_single_selector"]
> * [Azure 群集](service-fabric-cluster-upgrade.md)
> * [独立群集](service-fabric-cluster-upgrade-windows-server.md)
>
>

对于任何新式系统而言，升级能力是实现产品长期成功的关键所在。 Azure Service Fabric 群集是你拥有的资源。 本文介绍如何确保群集始终运行受支持的 Service Fabric 代码和配置版本。

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>控制群集上运行的 Service Fabric 版本
若要将群集设置为在 Microsoft 发布新版本时下载 Service Fabric 的更新，可将 **fabricClusterAutoupgradeEnabled** 群集配置设置为 true。 如果希望群集一直使用某个受支持的 Service Fabric 版本，可将 **fabricClusterAutoupgradeEnabled** 群集配置设置为 false。

> [!NOTE]
> 确保群集始终运行受支持的 Service Fabric 版本。 当 Microsoft 宣布推出新版 Service Fabric 时，即标志着自宣布日期起至少 60 天后，将结束对旧版的支持。 新版本将[在 Service Fabric 团队博客](https://blogs.msdn.microsoft.com/azureservicefabric/)中宣布。 从该时间开始，便可以选择使用新版本。
>
>

仅在使用生产类型节点配置（其中每个 Service Fabric 节点分配在单独的物理计算机或虚拟机上）时，才能将群集升级到新版本。 如果使用的是开发群集（单个物理机或虚拟机上有多个 Service Fabric 节点），必须使用新版本重新创建该群集。

可以使用两个不同的工作流将群集升级至最新版本或受支持的 Service Fabric 版本。 其中一个工作流适用于已建立网络连接，可自动下载最新版本的群集。 另一个工作流适用于未建立网络连接，无法下载最新 Service Fabric 版本的群集。

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>升级已建立网络连接，可下载最新代码和配置的群集
如果群集节点已与 [http://download.microsoft.com](http://download.microsoft.com) 建立 Internet 连接，可以使用以下步骤将群集升级到支持的版本。

对于已连接到 [http://download.microsoft.com](http://download.microsoft.com) 的群集，我们会定期检查是否有新的 Service Fabric 版本可用。

推出新的 Service Fabric 版本时，相应的包将下载到群集本地，并为升级完成预配。 此外，为了通知客户已推出新版本，系统会明确显示类似于下面的群集运行状况警告：

“当前的群集版本 [版本号] 支持在 [日期] 结束。”

群集运行最新版本后，警告将会消失。

#### <a name="cluster-upgrade-workflow"></a>群集升级工作流
看到群集运行状况警告后，请执行以下操作：

1. 从对群集配置文件中列为节点的所有计算机拥有管理员访问权限的任何计算机连接到该群集。 运行此脚本的计算机不必要是群集的一部分。

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. 获取可以升级到的 Service Fabric 版本的列表。

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    应会看到类似于下面的输出：

    ![获取结构版本][getfabversions]
3. 使用 [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell 命令，开始将群集升级到可用的版本。

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   若要监视升级进度，可以使用 Service Fabric Explorer，也可以运行以下 Windows PowerShell 命令。

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    如果不符合现行的群集运行状况策略，则回滚升级。 若要为 **Start-ServiceFabricClusterUpgrade** 命令指定自定义运行状况策略，请参阅 [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) 的文档。

解决造成回滚的问题后，请遵循前面所述的相同步骤再次启动升级。

### <a name="upgrade-clusters-that-have-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>升级 <U>未建立网络连接</u>，无法下载最新代码和配置的群集
如果群集节点无法通过 Internet 连接到 [http://download.microsoft.com](http://download.microsoft.com)，请使用以下步骤将群集升级到受支持的版本。

> [!NOTE]
> 如果运行的群集无法连接到 Internet，则必须关注 Service Fabric 团队博客来了解新版本的信息。 系统不会显示任何群集运行状况警告来提醒你有新版本可用。  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>自动预配与手动预配
若要为最新代码版本启用自动下载和注册，请设置 Service Fabric 更新服务。 请参阅[独立包](service-fabric-cluster-standalone-package-contents.md)中的 Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt 获取相关说明。
对于手动步骤，请根据以下说明操作。

在启动配置升级之前，请修改群集配置，将以下属性设置为 false。

    "fabricClusterAutoupgradeEnabled": false,

请参阅 [Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/library/mt788302.aspx) 了解使用的详细信息。 在启动配置升级之前，请务必在 JSON 中更新“clusterConfigurationVersion”。

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>群集升级工作流

1. 从群集中的一个节点运行 Get-ServiceFabricClusterUpgrade 并记下 TargetCodeVersion。
2. 从与 Internet 相连的计算机上运行以下命令，列出与当前版本兼容的所有升级版本，并从关联的下载链接下载相应的包。

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. 从对群集配置文件中列为节点的所有计算机拥有管理员访问权限的任何计算机连接到该群集。 运行此脚本的计算机不必要是群集的一部分

    ```powershell

    ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. 将下载的包复制到群集映像存储区中。

5. 注册复制的程序包。

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. 开始将群集升级到可用版本。

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
    可以在 Service Fabric Explorer 中或通过运行以下 PowerShell 命令来监视升级进度。

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    如果不符合现行的群集运行状况策略，则回滚升级。 若要为 **Start-ServiceFabricClusterUpgrade** 命令指定自定义运行状况策略，请参阅 [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) 的文档。

解决造成回滚的问题后，请遵循前面所述的相同步骤再次启动升级。

## <a name="upgrade-the-cluster-configuration"></a>升级群集配置
启动配置升级前，可通过运行独立程序包中的 powershell 脚本来测试新群集配置 json。

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
或

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

部分配置不能升级（如终结点、群集名称、节点 IP 等）。这将针对旧配置测试新的群集配置 json ，并在出现问题时在 Powershell 窗口中引发错误。

若要升级群集配置，请运行 **Start-ServiceFabricClusterConfigurationUpgrade**。 按升级域逐个处理配置升级。

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>群集证书配置升级  
群集证书用于群集节点之间的身份验证，因此执行证书切换时应额外小心，因为失败会阻止群集节点间的通信。  
从技术上讲，支持三种选择：  

1. 单个证书升级：升级路径为“证书 A（主证书）-> 证书 B（主证书）-> 证书 C（主证书）-> ...”。   
2. 双证书升级：升级路径为“证书 A（主证书）-> 证书 A（主证书）和证书 B（辅助证书）-> 证书 B（主证书）-> 证书 B（主证书）和证书 C（辅助证书）-> 证书 C（主证书）-> ...”。
3. 证书类型升级：“基于指纹的证书配置 <-> 基于 CommonName 的证书配置”。 例如，“证书指纹 A（主证书）和指纹 B（辅助证书）-> 证书 CommonName C”。
4. 证书颁发者指纹升级：升级路径为“Certificate CN=A,IssuerThumbprint=IT1 (Primary) -> Certificate CN=A,IssuerThumbprint=IT1,IT2 (Primary) -> Certificate CN=A,IssuerThumbprint=IT2 (Primary)”

## <a name="next-steps"></a>后续步骤
* 了解如何自定义某些 [Service Fabric 群集设置](service-fabric-cluster-fabric-settings.md)。
* 了解如何[扩大和缩小群集](service-fabric-cluster-scale-up-down.md)。
* 了解[应用程序升级](service-fabric-application-upgrade.md)。

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG

<!--Update_Description: wording update-->