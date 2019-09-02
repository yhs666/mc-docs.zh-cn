---
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 4900dc0c5754a0263b7e099992cb0eb6269e6d61
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676117"
---
下表列出了可能的 Windows 通用和专用OS 映像的上传和捕获组合。 使用 Y 表示处理不会有任何错误的组合，使用 N 表示会出现错误的组合。下表提供了有关各种错误的原因和解决方法。

| 操作系统 | 上传专用 OS 映像 | 上传通用 OS 映像 | 捕获专用 OS 映像 | 捕获通用 OS 映像 |
| --- | --- | --- | --- | --- |
| Windows 通用 |N<sup>1</sup> |Y |N<sup>3</sup> |Y |
| Windows 专用 |Y |N<sup>2</sup> |Y |N<sup>4</sup> |

<!--Update_Description: wording update, update link-->