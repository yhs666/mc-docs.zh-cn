---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 02/02/2018
ms.date: 12/31/2018
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 754ab266583e0d3c17f0c66ba7d3f93b69dad595
ms.sourcegitcommit: 80c59ae1174d71509b4aa64a28a98670307a5b38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735195"
---
使用 [az webapp deployment user set](https://docs.microsoft.com/cli/azure/webapp/deployment/user#set) 命令配置部署凭据。 对 Web 应用进行 FTP 和本地 Git 部署时需要此部署用户。 用户名和密码都为帐户级别。 它们与 Azure 订阅凭据不同。

在以下示例中，将 *\<username>* 和 *\<password>*（包括括号）替换为新的用户名和密码。 用户名在 Azure 中必须唯一。 密码长度必须至少为 8 个字符，其中包含以下 3 种元素中的两种：字母、数字、符号。 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

命令执行完成后你会获得一个 JSON 输出，密码显示为 `null`。 如果收到 `'Conflict'. Details: 409` 错误，请更改用户名。 如果收到 ` 'Bad Request'. Details: 400` 错误，请使用更强的密码。

只需配置此部署用户一次；之后可对所有 Azure 部署使用此用户。

> [!NOTE]
> 记录用户名和密码。 稍后需要使用它们来部署 Web 应用。
>
>
