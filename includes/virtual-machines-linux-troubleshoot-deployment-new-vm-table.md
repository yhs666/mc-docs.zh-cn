---
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: bd4c393d03cfaed80bf44d9bb6bcb789c170f3c8
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675660"
---
下表列出了可能的 Linux 通用和专用 OS 映像的上传与捕获组合。 使用 Y 表示处理不会有任何错误的组合，使用 N 表示会出现错误的组合。下表提供了有关各种错误的原因和解决方法。

| 操作系统 | 上传专用 OS 映像 | 上传通用 OS 映像 | 捕获专用 OS 映像 | 捕获通用 OS 映像 |
| --- | --- | --- | --- | --- |
| Linux 通用 |N<sup>1</sup> |Y |N<sup>3</sup> |Y |
| Linux 专用 |Y |N<sup>2</sup> |Y |N<sup>4</sup> |

<!-- Update_Description: update meta properties, wording update -->