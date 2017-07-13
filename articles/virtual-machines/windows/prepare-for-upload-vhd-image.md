---
title: "准备好要上传到 Azure 的 Windows VHD | Azure"
description: "上传到 Azure 前如何准备 Windows VHD 或 VHDX"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 05/26/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: fc1d807095489d2e9330fe63de0318af0cb0c62d
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 准备好要上传到 Azure 的 Windows VHD 或 VHDX
<a id="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure" class="xliff"></a>
若要将 Windows VM 从本地上传到 Azure，必须准备好虚拟硬盘（VHD 或 VHDX）。 Azure 仅支持采用 VHD 文件格式且磁盘大小固定的第 1 代虚拟机。 VHD 允许的最大大小为 1,023 GB。 可将第 1 代虚拟机从 VHDX 转换成 VHD 文件格式，并从动态扩展转换成固定大小磁盘。 但无法更改虚拟机的代次。 有关详细信息，请参阅 [是否应在 HYPER-V 中创建第 1 代或第 2 代虚拟机？](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)

## 将虚拟磁盘转换为 VHD 和固定大小的磁盘
<a id="convert-the-virtual-disk-to-vhd-and-fixed-size-disk" class="xliff"></a> 
如果要将虚拟磁盘转换为 Azure 所需的格式，请使用本部分中所述的某种方法。 运行虚拟磁盘转换过程之前先备份 VM，并确保 Windows VHD 在本地服务器上正常运行。 尝试转换磁盘或将其上传到 Azure 之前，先解决 VM 本身内部的所有错误。

转换磁盘后，创建一个使用转换磁盘的 VM。 启动并登录到 VM，准备好 VM 进行上传。

### 使用 Hyper-V 管理器转换磁盘
<a id="convert-disk-using-hyper-v-manager" class="xliff"></a>
1. 打开 Hyper-V 管理器，在左侧选择本地计算机。 在本地计算机上面的菜单中，单击“操作” > “编辑磁盘”。
2. 在“查找虚拟硬盘”屏幕上，浏览到并选择虚拟磁盘。
3. 在“选择操作”屏幕上，选择“转换”和“下一步”。
4. 如果需要从 VHDX 进行转换，请选择“VHD”，然后单击“下一步”
5. 如果需要从动态扩展磁盘进行转换，请选择“固定大小”，然后单击“下一步”
6. 浏览到并选择要保存新 VHD 文件的路径。
7. 单击“完成”关闭屏幕。

### 使用 PowerShell 转换磁盘
<a id="convert-disk-using-powershell" class="xliff"></a>
可通过在 Windows PowerShell 中使用 [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet 转换虚拟磁盘。 启动 PowerShell 时选择“以管理员身份运行”  。 下面的示例演示了如何从 VHDX 转换成 VHD，以及如何从动态扩展磁盘转换为固定大小磁盘：

```powershell
Convert-VHD -Path c:\test\MY-VM.vhdx -DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
将 -Path 的值替换为要转换的虚拟硬盘的路径，并将 -DestinationPath 的值替换为已转换磁盘的新路径和名称。

### 从 VMware VMDK 磁盘格式转换
<a id="convert-from-vmware-vmdk-disk-format" class="xliff"></a>
如果有 [VMDK 文件格式](https://en.wikipedia.org/wiki/VMDK)的 Windows VM 映像，可以使用 [Microsoft 虚拟机转换器](https://www.microsoft.com/download/details.aspx?id=42497)将其转换为 VHD。 有关详细信息，请阅读博客 [How to Convert a VMware VMDK to Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx)（如何将 VMware VMDK 转换为 Hyper-V VHD）。

## 设置 Azure 的 Windows 配置
<a id="set-windows-configurations-for-azure" class="xliff"></a>

在计划上传到 Azure 的虚拟机上，以[管理权限](https://technet.microsoft.com/library/cc947813.aspx)在命令提示符窗口中运行以下所有命令。

1. 删除路由表中的所有静态持久性路由：

   * 若要查看路由表，请在命令提示符窗口中运行 `route print` 。
   * 请查看 **持久性路由** 部分。 如果有持久性路由，请使用“路由删除”将它删除。
2. 删除 WinHTTP 代理：

    ```CMD
    netsh winhttp reset proxy
    ```
3. 将磁盘 SAN 策略设置为 [Onlineall](https://technet.microsoft.com/library/gg252636.aspx)。 

    ```CMD
    diskpart 
    san policy=onlineall
    exit
    ```

4. 为 Windows 设置协调世界时 (UTC) 时间，并将 Windows 时间 (w32time) 服务的启动类型设置为“自动”：

    ```CMD
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    ```
    ```CMD
    sc config w32time start= auto
    ```

## 将服务启动设置为 Windows 默认值
<a id="set-services-startup-to-windows-default-values" class="xliff"></a>
确保下面的每个 Windows 服务均设置为 **Windows 默认值**。 若要重置启动设置，请运行以下命令：

```CMD
sc config bfe start= auto

sc config dcomlaunch start= auto

sc config dhcp start= auto

sc config dnscache start= auto

sc config IKEEXT start= auto

sc config iphlpsvc start= auto

sc config PolicyAgent start= demand

sc config LSM start= auto

sc config netlogon start= demand

sc config netman start= demand

sc config NcaSvc start= demand

sc config netprofm start= demand

sc config NlaSvc start= auto

sc config nsi start= auto

sc config RpcSs start= auto

sc config RpcEptMapper start= auto

sc config termService start= demand

sc config MpsSvc start= auto

sc config WinHttpAutoProxySvc start= demand

sc config LanmanWorkstation start= auto

sc config RemoteRegistry start= auto
```

## 更新远程桌面注册表设置
<a id="update-remote-desktop-registry-settings" class="xliff"></a>
1. 如果有任何绑定到远程桌面协议 (RDP) 侦听程序的自签名证书，请将其删除：

    ```CMD
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash"
    ```

    有关配置 RDP 侦听器证书的详细信息，请参阅 [Listener Certificate Configurations in Windows Server ](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)
2. 配置 RDP 服务的 [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) 值：

    ```CMD
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f
    ```
    ```CMD
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f
    ```
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```
3. 配置 RDP 服务的身份验证模式：

    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f
   ```
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f
   ```
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```
4. 通过在注册表中添加以下子项来启用 RDP 服务：

    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```

## 配置 Windows 防火墙规则
<a id="configure-windows-firewall-rules" class="xliff"></a>
1. 在 PowerShell 中运行以下命令，允许 WinRM 通过三个防火墙配置文件（“域”、“专用”和“公共”）并启用 PowerShell 远程服务：

   ```powershell
   Enable-PSRemoting -force
   ```
2. 在命令提示符窗口中运行以下命令，确保已部署以下来宾操作系统防火墙规则：

   * 入站

   ```CMD
   netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

   netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```

   * 入站和出站

   ```CMD
   netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

   netsh advfirewall firewall set rule group="Core Networking" new enable=yes
   ```

   * 出站

   ```CMD
   netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

   netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
   ```

## 验证确保 VM 运行正常、安全并可使用 RDP 访问
<a id="verify-vm-is-healthy-secure-and-accessible-with-rdp" class="xliff"></a> 
1. 在命令提示符窗口中，运行 `winmgmt /verifyrepository`，确认 Windows Management Instrumentation (WMI) 存储库是否一致。 如果存储库已损坏，请参阅博客文章 [WMI：存储库是否损坏？](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not)
2. 设置引导配置数据 (BCD) 设置：

   ```CMD
   bcdedit /set {bootmgr} integrityservices enable

   bcdedit /set {default} device partition=C:

   bcdedit /set {default} integrityservices enable

   bcdedit /set {default} recoveryenabled Off

   bcdedit /set {default} osdevice partition=C:

   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. 删除所有其他传输驱动程序接口筛选器，例如分析 TCP 数据包的软件。
4. 为了确保磁盘正常运行且一致，请在命令提示符窗口中运行 `CHKDSK /f` 命令。 键入“Y”以安排检查并重启 VM。
5. 卸载与物理组件相关的任何其他第三方软件和驱动程序，或卸载任何其他虚拟化技术。
6. 确保第三方应用程序未使用端口 3389。 此端口用于 Azure 中的 RDP 服务。 可在命令提示符窗口中运行 `netstat -anob` ，查看应用程序使用的端口。
7. 如果要上传的 Windows VHD 是域控制器，请遵循 [这些附加步骤](https://support.microsoft.com/kb/2904015) 来准备磁盘。
8. 重新启动 VM，确保 Windows 仍可正常运行，并可以使用 RDP 连接来访问它。
9. 重置当前本地管理员密码，确保可以使用此帐户通过 RDP 连接登录 Windows。 此访问权限由“允许通过远程桌面服务登录”组策略对象控制。 可以在本地组策略编辑器中“计算机配置\Windows 设置\安全设置\本地策略\用户权限分配”下查看此对象。

## 安装 Windows 更新
<a id="install-windows-updates" class="xliff"></a>
安装最新的 Windows 更新。 如果无法安装，请确保安装以下更新：

* [KB3137061](https://support.microsoft.com/kb/3137061)若发生网络中断和数据损坏，Azure VM 无法恢复
* [KB3115224](https://support.microsoft.com/kb/3115224)：对在 Windows Server 2012 R2 或 Windows Server 2012 主机上运行的 VM 所做的可靠性改进
* [KB3140410](https://support.microsoft.com/kb/3140410)：MS16-031：Microsoft Windows 安全更新，解决权限提升过程中的问题：2016 年 3 月 8 日
* [KB3063075](https://support.microsoft.com/kb/3063075)：在 Azure 中运行 Windows Server 2012 R2 虚拟机时记录了许多 ID 129 事件
* [KB3114025](https://support.microsoft.com/kb/3114025)：从 Windows 8.1 或 Server 2012 R2 访问 Azure 文件存储时性能不佳
* [KB3033930](https://support.microsoft.com/kb/3033930)：该修补程序提升了 Windows 中 Azure 服务每个进程的 RIO 缓冲区的 64K 限制
* [KB3004545](https://support.microsoft.com/kb/3004545)：无法通过 Windows 中的 VPN 连接访问 Azure 托管服务上托管的虚拟机
* [KB3082343](https://support.microsoft.com/kb/3082343)：Azure 站点到站点 VPN 隧道使用 Windows Server 2012 R2 RRAS 时，跨界 VPN 连接断开
* [KB3146723](https://support.microsoft.com/kb/3146723)：MS16-048：CSRSS 的安全更新说明：2016 年 4 月 12 日
* [KB2904100](https://support.microsoft.com/kb/2904100)：Windows 中发生磁盘 I/O 期间系统会冻结

## 运行 Sysprep<a id="step23"></a>    
如果要创建一个映像并将其部署到多个 VM，则在将 VHD 上传到 Azure 之前，需要运行 Sysprep 来通用化该映像。 无需运行要用作专用 VHD 的 Sysprep。 

Sysprep 将删除所有个人帐户信息及其他某些数据，并准备好要用作映像的计算机。 有关 Sysprep 的详细信息，请参阅[如何使用 Sysprep：简介](http://technet.microsoft.com/library/bb457073.aspx)。

确保 Sysprep 支持计算机上运行的服务器角色。 有关详细信息，请参阅 [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

1. 登录到 Windows 虚拟机。
2. 以管理员身份打开“命令提示符”窗口。 将目录切换到 **%windir%\system32\sysprep**，然后运行 `sysprep.exe`。
3. 在“系统准备工具”对话框中，选择“进入系统全新体验(OOBE)”，确保已选中“通用化”复选框。
4. 在“关机选项”中选择“关机”。
5. 单击 **“确定”**。

    ![启动 Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. 在 Sysprep 完成时，它会关闭虚拟机。 不要重新启动 VM。

## 完成推荐配置
<a id="complete-recommended-configurations" class="xliff"></a>
以下设置不影响 VHD 上传。 但是，我们强烈建议你配置这些设置。

* 安装 [Azure 虚拟机代理](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)。 安装该代理后，可以启用 VM 扩展。 VM 扩展实现了要用于 VM 的大多数关键功能，例如重置密码、配置 RDP 和其他许多功能。
* 转储日志可帮助排查 Windows 崩溃问题。 启用转储日志收集：

    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```
* 在 Azure 中创建 VM 后，在 D: 驱动器上配置系统定义大小的页面文件

    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```

## 后续步骤
<a id="next-steps" class="xliff"></a>
* [将 Windows VM 映像上传到 Azure 以进行 Resource Manager 部署](upload-generalized-managed.md)
