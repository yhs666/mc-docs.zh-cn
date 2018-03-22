---
title: 使用 Azure Stack 门户部署模板 | Microsoft Docs
description: 了解如何使用 Azure Stack 门户部署模板。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: e8e85b4808c4b058672c723646c0bfb898a0fdf1
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>使用 Azure Stack 门户部署模板

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用门户可将 Azure 资源管理器模板部署到 Azure Stack 开发工具包。

资源管理器模板可通过单个协调操作部署和预配应用程序的所有资源。

1. 登录到门户，依次单击“新建”、“自定义”，然后单击“模板部署”。
2. 单击“编辑模板”，然后将 JSON 模板代码粘贴到边栏选项卡中，，再单击“保存”。
3. 单击“编辑参数”，为列出的参数键入值，然后单击“确定”。
4. 单击“订阅”，选择要使用的订阅，然后单击“确定”。
5. 单击“资源组”，选择现有资源组或创建新资源组，然后单击“确定”。
6. 单击“创建”。 仪表板上的新磁贴会跟踪模板部署的进度。

## <a name="next-steps"></a>后续步骤
[通过 PowerShell 部署模板](azure-stack-deploy-template-powershell.md)


