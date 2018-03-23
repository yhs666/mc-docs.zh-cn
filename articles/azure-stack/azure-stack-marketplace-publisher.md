---
title: 使用 Marketplace 工具包创建和发布 Marketplace 项 | Microsoft Docs
description: 了解如何使用发布工具包快速创建 Marketplace 项
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/14/2017
ms.date: 03/02/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: 5e4a8017330e3518ff77b674e0280827f9e4ec18
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
#  <a name="add-marketplace-items-using-publishing-tool"></a>使用发布工具添加 Marketplace 项
将内容添加到 [Azure Stack Marketplace](azure-stack-marketplace.md) 后，你和租户可将你的解决方案用于部署。  Marketplace 工具包基于 IaaS Azure 资源管理器模板或 VM 扩展创建 Azure Marketplace 包 (.azpkg) 文件。  还可以使用 Marketplace 工具包来发布通过工具或[手动](azure-stack-create-and-publish-marketplace-item.md)步骤创建的 .azpkg 文件。  本主题逐步讲解如何下载该工具、基于 VM 模板创建 Marketplace 项，然后将该项发布到 Azure Stack Marketplace。     


## <a name="prerequisites"></a>先决条件
 - 必须在 Azure Stack 主机上运行此工具包，或在运行此工具的计算机上建立 [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) 连接。

 - 下载 [Azure Stack 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip)并解压缩。

 - 下载 [Azure 库包工具](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe)。 

 - 要发布到 Marketplace 需有图标和缩略图文件。  可以使用自己的文件，或者将本示例的[示例](azure-stack-marketplace-publisher.md#support-files)文件保存在本地。

## <a name="download-the-tool"></a>下载工具
可[从 Azure Stack 工具存储库下载](azure-stack-powershell-download.md) Marketplace 工具包。


##  <a name="create-marketplace-items"></a>创建 Marketplace 项
在本部分，我们将使用 Marketplace 工具包创建 .azpkg 格式的 Marketplace 项包。  

### <a name="provide-marketplace-information-with-wizard"></a>在向导中提供 Marketplace 信息
1. 从 PowerShell 会话运行 Marketplace 工具包：
    ```PowerShell
    .\MarketplaceToolkit.ps1
    ```

2. 单击“解决方案”选项卡。此屏幕接受 Marketplace 项相关信息。 请输入想要显示在 Marketplace 中的项目相关信息。  也可以指定[参数文件](azure-stack-marketplace-publisher.md#use-a-parameters-file)来预先填充窗体。  
    
    ![Marketplace 工具包第一个屏幕的屏幕截图](./media/azure-stack-marketplace-publisher/image7.png)
3. 单击“浏览”以选择每个图标和屏幕截图字段的图像文件。  可以使用自己的图标，或[支持文件](azure-stack-marketplace-publisher.md#support-files)部分中的示例图标。
4. 填充所有字段后，选择“预览解决方案”来预览 Marketplace 内的解决方案。  可以修改并编辑文本、图像和屏幕截图，然后单击“下一步”。  

### <a name="import-template-and-create-package"></a>导入模板并创建包
在本部分，我们导入模板并使用解决方案的输入。

1.  单击“浏览”并从所下载模板的 101-Simple-Windows-VM 文件夹中选择 *azuredeploy.json*。

    ![Marketplace 工具包第二个屏幕的屏幕截图](./media/azure-stack-marketplace-publisher/image8.png)
2.  部署向导中填充了“基本”步骤，以及模板中所指定每个参数的输入项。  可以添加其他步骤，并在步骤之间移动输入项。  例如，你可能需要为解决方案添加“前端配置”和“后端配置”步骤。
3.  指定 AzureGalleryPackager.exe 的路径。  
4.  单击“创建”，然后 Marketplace 工具包会将解决方案打包成 .azpkg 文件。  完成后，向导会显示解决方案文件的路径，并让你选择继续将包发布到 Azure Stack。


## <a name="publish-marketplace-items"></a>发布 Marketplace 项
在本部分，我们将 Marketplace 项发布到 Azure Stack Marketplace。

![Marketplace 工具包第一个屏幕的屏幕截图](./media/azure-stack-marketplace-publisher/image9.png)

1.  向导需要以下信息来发布解决方案：
    
    |字段|说明|
    |-----|-----|
    | 服务管理员名称 | 服务管理员帐户。  示例：ServiceAdmin@mydomain.partner.onmschina.cn |
    | 密码 | 服务管理员帐户的密码。 |
    | API 终结点 | Azure Stack Azure 资源管理器终结点。  例如：management.local.azurestack.external |
2.  单击“发布”，发布日志随即显示。
3.  现在，可以通过 Azure Stack 门户部署发布的项。


## <a name="use-a-parameters-file"></a>使用参数文件
也可以使用参数文件来填写 Marketplace 项信息。  

Marketplace 工具包包含 *solution.parameters.ps1*，可以使用此脚本来创建自己的参数文件。


## <a name="support-files"></a>支持文件
| 说明 | 示例 |
| ----- | ----- |
| 40x40 .png 图标 | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| 90x90 .png 图标 | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| 115x115 .png 图标 | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| 255x115 .png 图标 | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| 533x324 .png 缩略图 | ![](./media/azure-stack-marketplace-publisher/image5.png) |



