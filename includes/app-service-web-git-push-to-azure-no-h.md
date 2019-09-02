---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 03/18/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: a18d510e02c19f9c5d75c8a3c92cac7a6136b4f4
ms.sourcegitcommit: 0ccbf718e90bc4e374df83b1460585d3b17239ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57350461"
---
回到本地终端窗口，将 Azure 远程功能添加到本地 Git 存储库。 将 _&lt;deploymentLocalGitUrl-from-create-step>_ 替换为在[创建 Web 应用](#create-a-web-app)中保存的 Git 远程 URL。

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

使用以下命令推送到 Azure 远程功能以部署应用。 当 Git 凭据管理器提示输入凭据时，请确保输入在配置部署用户中创建的凭据，而不是用于登录到 Azure 门户的凭据。

```bash
git push azure master
```

此命令可能需要花费几分钟时间运行。 运行时，该命令会显示类似于以下示例的信息：
