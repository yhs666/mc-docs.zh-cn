---
author: IEvangelist
ms.author: v-tawe
origin.date: 06/25/2019
ms.date: 10/11/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 53e77409432e758890022c9052467933b1aee9e1
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275551"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>使用 Docker CLI 对专用容器注册表进行身份验证

可通过多种方法中的任何一种使用认知服务容器的专用容器注册表进行身份验证，但建议的方法是在 [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) 中使用命令行。

使用 [`docker login` 命令](https://docs.docker.com/engine/reference/commandline/login/)（如以下示例所示）登录到 `containerpreview.azurecr.cn`，即认知服务容器的专用容器注册表。 将 \<username\> 替换为用户名，将 \<password\> 替换为从 Azure 认知服务团队收到的凭据中提供的密码   。

```
docker login containerpreview.azurecr.cn -u <username> -p <password>
```

如果已在文本文件中保护了凭据，则可以使用 `cat` 命令将该文本文件的内容连接到 `docker login` 命令，如以下示例所示。 将 \<passwordFile\> 替换为包含密码的文本文件的路径和名称，将 \<username\> 替换为凭据中提供的用户名   。

```
cat <passwordFile> | docker login containerpreview.azurecr.cn -u <username> --password-stdin
```
