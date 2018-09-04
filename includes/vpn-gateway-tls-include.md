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
ms.openlocfilehash: 68839ad37f5f4299d6ab6a0516ec3fb28c68b543
ms.sourcegitcommit: e17577aca6df1a41d3ec164f33189f0435c5e060
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2018
ms.locfileid: "43252777"
---
1. 右键单击“命令提示符”并选择“以管理员身份运行”，通过提升的特权打开命令提示符窗口。
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