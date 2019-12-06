---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 06/11/2018
ms.date: 11/25/2019
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 6840c3acc2a470ef8672d51200f21abff65c98e5
ms.sourcegitcommit: e7dd37e60d0a4a9f458961b6525f99fa0e372c66
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555981"
---
如果 Python 在启动应用程序时遇到错误，只只会返回简单的错误页（例如“由于发生内部服务器错误，无法显示该页。”）。

捕获 Python 应用程序错误：

1. 在 Azure 门户中的 Web 应用中，选择“设置”  。
2. 在“设置”  选项卡上，选择“应用程序设置”  。
3. 在“应用设置”  下，输入以下键/值对：
    * 键：WSGI_LOG
    * 值：D:\home\site\wwwroot\logs.txt（输入所选文件名）

现在应可在 wwwroot 文件夹中的 logs.txt 文件中看到错误。
