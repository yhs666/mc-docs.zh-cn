---
title: Azure Stack 快速入门 - 创建 Windows 虚拟机
description: Azure Stack 快速入门 - 使用门户创建 Windows VM
services: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
origin.date: 09/15/2017
ms.date: 03/09/2018
ms.author: v-junlch
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: cc825f1a950ff435335415109f4879b044055586
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-windows-virtual-machine-with-the-azure-stack-portal"></a>使用 Azure Stack 门户创建 Windows 虚拟机

可以使用 Azure Stack 门户创建 Windows 虚拟机。 该门户是基于浏览器的用户界面，可在其中创建、配置和管理资源。

## <a name="sign-in-to-the-azure-stack-portal"></a>登录到 Azure Stack 门户

登录到 Azure Stack 门户。 Azure Stack 门户的地址取决于连接到的 Azure Stack 产品：

- 对于 Azure Stack 开发工具包 (ASDK)，请转到：https://portal.local.azurestack.external。
- 对于 Azure Stack 集成系统，请转到 Azure Stack 运营商提供的 URL。

## <a name="create-a-virtual-machine"></a>创建虚拟机

1. 单击“新建” > “计算” > “Windows Server 2016 Datacenter Eval” > “创建”。 如果未看到“Windows Server 2016 Datacenter Eval”项，请联系 Azure Stack 运营商。 根据[将 Windows Server 2016 VM 映像添加到 Azure Stack Marketplace](../azure-stack-add-default-image.md) 一文中所述，请求运营商将此映像添加到 Marketplace。 
    ![](./media/azure-stack-quick-windows-portal/image01.png)
2. 在“基本信息”下，键入**名称**、**用户名**和**密码**。 选择“订阅”。 创建一个**资源组**或选择现有资源组，选择一个**位置**，然后单击“确定”。

    ![](./media/azure-stack-quick-windows-portal/image02.png)
3. 在“选择大小”下，单击“D1 标准” > “选择”。
    ![](./media/azure-stack-quick-windows-portal/image03.png)
4. 在“设置”下，接受默认值，然后单击“确定”。
    ![](./media/azure-stack-quick-windows-portal/image04.png)
5. 在“摘要”下，单击“确定”创建虚拟机。 
    ![](./media/azure-stack-quick-windows-portal/image05.png)
6. 若要查看新虚拟机，请单击“所有资源”，然后搜索该虚拟机并单击其名称。
    ![](./media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>清理资源

不再需要该虚拟机时，可以删除资源组、虚拟机和所有相关资源。 为此，请从虚拟机页中选择该资源组，并单击“删除”。

## <a name="next-steps"></a>后续步骤
在本快速入门中，我们部署了一个简单的 Windows 虚拟机。 有关 Azure Stack 虚拟机的详细信息，请转到 [Azure Stack 中虚拟机的注意事项](azure-stack-vm-considerations.md)。

