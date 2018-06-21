---
title: 使用 Visual Studio Online 持续交付云服务 | Microsoft Docs
description: 了解如何设置 Azure 云应用的持续交付，而无需将诊断存储密钥保存到服务配置文件
services: cloud-services
documentationcenter: ''
author: cawa
manager: paulyuk
editor: ''
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: v-yiso
ms.openlocfilehash: daceaf803f2de6c05d3bc5652eb6bec193b05084
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20181622"
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-to-azure-using-visual-studio-online"></a>使用 Visual Studio Online 安全地保存云服务诊断存储密钥及设置对 Azure 的持续集成和部署
 这是现今开放源代码项目的常见做法。 在配置文件中保存应用程序密钥不再是安全的做法，因为存在从公共源控件泄漏密钥的安全漏洞。 以纯文本格式将密钥存储在持续集成管道的文件中也不是安全的做法，因为生成服务器可能会在云环境中共享资源。 本文介绍 Visual Studio 和 Visual Studio Online 如何在开发和持续集成过程中减轻安全问题。

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>删除项目配置文件中的诊断存储密钥
云服务诊断扩展要求提供 Azure 存储才能保存诊断结果。 以前可在云服务配置 (.cscfg) 文件中指定存储连接字符串，并可将其签入到源代码管理。 在最新的 Azure SDK 版本中，我们将该行为更改为仅存储使用令牌替换密钥的部分连接字符串。 以下步骤介绍新的云服务工具的工作原理：

### <a name="1-open-the-role-designer"></a>1.打开角色设计器
* 双击或右键单击云服务角色来打开角色设计器

![打开角色设计器][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2.在诊断部分，添加新的“不删除项目的存储密钥机密信息”复选框
* 如果使用本地存储模拟器，此复选框会被禁用，因为本地连接字符串 (UseDevelopmentStorage=true) 没有任何可管理的密钥。

![本地存储模拟器连接字符串不是密钥][1]

* 如果要创建新项目，默认情况下取消选中此复选框。 这会让选定存储连接字符串的存储密钥部分被令牌替换。 令牌的值可在当前用户的 AppData Roaming 文件夹下找到，例如：C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService

> 请注意，user\AppData 文件夹是用户登录控制的访问，被视为存储开发机密的安全位置。
> 
> 

![存储密钥保存在用户配置文件文件夹下][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3.选择诊断存储帐户
* 单击“...”，从打开的对话框中选择存储帐户 按钮。 请注意生成的存储连接字符串不具有存储帐户密钥的原因。
* 例如：DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(clouddiagstrg.key)

### <a name="4----debugging-the-project"></a>4.  调试项目
* 按 F5 在 Visual Studio 中开始调试。 一切都应如以前一样运行。
  ![开始本地调试][3]

### <a name="5-publish-project-from-visual-studio"></a>5.从 Visual Studio 发布项目
* 启动发布对话框，然后按照登录说明操作，将应用程序发布到 Azure。

### <a name="6-additional-information"></a>6.其他信息
> 注意：角色设计器中的“设置”面板暂时保持原样不变。 如果想要使用诊断的密钥管理功能，请转到“配置”选项卡。
> 
> 

![添加设置][4]

> 注意：如果启用，Application Insights 密钥会以纯文本格式存储。 密钥仅用于上传内容，因此任何敏感数据都不存在泄漏风险。
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>使用 Visual Studio Online 任务模板生成和发布云服务项目
* 以下步骤演示如何使用 Visual Studio Online 任务设置云服务项目的持续集成：
  ### <a name="1----obtain-a-vso-account"></a>1.  获取 VSO 帐户
* [创建 Visual Studio Online 帐户][Create Visual Studio Online account] （如果尚未拥有）
* [创建团队项目][Create team project] 

### <a name="2----setup-source-control-in-visual-studio"></a>2.  在 Visual Studio 中安装源控件
* 连接到团队项目

![连接到团队项目][5]

![选择要连接到的团队项目][6]

* 将项目添加到源控件

![将项目添加到源控件][7]

![将项目映射到源控件文件夹][8]

* 从团队资源管理器签入项目

![将项目签入源控件][9]

### <a name="3----configure-build-process"></a>3.  配置生成过程
* 浏览到团队项目，然后添加新的生成过程模板

![添加新的生成][10]

* 选择生成任务

![添加生成任务][11]

![选择 Visual Studio 生成任务模板][12]

* 编辑生成任务输入。 请根据需要自定义生成参数

![配置生成任务][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* 配置生成变量

![配置生成变量][14]

* 添加用于上传生成放置的任务

![选择发布生成放置任务][15]

![配置发布生成放置任务][16]

* 运行生成

![将新的生成排队][17]

![查看生成摘要][18]

* 如果生成成功，会看到类似如下的结果

![生成结果][19]

### <a name="4----configure-release-process"></a>4.  配置发布过程
* 创建新的发布

![创建新的发布][20]

* 选择 Azure 云服务部署任务

![选择 Azure 云服务部署任务][21]

* 由于存储帐户密钥未签入源控件，因此需要为设置诊断扩展指定机密密钥。 展开“用于创建新服务的高级选项”部分，然后编辑“诊断存储帐户密钥”参数输入。 此输入以 [RoleName]:$(StorageAccountKey) 格式采取多行键值对

> 注意：如果诊断存储帐户位于要在其中发布云服务应用程序的同一订阅下，则无需在部署任务输入中输入密钥；部署会以编程方式从订阅获取存储信息
> 
> 

![配置 Azure 云服务部署任务][22]

* 使用密钥生成变量保存存储密钥。 若要屏蔽作为密钥的变量，请单击变量输入右侧的锁图标

![在密钥生成变量中保存存储密钥][23]

* 创建发布并将项目部署到 Azure

![创建新的发布][24]

## <a name="next-steps"></a>后续步骤
若要了解如何设置 Azure 云服务的诊断扩展的详细信息，请参阅 [Enable diagnostics in Azure Cloud Services using PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png