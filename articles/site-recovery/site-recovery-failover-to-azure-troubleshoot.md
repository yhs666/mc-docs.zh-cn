---
title: 对故障转移到 Azure 的故障进行排除 | Azure
description: 本指南介绍如何解决在故障转移到 Azure 中的常见错误
services: site-recovery
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 09/11/2018
ms.date: 09/24/2018
ms.author: v-yeche
ms.openlocfilehash: 8c5eebd9b81c505d3f38b18abd5807307b1b6c5e
ms.sourcegitcommit: fb353628b721f124b82a30155ca5f78bbb7fa60b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47424054"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>解决从虚拟机到 Azure 的故障转移时出现的错误

在执行从虚拟机到 Azure 的故障转移时，可能会收到以下错误之一。 若要解决错误，请为每个错误条件使用所述步骤。

## <a name="failover-failed-with-error-id-28031"></a>故障转移失败，错误 ID 为 28031

Site Recovery 无法在 Azure 中创建故障转移的虚拟机。 以下其中一个原因也可能导致此情况的发生：

* 创建虚拟机的可用配额不足：你可以通过转到“订阅” -> “使用情况 + 配额”来检查可用配额。 可以打开 [新的支持请求](http://aka.ms/getazuresupport) 来增加此配额。

* 尝试在同一个可用性集中故障转移不同大小系列的虚拟机。 确保在同一个可用性集中选择相同大小系列的所有虚拟机。 可以转到虚拟机的“计算和网络”设置来更改大小，然后重试故障转移。

* 订阅上有一个阻止创建虚拟机的策略。 请更改此策略以允许创建虚拟机，然后重试故障转移。

## <a name="failover-failed-with-error-id-28092"></a>故障转移失败，错误 ID 为 28092

Site Recovery 无法为故障转移的虚拟机创建网络接口。 请确保订阅中有足够的配额来创建网络接口。 可以通过转到“订阅” -> “使用情况 + 配额”来检查可用配额。 可以打开 [新的支持请求](http://aka.ms/getazuresupport) 来增加此配额。 如果你拥有足够的配额，则这可能是一个间歇性的问题，请重试该操作。 如果即使在重试后问题仍然存在，请在本文档结尾处留下注释。  

## <a name="failover-failed-with-error-id-70038"></a>故障转移失败，错误 ID 为 70038

Site Recovery 无法在 Azure 中创建故障转移的经典虚拟机。 这可能是因为：

* 创建虚拟机所需的其中一个资源（如虚拟网络）不存在。 在虚拟机的“计算和网络”设置下创建虚拟网络，或者将设置修改为已经存在的虚拟网络，然后重试故障转移。

## <a name="unable-to-connectrdpssh---vm-connect-button-grayed-out"></a>无法连接/RDP/SSH - VM“连接”按钮灰显

在 Azure 中，如果故障转移后的 VM 上的“连接”按钮灰显，并且你未通过快速路由或站点到站点 VPN 连接来连接到 Azure，则执行以下操作：

1. 转到“虚拟机” > “网络”，单击所需网络接口的名称。  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. 导航到“IP 配置”，然后单击所需 IP 配置的名称字段。 ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. 若要启用公共 IP 地址，请单击“启用”。 ![启用 IP](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. 单击“配置所需设置” > “新建”。 ![新建](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. 输入公共地址的名称，选择“分配”的默认选项，然后单击“确定”。
    <!-- Not Available on **SKU**-->
6. 现在，单击“保存”以保存所做的更改。
7. 关闭面板并导航到虚拟机的“概述”部分以进行连接/通过 RDP 连接。

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>无法连接/RDP/SSH - VM“连接”按钮不可用

在 Azure 中，如果故障转移后的 VM 上的“连接”按钮可用（未灰显），则在虚拟机上检查**启动诊断**并检查是否存在[此文章](../virtual-machines/windows/boot-diagnostics.md)中列出的错误。

1. 如果虚拟机尚未启动，请尝试故障转移到以前的恢复点。
2. 如果虚拟机中的应用程序未启动，请尝试故障转移到应用一致的恢复点。
3. 如果虚拟机已加入域，请确保域控制器正确运行。 可以按照下面给出的步骤执行此操作：

    a. 在同一网络中创建一台新虚拟机。

    b. 确保它能够加入到预期要在其中应启动故障转移的虚拟机的同一域。

    c. 如果域控制器未正常工作，请尝试使用本地管理员帐户登录到故障转移的虚拟机。
4. 如果使用自定义 DNS 服务器，请确保可以访问该服务器。 可以按照下面给出的步骤执行此操作：

    a. 在同一网络中创建一台新虚拟机并且

    b. 检查虚拟机是否能够使用自定义 DNS 服务器执行名称解析

>[!Note]
>启用除“启动诊断”以外的任何设置，都需要在故障转移之前在虚拟机中安装 Azure VM 代理

## <a name="unexpected-shutdown-message-event-id-6008"></a>意外的关闭消息（事件 ID 6008）

在故障转移后启动 Windows VM 时，如果在恢复后的 VM 上收到意外的关闭消息，则表明在用于故障转移的恢复点中未捕获 VM 关闭状态。 当恢复到 VM 未完全关闭的时间点时会发生此情况。

通常情况下不需要担心此消息，对于计划外故障转移，通常可以忽略此消息。 对于计划内故障转移，请确保 VM 在故障转移前正确关闭，并留出足够的时间供用来将挂起的本地复制数据发送到 Azure。 然后，使用**故障转移**屏幕上的[最新](site-recovery-failover.md#run-a-failover)选项，以便将 Azure 上挂起的任何数据都处理到恢复点中，然后将该恢复点用于 VM 故障转移。

## <a name="retaining-drive-letter-after-failover"></a>在故障转移后保留驱动器号
若要在故障转移后保留虚拟机上的驱动器号，可将本地虚拟机的**SAN 策略**设置为**OnlineAll**。 [了解详细信息](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure)。

## <a name="next-steps"></a>后续步骤
- 对[到 Windows VM 的 RDP 连接](../virtual-machines/windows/troubleshoot-rdp-connection.md)进行故障排除
- 对[到 Linux VM 的 SSH 连接](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)进行故障排除

如需更多帮助，请在 [Site Recovery 论坛](https://www.azure.cn/support/contact/)提出询问。
<!--Notice: Remove   or leave a comment at the end of this document-->
<!--Notice: Remove   We have an active community that should be able to assist you.-->

<!--Update_Description: update meta properties, wording update, update link -->