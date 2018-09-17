---
title: include 文件
description: include 文件
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: include
origin.date: 06/28/2018
ms.date: 07/23/2018
ms.author: v-yeche
ms.openlocfilehash: 0f2539e28c9a772dc3a8b3033d076760edf26b00
ms.sourcegitcommit: 96d06c506983906a92ff90a5f67199f8f7e10996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2018
ms.locfileid: "45637034"
---
| Name | 商用 URL | 说明 |
|---|---|---|---|
| Azure AD | ``login.partner.microsoftonline.cn`` | 用于使用 AAD 的访问控制和标识管理 |
| 备份 | ``*.backup.windowsazure.cn`` | 用于复制数据传输和协调 |
| 复制 | ``*.hypervrecoverymanager.windowsazure.cn`` | 用于复制管理操作和协调 |
| 存储 | ``*.blob.core.chinacloudapi.cn`` | 用于访问存储所复制数据的存储帐户 |
| 遥测（可选） | ``dc.services.visualstudio.com`` | | 用于遥测 |

``time.nist.gov`` 和 ``time.windows.com`` 用于检查所有部署中的系统时间与全球时间之间的时间同步。