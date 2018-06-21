---
title: 部署远程监视解决方案
description: 本教程演示如何从 azureiotsuite.cn 预配远程监视预配置解决方案。
services: ''
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
origin.date: 02/22/2018
ms.date: 03/12/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 2538f2089c6689e977a96c2977937d96e202825e
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
ms.locfileid: "29730809"
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>部署远程监视预配置解决方案

本教程演示如何预配远程监视预配置解决方案。 从 azureiotsuite.cn 部署解决方案。 还可以使用 CLI 部署解决方案，若要了解此选项，请参阅[从命令行部署预配置解决方案](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line)。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 配置预配置解决方案
> * 部署预配置解决方案
> * 登录到预配置解决方案

## <a name="prerequisites"></a>先决条件

需要有效的 Azure 订阅才能完成此教程。

如果没有帐户，可以创建一个试用帐户，只需几分钟即可完成。 有关详细信息，请参阅 [Azure 试用版](http://www.azure.cn/pricing/1rmb-trial/)。

## <a name="deploy-the-preconfigured-solution"></a>部署预配置解决方案

将预配置解决方案部署到 Azure 订阅之前，必须选择一些配置选项：

1. 使用 Azure 帐户凭据登录 [azureiotsuite.cn](https://www.azureiotsuite.cn)，并单击“+”创建新的解决方案：

    ![创建新的解决方案](media/iot-suite-remote-monitoring-deploy/createnewsolution.png)

1. 单击“远程监视预览”磁贴上的“选择”。

    ![选择远程监视](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. 在“创建远程监视解决方案”页上，为远程监视预配置解决方案输入**解决方案名称**。

1. 选择“基本”或“标准”部署。 如果部署解决方案的目的是要了解其工作原理或运行演示，请选择“基本”选项，使成本降到最低。

1. 选择 **Java** 或 **.NET** 作为语言。 所有微服务都可用作 Java 或 .NET 实现。

1. 有关配置选择的详细信息，请查看“解决方案详细信息”面板。

1. 选择要用于预配解决方案的“订阅”和“区域”。

1. 单击“创建解决方案”  开始预配过程。 此过程通常需要数分钟的运行时间：

    ![远程监视解决方案详细信息](media/iot-suite-remote-monitoring-deploy/createform.png)

有关故障排除的信息，请参阅 GitHub 存储库中的[部署失败时该如何操作](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails)。

## <a name="sign-in-to-the-preconfigured-solution"></a>登录到预配置解决方案

预配过程完成后，可以登录到远程监视预配置解决方案。

1. 在“预配解决方案”页上，选择新的远程监视解决方案：

    ![选择新的解决方案](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. 可以在显示的面板中查看有关远程监视解决方案的信息。 选择“解决方案仪表板”连接到远程监视解决方案。

    > [!NOTE]
    > 使用完远程监视解决方案后，可以将其从此面板中删除。

    ![解决方案面板](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. 远程监视解决方案仪表板将显示在浏览器中。

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 配置预配置解决方案
> * 部署预配置解决方案
> * 登录到预配置解决方案

现在已部署远程监视解决方案，下一步是[浏览解决方案仪表板的功能](./iot-suite-remote-monitoring-explore.md)。

<!-- Next tutorials in the sequence -->