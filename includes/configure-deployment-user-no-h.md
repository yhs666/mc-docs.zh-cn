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
ms.openlocfilehash: 8b5d7451f8f891536b990a0af1bb53b7997212f8
ms.sourcegitcommit: b8aa5d05ef46f1db2df4f2653cdd8d150e847113
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906292"
---
使用 [az webapp deployment user set](https://docs.microsoft.com/cli/azure/webapp/deployment/user#set) 命令配置部署凭据。 对 Web 应用进行 FTP 和本地 Git 部署时需要此部署用户。 用户名和密码都为帐户级别。 它们与 Azure 订阅凭据不同。

在以下示例中，将 *\<username>* 和 *\<password>*（包括括号）替换为新的用户名和密码。 用户名在 Azure 中必须唯一。 密码长度必须至少为 8 个字符，其中包含以下 3 种元素中的两种：字母、数字、符号。 

```azurecli
az webapp deployment user set --user-name <username> --password <password>
```

命令执行完成后你会获得一个 JSON 输出，密码显示为 `null`。 如果收到 `'Conflict'. Details: 409` 错误，请更改用户名。 如果收到 ` 'Bad Request'. Details: 400` 错误，请使用更强的密码。

只需配置此部署用户一次；之后可对所有 Azure 部署使用此用户。

> [!NOTE]
> 记录用户名和密码。 稍后需要使用它们来部署 Web 应用。
>
>
