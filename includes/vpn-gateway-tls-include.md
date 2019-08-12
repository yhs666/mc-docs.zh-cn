---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 07/27/2018
ms.date: 01/21/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 92a6067aa8c1c04f39cb1e62853580a71634084f
ms.sourcegitcommit: 235c6c8a11af703474236c379aa6310e84ff03a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2019
ms.locfileid: "63821017"
---
1. 右键单击“命令提示符”  并选择“以管理员身份运行”  ，使用提升的权限打开命令提示符。
2. 在命令提示符窗口中运行以下命令：

   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13 /v TlsVersion /t REG_DWORD /d 0xfc0
   reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" /v DefaultSecureProtocols /t REG_DWORD /d 0xaa0
   if %PROCESSOR_ARCHITECTURE% EQU AMD64 reg add "HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" /v DefaultSecureProtocols /t REG_DWORD /d 0xaa0
   ```

3. 安装以下更新：
  
   * [KB3140245](https://www.catalog.update.microsoft.com/search.aspx?q=kb3140245)
   * [KB2977292](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2977292)

4. 重启计算机。
5. 连接到 VPN。

> [!NOTE]
> 如果运行的是旧版本的 Windows 10 (10240)，则必须设置上述注册表项。
>
