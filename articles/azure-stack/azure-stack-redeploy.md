---
title: "重新部署 Azure Stack | Microsoft Docs"
description: "重新部署 Azure Stack。"
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/24/2018
ms.date: 03/02/2018
ms.author: v-junlch
ms.openlocfilehash: 8992c72c5eaa0c74def999ecab65bcdc216f3e80
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="redeploy-azure-stack"></a>重新部署 Azure Stack
如果部署 Azure Stack 时收到错误，可以使用以下 PowerShell 命令重新运行安装程序：`.\InstallAzureStackpoc.ps1 -rerun`。 此命令会从先前失败之处重新启动 Azure Stack 安装，而无需从头开始。 如果再次收到相同的安装错误，则可能必须执行完整的重新部署以解决问题。 

若要重新部署 Azure Stack，必须从头开始进行，如下所述。

## <a name="steps-to-redeploy-azure-stack"></a>重新部署 Azure Stack 的步骤
1. 在开发工具包主机上，打开提升权限的 PowerShell 控制台 > 导航到 asdk-installer.ps1 脚本 > 运行该脚本 > 单击“重新启动”。
2. 选择基础操作系统（非 **Azure Stack**），然后单击“下一步”。
3. 开发工具包主机重新启动后，删除先前部署过程中所使用的 CloudBuilder.vhdx 文件。
4. [部署开发工具包](azure-stack-run-powershell-script.md)。

## <a name="next-steps"></a>后续步骤
[连接到 Azure Stack](azure-stack-connect-azure-stack.md)


