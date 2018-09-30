---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 07/27/2018
ms.date: 09/02/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 4f8149ce8f43603b7656133987d2a493a3558ca7
ms.sourcegitcommit: 04071a6ddf4e969464d815214d6fdd9813c5c5a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47426488"
---
1. 右键单击“命令提示符”并选择“以管理员身份运行”，使用提升的权限打开命令提示符。
2. 在命令提示符窗口中运行以下命令：

  ```
  reg add HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13 /v TlsVersion /t REG_DWORD /d 0xfc0
  reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" /v DefaultSecureProtocols /t REG_DWORD /d 0xaa0
  if %PROCESSOR_ARCHITECTURE% EQU AMD64 reg add "HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" /v DefaultSecureProtocols /t REG_DWORD /d 0xaa0
  ```

3. 安装以下更新：
  
  * [KB3140245](https://www.catalog.update.microsoft.com/search.aspx?q=kb3140245)
  * [KB2977292](https://www.microsoft.com/en-us/download/details.aspx?id=44342)

4. 重启计算机。
5. 连接到 VPN。