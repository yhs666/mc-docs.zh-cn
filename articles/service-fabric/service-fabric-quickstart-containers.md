---
title: 创建 Azure Service Fabric Windows 容器应用程序 | Azure
description: 在本快速入门中，请在 Azure Service Fabric 上创建第一个 Windows 容器应用程序。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 04/30/2018
ms.date: 05/28/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: c1cd385a4e886c38a1ec8005e0dbd7a36437400a
ms.sourcegitcommit: e50f668257c023ca59d7a1df9f1fe02a51757719
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2018
ms.locfileid: "34554283"
---
# <a name="quickstart-deploy-a-service-fabric-windows-container-application-on-azure"></a>快速入门：在 Azure 上部署 Service Fabric Windows 容器应用程序
Azure Service Fabric 是一款分布式系统平台，可用于部署和管理可缩放的可靠微服务和容器。 

在 Service Fabric 群集上运行 Windows 容器中的现有应用程序不需要对应用程序进行任何更改。 本快速入门介绍如何在 Service Fabric 应用程序中部署预建的 Docker 容器映像。 完成后，你会有一个正在运行的 Windows Server 2016 Nano Server 和 IIS 容器。 本快速入门介绍如何部署 Windows 容器。若要部署 Linux 容器，请阅读[此快速入门](service-fabric-quickstart-containers-linux.md)。

![iis-default][iis-default]

此快速入门介绍如何：

* 打包 Docker 映像容器
* 配置通信
* 生成并打包 Service Fabric 应用程序
* 将容器应用程序部署到 Azure

## <a name="prerequisites"></a>先决条件
* 一个 Azure 订阅（可以创建[试用帐户](https://www.azure.cn/pricing/1rmb-trial)）。
* 一台运行以下软件的开发计算机：
  * Visual Studio 2015 或 Visual Studio 2017。
  * [Service Fabric SDK 和工具](service-fabric-get-started.md)。

## <a name="package-a-docker-image-container-with-visual-studio"></a>使用 Visual Studio 打包 Docker 映像容器
Service Fabric SDK 和工具提供服务模板，用于将容器部署到 Service Fabric 群集。

以“管理员”身份启动 Visual Studio。  选择“文件” > “新建” > “项目”。

选择“Service Fabric 应用程序”，将其命名为“MyFirstContainer”，并单击“确定”。

从“托管的容器和应用程序”模板中选择“容器”。

在“映像名称”中输入“microsoft/iis:nanoserver”，即 [Windows Server Nano Server 和 IIS 基映像](https://hub.docker.com/r/microsoft/iis/)。 

配置容器的“端口到主机”端口映射，使端口 80 上针对服务的传入请求映射到容器上的端口 80。  将“容器端口”设置为“80”并将“主机端口”设置为“80”。  

将服务命名为“MyContainerService”，然后单击“确定”。

![新服务对话框][new-service]

## <a name="create-a-cluster"></a>创建群集
若要将应用程序部署到 Azure 中的群集，可[在 Azure 上创建自己的群集](service-fabric-tutorial-create-vnet-and-windows-cluster.md)。
<!-- Notice: follow the previous code due to there is no party service fabric in china-->
<!-- Not Avaiable on Party clusters -->
## <a name="deploy-the-application-to-azure-using-visual-studio"></a>使用 Visual Studio 将应用程序部署到 Azure
至此，应用程序已准备就绪，可以直接通过 Visual Studio 将它部署到群集了。

在解决方案资源管理器中右键单击“MyFirstContainer”，选择“发布”。 此时，“发布”对话框显示。

将 Party 群集页面中的“连接终结点”复制到“连接终结点”字段。 例如，`zwin7fh14scd.chinanorth.cloudapp.chinacloudapi.cn:19000`。
<!-- Not Avaiable on Click **Advanced Connection Parameters** and fill in the following information.  *FindValue* and *ServerCertThumbprint* values must match the thumbprint of the certificate installed in the previous step. -->

单击“发布”。

群集中的每个应用程序都必须具有唯一名称。  Party 群集是一个公共、共享的环境，但是可能与现有应用程序存在冲突。  如果存在名称冲突，请重命名 Visual Studio 项目并重新部署。

打开浏览器并导航到“合作群集”页中指定的“连接终结点”。 可以选择性地在 URL 的前面添加方案标识符 `http://`，并在后面追加端口 `:80`。 例如，http://zwin7fh14scd.chinanorth.cloudapp.chinacloudapi.cn:80。 此时会看到 IIS 默认网页：![IIS 默认网页][iis-default]

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Service Fabric 应用程序和服务清单的完整示例
以下是用在本快速入门中的完整服务和应用程序清单。

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>microsoft/iis:nanoserver</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>后续步骤
在此快速入门中，读者学习了如何：

* 打包 Docker 映像容器
* 配置通信
* 生成并打包 Service Fabric 应用程序
* 将容器应用程序部署到 Azure

若要详细了解如何在 Service Fabric 中使用 Windows 容器，请继续学习适用于 Windows 容器应用的教程。

> [!div class="nextstepaction"]
> [创建 Windows 容器应用](./service-fabric-host-app-in-a-container.md)

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
[new-service]: ./media/service-fabric-quickstart-containers/NewService.png
<!--Update_Description: wording update, update link -->