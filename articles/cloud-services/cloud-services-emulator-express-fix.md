---
title: 在 Visual Studio 中安装 Emulator Express 以调试云服务应用程序 | Microsoft Docs
description: 介绍如何安装 C++ Redistributable，在 Visual Studio 中启用 Emulator Express
services: cloud-services
documentationcenter: ''
author: cawa
manager: paulyuk
editor: ''
ms.assetid: 22b20f7a-23f4-4f7f-b536-3bf1e01adcd1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: v-yiso
ms.openlocfilehash: aceacfc37ae878578d4b9ee5c251fc6486674f71
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52656490"
---
# <a name="use-emulator-express-to-debug-cloud-services-application-in-vs-2017"></a>在 VS 2017 中使用 Emulator Express 调试云服务应用程序
本文介绍如何在 VS 2017 中使用 Emulator Express 调试云服务应用程序。

## <a name="background-context"></a>后台上下文
在 Visual Studio 中，默认使用 Emulator Express 来调试云服务 Web 角色和辅助角色。 此设置在云服务项目属性页中指定。

![打开项目属性][0]

![默认选择 Emulator Express][1]

Emulator Express 要求提供 [Visual C++ Redistributable][Visual C++ Redistributable] for Visual Studio。 它当前不随 Azure 工作负荷安装。 使用 F5 手势调试云服务应用程序时，Visual Studio 会提示安装此组件，并继续调试。

![提示安装 C++ Redistributable][2]

单击“是”安装 C++ Redistributable。

![安装 C++ Redistributable][3]

再次按 F5 启动调试会话。

![开始调试][4]

![调试成功][5]

> 注意：安装 Visual C++ Redistributable 是一次性工作。 如果从之前版本的 Azure SDK 升级并且已安装 Emulator Express，则不会遇到此问题。
> 
> 

## <a name="manual-workaround"></a>手动解决方法
还可以手动安装 [Visual C++ Redistributable][Visual C++ Redistributable] ，效果和通过 Visual Studio 在系统上安装该组件一样。

[vcredist_x86.exe][vcredist_x86.exe]

[vcredist_x64.exe][vcredist_x64.exe]

## <a name="next-steps"></a>后续步骤
了解有关在 Visual Studio 中使用 Azure 计算模拟器调试云服务应用程序的详细信息： [Using Emulator Express to run and debug a cloud service on a local machine][Using Emulator Express to run and debug a cloud service on a local machine]

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Using Emulator Express to run and debug a cloud service on a local machine]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png