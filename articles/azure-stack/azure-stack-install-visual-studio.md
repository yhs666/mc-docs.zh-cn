---
title: 安装 Visual Studio 并连接到 Azure Stack | Microsoft Docs
description: 了解安装 Visual Studio 并连接到 Azure Stack 所需的步骤
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/21/2018
ms.date: 03/02/2018
ms.author: v-junlch
ms.reviewer: unknown
ms.openlocfilehash: b0a927d722ccaf084f7cd277dff822888fb669e8
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>安装 Visual Studio 并连接到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用 Visual Studio 在 Azure Stack 中创建和部署 Azure 资源管理器[模板](user/azure-stack-arm-templates.md)。 可以按照本文中所述的步骤，从 [Azure Stack 开发工具包](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop)或从基于 Windows 的外部客户端（如果已通过 [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) 连接）安装 Visual Studio。 这些步骤执行 Visual Studio 2015 Community Edition 的全新安装。 详细了解其他 Visual Studio 版本之间的[共存](https://msdn.microsoft.com/library/ms246609.aspx)。

## <a name="install-visual-studio"></a>安装 Visual Studio
1. 下载并运行此 [Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。             
2. 搜索“Visual Studio Community 2015 with Azure SDK - 2.9.6”，依次单击“添加”和“安装”。

    ![WebPI 安装步骤的屏幕截图](./media/azure-stack-install-visual-studio/image1.png) 

3. 卸载作为 Azure SDK 的一部分安装的 **Azure PowerShell**。

    ![Azure PowerShell 的“添加/删除程序”界面的屏幕截图](./media/azure-stack-install-visual-studio/image2.png) 

4. [安装适用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)

5. 安装完成后，重启操作系统。

## <a name="connect-to-azure-stack"></a>连接到 Azure Stack

1. 启动 Visual Studio。

2. 在“视图”菜单中，选择“云资源管理器”。

3. 在新窗格中，选择“添加帐户”并使用 Azure Active Directory 凭据登录。  
    ![登录并连接到 Azure Stack 后的 Cloud Explorer 屏幕截图](./media/azure-stack-install-visual-studio/image6.png)

登录后，可以[部署模板](user/azure-stack-deploy-template-visual-studio.md)或浏览可用的资源类型和资源组以创建自己的模板。  

## <a name="next-steps"></a>后续步骤

 - [为 Azure Stack 开发模板](user/azure-stack-develop-templates.md)

