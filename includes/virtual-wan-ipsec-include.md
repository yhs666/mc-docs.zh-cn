---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 05/28/2019
ms.date: 06/28/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 624eb4a92b67aa6311062d7589fd8b7b6cffa7d4
ms.sourcegitcommit: ab87d30f4435c3b7c03f7edd33c9f374b7fe88c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540089"
---
### <a name="initiator"></a>发起程序

以下部分列出了 Azure 作为隧道发起程序时支持的策略组合。

**阶段 1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2

**阶段 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE

### <a name="responder"></a>响应方

以下部分列出了 Azure 作为隧道响应方时支持的策略组合。

**阶段 1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2

**阶段 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* AES_256, SHA_1, PFS_1
* AES_256, SHA_1, PFS_2
* AES_256, SHA_1, PFS_14
* AES_128, SHA_1, PFS_1
* AES_128, SHA_1, PFS_2
* AES_128, SHA_1, PFS_14
* AES_256, SHA_256, PFS_1
* AES_256, SHA_256, PFS_2
* AES_256, SHA_256, PFS_14
* AES_256, SHA_1, PFS_24
* AES_256, SHA_256, PFS_24
* AES_128, SHA_256, PFS_NONE
* AES_128, SHA_256, PFS_1
* AES_128, SHA_256, PFS_2
* AES_128, SHA_256, PFS_14

<!--Update_Description: new articles on virtual wan ipsec -->
<!--ms.date: 07/01/2019-->