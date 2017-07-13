---
title: "使用 Visual Studio 对 Windows 排查 Docker 客户端错误 | Azure"
description: "在 Windows 上通过使用 Visual Studio 排查在使用 Visual Studio 创建 Web 应用并将其部署到 Docker 时遇到的问题。"
services: azure-container-service
documentationCenter: na
authors: mlearned
manager: douge
editor: 
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: v-junlch
ms.openlocfilehash: 9e08a062b937b4a9106860dad91edff24eb3fe16
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# Visual Studio Docker 开发故障排除
<a id="troubleshooting-visual-studio-docker-development" class="xliff"></a>

在使用 Visual Studio Tools for Docker 预览版时，可能会因预览特性而遇到一些问题。
以下是一些常见问题和解决方法。

## 无法验证卷映射
<a id="unable-to-validate-volume-mapping" class="xliff"></a>
需要卷映射才能与容器中的应用文件夹共享应用程序的源代码和二进制文件。  docker-compose.dev.debug.yml 和 docker-compose.dev.release.yml 文件中包含特定的卷映射。 主机上的文件更改时，容器会以类似的文件夹结构反映这些更改。

若要启用卷映射，请从 Docker For Windows“小鲸鱼”任务栏图标打开“设置...”，然后选择“共享驱动器”选项卡。  检查托管项目的驱动器号以及 %USERPROFILE% 所在的驱动器号，然后单击“应用” ，确保共享了这些驱动器号。

若要测试卷映射是否正常工作，在共享驱动器后，在 Visual Studio 中执行“重新生成”并按 F5，或者从命令提示符尝试以下操作：

*在 Windows 命令提示符中*

*[注意：这假定 Users 文件夹位于 "C" 驱动器上并且已共享。如果共享了一个不同的驱动器，请根据需要进行更新]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*在 Linux 容器中*

```
/ # ls
```

你应看到 Users/Public 文件夹中的目录列表。
如果未显示任何文件，并且 /c/Users/Public 文件夹不为空，则卷映射未正确配置。 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

转到旋涡式星体目录，以查看 `/c/Users/Public` 目录的内容：

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**注意：** *使用 Linux VM 时，容器文件系统区分大小写。*

##生成：“PrepareForBuild”任务意外失败。
<a id="build--prepareforbuild-task-failed-unexpectedly" class="xliff"></a>

Microsoft.DotNet.Docker.CommandLine.ClientException：尝试连接时出错：

验证默认 Docker 主机是否正在运行。 打开命令提示符并执行以下命令：

```
docker info
```

如果此命令返回错误，则尝试启动 **Docker For Windows** 桌面应用。  如果桌面应用正在运行，则应该在任务栏中看到 **魔比** 图标。 右键单击任务栏图标，打开“设置” 。  单击“重置”选项卡，然后单击“重新启动 Docker...”。

##从 0.31 版手动升级到 0.40 版
<a id="manually-upgrading-from-version-031-to-040" class="xliff"></a>

1. 备份项目
1. 删除项目中的以下文件：

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. 关闭解决方案并从 .xproj 文件中删除以下行：

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. 重新打开解决方案
1. 从 Properties\launchSettings.json 文件中删除下列行：

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. 在 publishOptions 中从 project.json 删除与 Docker 相关的以下文件：

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. 卸载以前的版本并安装 Docker Tools 0.40，然后再次从 ASP.Net Core Web 或控制台应用程序的上下文菜单中选择“添加”->“Docker 支持”。 这会将新的必要 Docker 项目添加回项目。 

## 尝试在容器中执行“添加”->“Docker 支持”时或者调试 (F5) ASP.NET Core 应用程序时出现一个错误对话框
<a id="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container" class="xliff"></a>

在卸载和安装扩展后，有时候可能会看到 Visual Studio 的 MEF（托管扩展框架）缓存可能已损坏。 当发生此情况时，它可能会导致在添加 Docker 支持并且/或者尝试运行或调试 (F5) ASP.NET Core 应用程序时显示各种错误对话框。 暂时的解决方法是执行以下步骤来删除和重新生成 MEF 缓存。

1. 关闭 Visual Studio 的所有实例
1. 打开 %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. 删除以下文件夹
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. 打开 Visual Studio
1. 再次尝试该方案