---
title: 从 Azure 添加 Azure Stack Marketplace 项 | Microsoft Docs
description: 介绍如何向 Azure Stack Marketplace 添加基于 Azure 的 Windows Server 虚拟机映像。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
origin.date: 03/16/2018
ms.date: 03/22/2018
ms.author: v-junlch
ms.reviewer: misainat
ms.openlocfilehash: 5096a45e656d5a8eb81021b2d84593b4ff3cd942
ms.sourcegitcommit: 61fc3bfb9acd507060eb030de2c79de2376e7dd3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
ms.locfileid: "30155676"
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-azure"></a>教程：从 Azure 添加 Azure Stack Marketplace 项

作为 Azure Stack 操作员，若要让用户能够部署虚拟机 (VM)，首先需要做的事是将 VM 映像添加到 Azure Stack Marketplace。 默认情况下，没有内容发布到 Azure Stack Marketplace，但你可以从[特选的 Azure Marketplace 项列表](../azure-stack-marketplace-azure-items.md)中下载项。这些项已经过预先测试，可以在 Azure Stack 上运行。 如果在联网场景中执行操作，并且已向 Azure 注册 Azure Stack 实例，请使用此选项。

在本教程中，请将 Windows Server 2016 VM 映像从 Azure Marketplace 添加到 Azure Stack Marketplace。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 添加 Windows Server VM Azure Stack Marketplace 项
> * 验证 VM 映像是否可用 
> * 测试 VM 映像

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

完成本教程：

- 安装 [Azure Stack 兼容的 Azure PowerShell 模块](asdk-post-deploy.md#install-azure-stack-powershell)
- 下载 [Azure Stack 工具](asdk-post-deploy.md#download-the-azure-stack-tools)
- [将 ASDK 注册](asdk-register.md)到 Azure 订阅

## <a name="add-a-windows-server-vm-image"></a>添加 Windows Server VM 映像
可以从 Azure Marketplace 下载 Windows Server 2016 映像，然后再将其添加到 Azure Stack Marketplace。 如果在联网场景中执行操作，并且已向 Azure [注册 Azure Stack 实例](asdk-register.md)，请使用此选项。

1. 登录到 [ASDK 管理员门户](https://adminportal.local.azurestack.external)。

2. 单击“更多服务” > “Marketplace 管理” > “从 Azure 添加”。 

   ![从 Azure 添加](./media/asdk-marketplace-item/azs-marketplace.png)

3. 查找或搜索 **Windows Server 2016 Datacenter** 映像。

4. 选择 **Windows Server 2016 Datacenter** 映像，然后选择“下载”。

   ![从 Azure 下载映像](./media/asdk-marketplace-item/azure-marketplace-ws2016.png)


> [!NOTE]
> 下载此 VM 映像并将其发布到 Azure Stack Marketplace 大约需要一小时。 

下载完成后会发布该映像，将其显示在“Marketplace 管理”下。 该映像也可以从“计算”获取，用于创建新的虚拟机。

## <a name="verify-the-marketplace-item-is-available"></a>验证是否已提供 Marketplace 项
使用以下步骤，验证新的 VM 映像是否已在 Azure Stack Marketplace 中提供。

1. 登录到 [ASDK 管理员门户](https://adminportal.local.azurestack.external)。

2. 选择“更多服务” > “Marketplace 管理”。 

3. 验证是否已成功添加 Windows Server 2016 Datacenter VM 映像。

   ![从 Azure 下载的映像](./media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>测试 VM 映像
Azure Stack 操作员可以使用[管理员门户](https://adminportal.local.azurestack.external)来创建测试 VM，以验证是否已成功提供映像。 

1. 登录到 [ASDK 管理员门户](https://adminportal.local.azurestack.external)。

2. 单击“新建” > “计算” > “Windows Server 2016 Datacenter” > “创建”。  
 ![从 Marketplace 映像创建 VM](./media/asdk-marketplace-item/new-compute.png)

3. 在“基本信息”边栏选项卡中输入以下信息，并单击“确定”：
  - **名称**：test-vm-1
  - **用户名**：AdminTestUser
  - **密码**：AzS TestVM01
  - **订阅**：接受“默认提供商订阅”
  - **资源组**：test-vm-rg
  - **位置**：本地

4. 在“选择大小”边栏选项卡中，单击“A1 标准”，并单击“选择”。  

5. 在“设置”边栏选项卡中接受默认值，然后单击“确定”

6. 在“摘要”边栏选项卡中，单击“确定”创建虚拟机。  
> [!NOTE]
> 虚拟机部署需要几分钟时间才能完成。

7. 若要查看新 VM，请单击“虚拟机”，然后搜索 **test-vm-1** 并单击其名称。
    ![显示的第一个测试 VM 映像](./media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>清理资源
成功登录到 VM 并验证测试映像可正常运行后，应删除测试资源组。 这会释放有限的资源供单节点 ASDK 安装使用。

如果不再需要资源组、VM 和所有相关资源，请遵循以下步骤将其删除：

1. 登录到 [ASDK 管理员门户](https://adminportal.local.azurestack.external)。
2. 单击“资源组” > “test-vm-rg” > “删除资源组”。
3. 键入 **test-vm-rg** 作为资源组名称，并单击“删除”。

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 添加 Windows Server VM Azure Stack Marketplace 项
> * 验证 VM 映像是否可用 
> * 测试 VM 映像

继续学习下一篇教程，了解如何创建 Azure Stack 产品/服务和计划。

> [!div class="nextstepaction"]
> [提供 Azure Stack IaaS 服务](asdk-offer-services.md)

