---
title: 在 Azure Stack 上删除 SQL 资源提供程序 | Microsoft Docs
description: 了解如何从 Azure Stack 部署中删除 SQL 资源提供程序。
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/11/2018
ms.date: 06/26/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: c0ab2ad7bf80a88fd5a6579811fa3dcad457096c
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027219"
---
# <a name="removing-the-mysql-resource-provider"></a>删除 MySQL 资源提供程序  
在删除 SQL 资源提供程序之前，必须先删除所有依赖项。

## <a name="remove-the-mysql-resource-provider"></a>删除 MySQL 资源提供程序 

1. 确认已删除所有现有的 SQL 资源提供程序依赖项。

    > [!NOTE]
    > 即使依赖资源当前正在使用 SQL 资源提供程序，也将继续卸载该资源提供程序。 
  
2. 确保已保留针对此 SQL 资源提供程序适配器版本下载的原始部署包。
3. 使用以下参数重新运行部署脚本：
    - 使用 -Uninstall 参数
    - 特权终结点的 IP 地址或 DNS 名称。
    - 访问特权终结点时所需的云管理员凭据。
    - Azure Stack 服务管理员帐户的凭据。 使用部署 Azure Stack 时所用的相同凭据。

## <a name="next-steps"></a>后续步骤
[提供应用服务作为 PaaS](azure-stack-app-service-overview.md)

<!-- Update_Description: wording update -->