---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 055655edce146af9b8a20f659b04ba89f7260d7e
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993104"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

若要创建一个共享访问策略以授予“服务连接”和“注册表读取”权限，同时获取该策略的连接字符串，请执行以下步骤：  

1. 在 [Azure 门户](https://portal.azure.cn)中打开 IoT 中心。 若要转到 IoT 中心，最简单的方法是选择“资源组”，接着选择 IoT 中心所在的资源组，然后从资源列表中选择该 IoT 中心。 

2. 在 IoT 中心的左侧窗格中，选择“共享访问策略”  。

3. 从策略列表上方的顶部菜单中选择“添加”  。

4. 在“添加共享访问策略”窗格中，为策略输入一个说明性名称，例如 *serviceAndRegistryRead*。  在“权限”下选择“注册表读取”和“服务连接”，然后选择“创建”   。  

    ![显示如何添加新的共享访问策略](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

5. 回到“共享访问策略”窗格，从策略列表中选择新的策略  。

6. 在“共享访问密钥”  下，选择“连接字符串 - 主密钥”  所对应的“复制”图标并保存该值。

    ![显示如何检索连接字符串](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

有关 IoT 中心共享访问策略和权限的详细信息，请参阅[访问控制和权限](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)。
