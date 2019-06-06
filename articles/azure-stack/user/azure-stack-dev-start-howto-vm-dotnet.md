---
title: 将 C# ASP .Net Web 应用部署到 Azure Stack 中的虚拟机 | Microsoft Docs
description: 将 C# ASP.net Web 应用程序部署到 Azure Stack 中的 VM。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 86e56f13497929eac4f1906f2549b6151ee5d211
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249150"
---
# <a name="how-to-deploy-a-c-aspnet-web-app-to-a-vm-in-azure-stack"></a>如何将 C# ASP.net Web 应用程序部署到 Azure Stack 中的 VM

可以创建一个 VM 用于在 Azure Stack 中托管 C# (ASP.NET) Web 应用。 本文探讨在设置服务器、配置服务器以托管 C# (ASP.NET) Web 应用，然后直接从 Visual Studio 部署该应用时所要遵循的步骤。

C# 是一种通用的多模式编程语言，其中包括强类型化、词法范围限定、命令式、声明性、函数型、泛型、面向对象和面向组件的编程原则。 若要了解 C# 编程语言和查找有关 C# 的其他资源，请参阅 [C# 指南](https://docs.microsoft.com/dotnet/csharp/)。

本文使用一个 C# 6.0 应用，该应用使用在 Windows 2016 服务器上运行的 ASP.NET Core 2.2。

## <a name="create-a-vm"></a>创建 VM

1. 创建 [Windows Server VM](azure-stack-quick-windows-portal.md)。

2. 运行以下脚本，在 VM 上安装组件。 脚本：
      - 安装 IIS（使用管理控制台）。
      - 安装 ASP.NET 4.6。

        ```PowerShell  
        # Install IIS (with Management Console)
        Install-WindowsFeature -name Web-Server -IncludeManagementTools
        
        # Install ASP.NET 4.6
        Install-WindowsFeature Web-Asp-Net45
        
        # Install Web Management Service
        Install-WindowsFeature -Name Web-Mgmt-Service
        ```

3. 下载 [Web 部署 3.6 的 MSI](https://www.microsoft.com/download/details.aspx?id=43717)。 从 MSI 文件完成安装，然后启用所有功能。

4. 在服务器上安装 .NET Core 2.2 Hosting Bundle。 有关步骤，请参阅 [.NET Core 安装程序](https://dotnet.microsoft.com/download/dotnet-core/2.2)。 确保在开发计算机和目标服务器上使用相同版本的 .NET Core。

5. 返回 Azure Stack 门户，打开 VM 网络设置中的端口。

    1. 打开租户的 Azure Stack 门户。
    2. 找到你的 VM。 你可能已将 VM 固定到仪表板；或者，可以在“搜索资源”框中搜索该 VM。 
    3. 选择“网络”  。
    4. 选择 VM 下的“添加入站端口规则”。 
    1. 为以下端口添加入站安全规则：

    | 端口 | 协议 | 说明 |
    | --- | --- | --- |
    | 80 | HTTP | 超文本传输协议 (HTTP) 是一种适用于分布式协作型超媒体信息系统的应用程序协议。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 443 | HTTPS | 安全超文本传输协议 (HTTPS) 是超文本传输协议 (HTTP) 的扩展。 它用于通过计算机网络进行安全通信。 客户端将使用 VM 的公共 IP 或 DNS 名称连接到你的 Web 应用。 |
    | 22 | SSH | 安全外壳 (SSH) 是一种加密网络协议，用于在不安全的网络上安全地运行网络服务。 你将在 SSH 客户端上使用此连接来配置 VM 并部署应用。 |
    | 3389 | RDP | 可选。 远程桌面协议允许远程桌面连接使用计算机的图形用户界面。   |

    对于每个端口：

    1. 为“源”选择“任何”。 
    1. 为“源端口范围”键入 `*`。
    1. 为“目标”选择“任何”。  
    1. 对于“目标端口范围”，请添加要打开的端口。 
    1. 为“协议”选择“任何”。  
    1. 为“操作”选择“允许”。  
    1. 对“优先级”保留默认值。 
    1. 输入**名称**，并提供**说明**来解释为何打开该端口。
    1. 选择“添加”。

5.  对于 Azure Stack 中的 VM，在“网络”设置中创建服务器的 DNS 名称。  用户可使用 URL 连接到你的网站。

    1. 打开租户的 Azure Stack 门户。
    1. 找到你的 VM。 你可能已将 VM 固定到仪表板；或者，可以在“搜索资源”框中搜索该 VM。 
    1. 选择“概述”。 
    1. 选择 VM 下的“配置”  。
    1. 为“分配”选择“动态”。  
    1. 键入 DNS 名称标签（例如 `mywebapp`），完整的 URL 类似于：`mywebapp.local.cloudapp.azurestack.external`。

## <a name="create-an-app"></a>创建应用 

可以使用你自己的 Web 应用，或使用[使用 Visual Studio 将 ASP.NET Core 应用发布到 Azure](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-2.2&tabs=visual-studio
) 中的示例。

本文将介绍如何使用 Visual Studio 2017 中的 Microsoft Azure 虚拟机发布功能，创建一个 ASP.NET Web 应用程序并将其发布到 Azure 虚拟机 (VM)。 安装应用并确保它在本地运行后，将发布目标更新为 Azure Stack 中的 Windows VM。

## <a name="deploy-and-run-the-app"></a>部署和运行应用

为 Azure Stack 中的 VM 创建发布目标。

1. 在解决方案资源管理器中，右键单击该项目并选择“发布”。 

    ![将 ASP.NET Web 应用部署到 Azure Stack 发布](media/azure-stack-dev-start-howto-vm-dotnet/deploy-app-to-azure-stack.png)

2.  在“发布”窗口中选择“新建配置文件”。
3. 选择“IIS”、“FTP”等。
4. 选择“发布”。

5.  为“发布方法”选择“Web 部署”。  
6.  对于“服务器”，请输入前面定义的 DNS 名称，例如 `w21902.local.cloudapp.azurestack.external` 
7.  对于“站点名称”，请键入 `Default Web Site`。 
8.  对于“用户名”，请键入计算机的用户名。 
9.  对于“密码”，请键入计算机的密码。 
10. 对于“目标 URL”，请键入站点的 URL，例如 `mywebapp.local.cloudapp.azurestack.external`。 

    ![部署 ASP.NET Web 应用 - 配置 Web 部署](media/azure-stack-dev-start-howto-vm-dotnet/configure-web-deploy.png)

9. 选择“验证连接”以验证 Web 部署配置。  然后单击“下一步”。 
10. 将“配置”设置为“发布”。  
11. 将“目标框架”设置为“netcoreapp2.2”。  
12. 将“目标运行时”设置为“便携式”。  
13. 选择“其他安全性验证”  。
14. 选择“发布”  。
15. 导航到新服务器。 应会看到你的 Web 应用程序正在运行。

```HTTP  
    mywebapp.local.cloudapp.azurestack.external
```

## <a name="next-steps"></a>后续步骤

- 详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)
- 了解[用作 IaaS 的 Azure Stack 的常见部署](azure-stack-dev-start-deploy-app.md)。