---
title: 评估 Azure Stack 开发工具包 | Microsoft Docs
description: 了解如何部署用于评估目的的 Azure Stack 开发工具包。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
origin.date: 01/22/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 55776809d78837e6b1c07477ee4eecededff0fbc
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>快速入门：评估 Azure Stack 开发工具包

*适用于：Azure Stack 开发工具包*

[Azure Stack 开发工具包](azure-stack-poc.md)是一个测试和开发环境，你可以部署它以评估和演示 Azure Stack 功能和服务。 若要启动并运行该工具包，需要准备环境硬件并运行一些脚本（这将需要几个小时）。 之后，便可以登录到管理员门户和用户门户管理 Azure Stack 和测试产品/服务。 

1. [**规划硬件、软件和网络**](azure-stack-deploy.md)。 承载开发工具包的计算机（开发工具包主机）必须满足硬件、软件和网络要求。 还必须选择是使用 Azure Active Directory 还是使用 Active Directory 联合身份验证服务 (AD FS)。 请务必在开始部署前符合这些先决条件，以便安装进程能顺利运行。 

2. [**下载并提取部署包**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit)。 可以将部署包下载到开发工具包主机或另一台计算机。 提取的部署文件占用 60 GB 的可用磁盘空间，因此使用另一台计算机有助于减少开发工具包主机的硬件要求。

3. 使用安装程序[**准备开发工具包主机**](azure-stack-run-powershell-script.md)。 此步骤完成后，开发工具包主机将启动到 Cloudbuilder.vhdx（一个包含可启动操作系统和 Azure Stack 安装文件的虚拟硬盘）。

4. 在开发工具包主机上[**部署开发工具包**](azure-stack-run-powershell-script.md)。

5. 如果 Azure Stack 部署使用 Azure Active Directory，则必须[向 Azure 注册 Azure Stack](azure-stack-register.md)，以便可以[下载 Azure Marketplace 项](azure-stack-download-azure-marketplace-item.md)到 Azure Stack。

完成这些步骤后，便具备了包含管理员门户和用户门户的开发工具包环境。 接下来，可以[连接并登录](azure-stack-connect-azure-stack.md)到门户。 然后可以开始部署资源提供程序、创建[产品/服务](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions)以及填充 Azure Stack [应用商店](azure-stack-marketplace.md)。

