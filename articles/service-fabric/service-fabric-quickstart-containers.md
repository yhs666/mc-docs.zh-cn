---
title: 在 Azure 中的 Service Fabric 上创建 Windows 容器应用 | Azure
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
ms.date: 12/10/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 1f5cf245361bcc54af8dedc16881b1782e012d4a
ms.sourcegitcommit: 38f95433f2877cd649587fd3b68112fb6909e0cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "52901056"
---
<!--Verify Successfully-->
# <a name="quickstart-deploy-windows-containers-to-service-fabric"></a>快速入门：将 Windows 容器部署到 Service Fabric

Azure Service Fabric 是一款分布式系统平台，可用于部署和管理可缩放的可靠微服务和容器。

在 Service Fabric 群集上运行 Windows 容器中的现有应用程序不需要对应用程序进行任何更改。 本快速入门介绍如何在 Service Fabric 应用程序中部署预建的 Docker 容器映像。 完成后，你会有一个正在运行的 Windows Server 2016 Nano Server 和 IIS 容器。 本快速入门介绍如何部署 Windows 容器。若要部署 Linux 容器，请阅读[此快速入门](service-fabric-quickstart-containers-linux.md)。

![IIS 默认网页][iis-default]

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

选择“云” > “Service Fabric 应用程序”，将其命名为“MyFirstContainer”，并单击“确定”。

<!--Notice: We add **Cloud** to help user search **Service Fabric application**->
Select **Container** from the **Hosted Containers and Applications** templates.

In **Image Name**, enter "microsoft/iis:nanoserver", the [Windows Server Nano Server and IIS base image](https://hub.docker.com/r/microsoft/iis/).

Configure the container port-to-host port mapping so that incoming requests to the service on port 80 are mapped to port 80 on the container.  Set **Container Port** to "80" and set **Host Port** to "80".  

Name your service "MyContainerService", and click **OK**.

![New service dialog][new-service]

## Specify the OS build for your container image
Containers built with a specific version of Windows Server may not run on a host running a different version of Windows Server. For example, containers built using Windows Server version 1709 do not run on hosts running Windows Server 2016. To learn more, see [Windows Server container OS and host OS compatibility](service-fabric-get-started-containers.md#windows-server-container-os-and-host-os-compatibility). 

With version 6.1 of the Service Fabric runtime and newer, you can specify multiple OS images per container and tag each with the build version of the OS that it should be deployed to. This helps to make sure that your application will run across hosts running different versions of Windows OS. To learn more, see [Specify OS build specific container images](service-fabric-get-started-containers.md#specify-os-build-specific-container-images). 

Azure publishes different images for versions of IIS built on different versions of Windows Server. To make sure that Service Fabric deploys a container compatible with the version of Windows Server running on the cluster nodes where it deploys your application, add the following lines to the *ApplicationManifest.xml* file. The build version for Windows Server 2016 is 14393 and the build version for Windows Server version 1709 is 16299. 

```xml
    <ContainerHostPolicies CodePackageRef="Code"> 
      <ImageOverrides> 
        ...
          <Image Name="microsoft/iis:nanoserverDefault" /> 
          <Image Name= "microsoft/iis:nanoserver" Os="14393" /> 
          <Image Name="microsoft/iis:windowsservercore-1709" Os="16299" /> 
      </ImageOverrides> 
    </ContainerHostPolicies> 
```

The service manifest continues to specify only one image for the nanoserver, `microsoft/iis:nanoserver`. 

<!-- Not Available on ## Create a cluster-->
<!-- Not Available on Join Party-->
<!-- Not Available on [join a Windows cluster](http://aka.ms/tryservicefabric)-->

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>使用 Visual Studio 将应用程序部署到 Azure

至此，应用程序已准备就绪，可以直接通过 Visual Studio 将它部署到群集了。

在解决方案资源管理器中右键单击“MyFirstContainer”，选择“发布”。 此时，“发布”对话框显示。

1. 在“连接终结点”列表中选择 `<Create New Cluster...>` 的项。
    ![创建新群集](./media/service-fabric-quickstart-containers/publish-app-chenye-step-1-create-new-cluster.png)
    
2. 设置“群集”选项卡信息。
    ![设置群集信息](./media/service-fabric-quickstart-containers/publish-app-chenye-step-2-set-cluster.png)
    
3. 设置“证书”选项卡信息。
    ![设置证书信息](./media/service-fabric-quickstart-containers/publish-app-chenye-step-3-set-certificate.png)
    
4. 设置“VM”选项卡信息，然后选择“创建”。
    ![设置证书信息](./media/service-fabric-quickstart-containers/publish-app-chenye-step-4-set-vm.png)

5. 群集中的每个应用程序都必须具有唯一名称。 如果存在名称冲突，请重命名 Visual Studio 项目并重新部署。
    <!--Not Available on Party clusters are a public, shared environment however and there may be a conflict with an existing application.-->

6. 登录到 [Azure 门户](https://portal.azure.cn)。 通过单击 **PFX** 链接，将 PFX 证书下载到计算机。 单击“如何连接到安全合作群集?”链接并复制证书密码。 后续步骤中需要使用证书、证书密码和“连接终结点”值。

    ![PFX 和连接终结点](./media/service-fabric-quickstart-containers/publish-app-chenye-download-certificate.png)

    在 Windows 计算机上，将 PFX 安装到 *CurrentUser\My* 证书存储中。

    ```powershell
    PS C:\mycertificates> Import-PfxCertificate -FilePath .\<your-saved-certificate-name>.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)

      PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

    Thumbprint                                Subject
    ----------                                -------
    3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.chinanorth.cloudapp.chinacloudapi.cn
    ```


7. 打开浏览器并导航到在群集页中指定的“连接终结点”。 可以选择性地在 URL 的前面添加方案标识符 `http://`，并在后面追加端口 `:80`。 例如， http://zwin7fh14scd.chinanorth.cloudapp.chinacloudapi.cn:80。 此时会看到 IIS 默认网页：![IIS 默认网页][iis-default]

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