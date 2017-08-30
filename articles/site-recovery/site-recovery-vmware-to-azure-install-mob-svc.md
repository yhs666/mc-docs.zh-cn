---
title: "安装移动服务（VMware/物理机到 Azure）| Azure"
description: "了解如何安装移动服务代理来保护本地计算机。"
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
origin.date: 06/29/2017
ms.date: 
ms.author: v-johch
ms.openlocfilehash: 4b1d70823c9308b81c1b39e8fe8f67c79c914a8a
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="install-mobility-service-vmware-or-physical-to-azure"></a>安装移动服务（VMware/物理到 Azure）
Azure Site Recovery 移动服务可以捕获计算机上的数据写入，并将其转发到进程服务器。 将移动服务部署到要复制到 Azure 的每台计算机（VMware VM 或物理服务器）上。 可以使用以下方法将移动服务部署到需要保护的服务器上：

* [使用 System Center Configuration Manager 等软件部署工具安装移动服务](site-recovery-install-mobility-service-using-sccm.md)
* [使用 Azure 自动化和 Desired State Configuration (Automation DSC) 安装移动服务](site-recovery-automate-mobility-service-install.md)
* [使用图形用户界面 (GUI) 手动安装移动服务](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [在命令提示符下手动安装移动服务](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [通过推送安装从 Azure Site Recovery 安装移动服务](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)

>[!IMPORTANT]
> 从版本 9.7.0.0 开始，在 Windows 虚拟机 (VM) 上，移动服务安装程序还会安装最新可用的 [Azure VM 代理](../virtual-machines/windows/extensions-features.md#azure-vm-agent)。 这可确保当计算机故障转移到 Azure 时，它满足使用任何 VM 扩展的代理安装先决条件。

## <a name="prerequisites"></a>先决条件
在服务器上手动安装移动服务之前，请先完成以下必要步骤：
1. 登录到配置服务器，并以管理员身份打开“命令提示符”窗口。
2. 将目录切换到 bin 文件夹，并创建一个密码文件：

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. 将密码文件存储在安全位置中。 在移动服务安装过程中会用到该文件。
4. 适用于所有支持的操作系统的移动服务安装程序均位于 %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository 文件夹。

### <a name="mobility-service-installer-to-operating-system-mapping"></a>移动服务安装程序到操作系统的映射

| 安装程序文件模板名称| 操作系统 |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1（64 位） </br> Windows Server 2012（64 位） </br> Windows Server 2012 R2（64 位） |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4、6.5、6.6、6.7、6.8（仅限 64 位） </br> CentOS 6.4、6.5、6.6、6.7、6.8（仅限 64 位） |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1、7.2（仅限 64 位） </br> CentOS 7.0、7.1、7.2（仅限 64 位）</br> CentOs 7.3（仅适用于迁移） |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3（仅限 64 位）|
|Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4（仅限 64 位）|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4、6.5（仅限 64 位）|
|Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04（仅限 64 位）|

## <a name="install-mobility-service-manually-by-using-the-gui"></a>使用 GUI 手动安装移动服务

>[!IMPORTANT]
> 如果要使用**配置服务器**将 **Azure IaaS 虚拟机**从一个 Azure 订阅/区域复制到另一个 Azure 订阅/区域，则**使用基于命令行的安装**方法

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>在命令提示符下手动安装移动服务

### <a name="command-line-installation-on-a-windows-computer"></a>Windows 计算机上基于命令行的安装
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Linux 计算机上基于命令行的安装
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]

## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>在 Azure Site Recovery 中使用推送安装安装移动服务
若要使用 Site Recovery 执行移动服务的推送安装，所有目标计算机都必须满足以下先决条件。

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]

> [!NOTE]
安装移动服务后，在 Azure 门户中选择“复制”按钮以开始保护这些 VM。

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>卸载 Windows Server 计算机上的移动服务
通过以下方法之一卸载 Windows Server 计算机上的移动服务。

### <a name="uninstall-by-using-the-gui"></a>使用 GUI 卸载
1. 在控制面板中，选择“程序” 。
2. 选择“Azure Site Recovery 移动服务/主目标服务器”，并单击“卸载”。

### <a name="uninstall-at-a-command-prompt"></a>在命令提示符下卸载
1. 以管理员身份打开“命令提示符”窗口。
2. 若要卸载移动服务，请运行以下命令：

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>卸载 Linux 计算机上的移动服务
1. 以 **root** 用户身份在 Linux 服务器上登录。
2. 在终端中转到 /user/local/ASR。
3. 若要卸载移动服务，请运行以下命令：

```
uninstall.sh -Y
```