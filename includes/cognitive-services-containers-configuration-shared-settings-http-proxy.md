---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 01/22/2019
ms.date: 02/21/2019
ms.openlocfilehash: 209f79af0ed0ef5cec1d919d8fe82cf719abed59
ms.sourcegitcommit: 3ae99942621d28a8439ca1e7a7905caa5a3a10f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2019
ms.locfileid: "56582783"
---
如果需要配置 HTTP 代理以发出出站请求，请使用以下两个参数：

| Name | 数据类型 | 说明 |
|--|--|--|
|HTTP_PROXY|字符串|要使用的代理，例如 http://proxy:8888|
|HTTP_PROXY_CREDS|字符串|向代理验证身份所需的任何凭据，例如 username:password。|

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=http://190.169.1.6:3128 \
HTTP_PROXY_CREDS=jerry:123456 \
Logging:Disk:LogLevel=Debug Logging:Disk:Format=json
```

<!-- ms.date: 02/21/2019 -->