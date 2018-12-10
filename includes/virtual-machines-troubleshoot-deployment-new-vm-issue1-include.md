---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 31cfa4b21b0e7cfeb12c04caa9fe78ebb84a7a9a
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675663"
---
## <a name="issue-custom-image-provisioning-errors"></a>问题：自定义映像；预配错误
当你上传或捕获用作专用 VM 映像的通用化 VM 映像时，将发生预配错误，反之亦然。 前者会导致预配超时错误，后者会导致预配失败。 若要部署自定义映像且不出错，必须确保在捕获过程中映像类型不会更改。

下表列出了通用化和专用映像的可能组合、你会遇到的错误类型，以及需要执行哪些操作来解决错误。

<!--Update_Description: wording update, update link-->