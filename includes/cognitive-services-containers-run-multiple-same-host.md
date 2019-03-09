---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/21/2019
ms.openlocfilehash: ae7dafb73058123e3ee4f592cf9d9dbcd674753c
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204209"
---
### <a name="running-multiple-containers-on-the-same-host"></a>在同一主机上运行多个容器

如果打算使用所公开的端口运行多个容器，请确保使用不同的端口运行每个容器。 例如，在端口 5000 上运行第一个容器，在端口 5001 上运行第二个容器。

将 `<container-registry>` 和 `<container-name>` 替换为你使用的容器的值。 它们并非必须是同一容器。 你可以让“人脸”容器和 LUIS 容器一起在主机上运行，也可以运行多个“人脸”容器。 

在端口 5000 上运行第一个容器。 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

在端口 5001 上运行第二个容器。


```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

每个后续容器都应当位于不同的端口上。 
