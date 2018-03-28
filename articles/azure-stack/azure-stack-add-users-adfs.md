---
title: 为 Azure Stack ADFS 添加用户 | Microsoft Docs
description: 了解如何为 Azure Stack 的 ADFS 部署添加用户
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/21/2018
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: ae9ee73a20211abd454479159d6109fe9b4107c2
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="add-users-in-the-azure-stack-development-kit"></a>将用户添加到 Azure Stack 开发工具包中

*适用于：Azure Stack 开发工具包*

若要将更多用户添加到开发工具包部署，必须使用 Microsoft 管理控制台从 Azure Stack 主机计算机将这些用户添加到 Azure Stack 开发工具包目录中。
1.  在 Azure Stack 主机计算机上，打开 Microsoft 管理控制台。
2.  单击“文件”>“添加或删除管理单元”。
3.  选择“Active Directory 用户和计算机” > “AzureStack.local” > “用户”。
4.  单击“操作” > “新建” > “用户”。
5.  在“新建对象 - 用户”窗口中，提供并确认密码
6.  单击“下一步”以最终确定值并单击“完成”创建用户。



