---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 04/01/2019
ms.date: 04/23/2019
ms.openlocfilehash: 6d4c5622df9575f6db6560d877d35affa99204b4
ms.sourcegitcommit: 9642fa6b5991ee593a326b0e5c4f4f4910f50742
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64855631"
---
如果需要配置 HTTP 代理以发出出站请求，请使用以下两个参数：

| Name | 数据类型 | 说明 |
|--|--|--|
|HTTP_PROXY|字符串|要使用的代理，例如 `http://proxy:8888`|
|HTTP_PROXY_CREDS|字符串|向代理验证身份所需的任何凭据，例如 username:password。|

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=http://190.169.1.6:3128 \
HTTP_PROXY_CREDS=jerry:123456 \
```

