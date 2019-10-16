---
author: IEvangelist
ms.author: v-tawe
origin.date: 06/25/2019
ms.date: 09/24/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: cdd4fcfc021b359a6e05b4241d91f442ea2f94d4
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267041"
---
如果需要配置 HTTP 代理以发出出站请求，请使用以下两个参数：

| Name | 数据类型 | 说明 |
|--|--|--|
|HTTP_PROXY|string|要使用的代理，例如 `http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|string|对代理进行身份验证所需的凭据，例如 username:password。|
|`<proxy-user>`|string|代理的用户。|
|`<proxy-password>`|string|与代理的 `<proxy-user>` 关联的密码。|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
