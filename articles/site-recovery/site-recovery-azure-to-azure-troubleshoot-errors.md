---
title: "Azure 到 Azure 复制问题和错误的 Azure Site Recovery 故障排除 | Azure"
description: "排查复制 Azure 虚拟机进行灾难恢复时出现的错误和问题"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 11/21/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.openlocfilehash: 186c4ed2d5d0c3e7e1a1946938cd835ff489513c
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure 到 Azure VM 复制问题故障排除

本文介绍将 Azure 虚拟机从一个区域复制和恢复到另一个区域时的 Azure Site Recovery 常见问题，并介绍了如何排查这些问题。 有关支持的配置的详细信息，请参阅 [support matrix for replicating Azure VMs](site-recovery-support-matrix-azure-to-azure.md)（复制 Azure VM 的支持矩阵）。

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure 资源配额问题（错误代码 150097）
应启用订阅，在计划用作灾难恢复区域的目标区域中创建 Azure VM。 此外，订阅应启用充足的配额以创建特定大小的 VM。 默认情况下，Site Recovery 选取与源 VM 同样大小的目标 VM。 如果无法选择匹配的大小，则会自动选取最接近的大小。 如果没有支持源 VM 配置的匹配大小，将出现下面的错误消息：

错误代码 | **可能的原因** | 建议
--- | --- | ---
150097<br></br>消息：无法为虚拟机 VmName 启用复制。 | - 可能未启用订阅 ID，无法在目标区域位置中创建任何 VM。</br></br>- 可能未启用订阅 ID 或没有足够的配额，无法在目标区域位置中创建特定大小的 VM。</br></br>- 对于订阅 ID，在目标区域位置中找不到与源 VM NIC 计数 (2) 匹配的合适的目标 VM 大小。| 联系 [Azure 计费支持](/azure-supportability/resource-manager-core-quotas-request)，对订阅启用 VM 创建，以便在目标位置中创建所需大小的 VM。 启用后，重试失败的操作。

### <a name="fix-the-problem"></a>解决问题
可联系 [Azure 计费支持](/azure-supportability/resource-manager-core-quotas-request)启用订阅，以便在目标位置中创建所需大小的 VM。

如果目标位置存在容量约束，可禁用复制然后在订阅拥有充足配额的其他位置启用复制，以便创建所需大小的 VM.

## <a name="trusted-root-certificates-error-code-151066"></a>受信任的根证书（错误代码 151066）

如果 VM 上不存在任何最新的受信任根证书，则“启用复制”作业可能会失败。 没有证书，VM 发出的 Site Recovery 服务调用的身份验证和授权会失败。 将显示失败的“启用复制”Site Recovery 作业的错误消息：

错误代码 | 可能的原因 | 建议
--- | --- | ---
151066<br></br>消息：Site Recovery 配置失败。 | 计算机上不存在用于授权和身份验证的必需受信根证书。 | - 对于运行 Windows 操作系统的 VM，请确保计算机上存在受信任的根证书。 有关信息，请参阅[配置受信任根和不允许的证书](https://technet.microsoft.com/library/dn265983.aspx)。<br></br>- 对于运行 Linux 操作系统的 VM，请按照 Linux 操作系统版本分发商发布的受信任的根证书指南进行操作。

### <a name="fix-the-problem"></a>解决问题
**Windows**

在 VM 上安装所有最新的 Windows 更新，以便所有受信任的根证书均存在于计算机上。 在断开连接的环境中，请按照组织中的标准 Windows 更新过程执行操作并获取证书。 如果 VM 上不存在所需证书，则对 Site Recovery 服务的调用会因安全原因而失败。

按照组织中典型的 Windows 更新管理或证书更新管理过程进行操作，在 VM 上获取所有最新的根证书和更新的证书吊销列表。

要验证问题是否已解决，请在 VM 中使用浏览器转到 login.chinacloudapi.cn。

**Linux**

请按照 Linux 分发商提供的指南进行操作，在 VM 上获取最新的受信任根证书和最新的证书吊销列表。

由于 SuSE Linux 使用符号链接来维护证书列表，因此请按照以下步骤进行操作：

1.  以根用户身份登录。

2.  运行以下命令：

      ``# cd /etc/ssl/certs``

3.  若要查看是否存在 Symantec 根 CA 证书，请运行以下命令：

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  如果找不到该文件，请运行以下命令：

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  若要使用 b204d74a.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem 创建符号链接，请运行以下命令：

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  查看下面的命令是否具有以下输出。 如果没有，需创建一个符号链接：

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. 如果符号链接 653b494a.0 不存在，使用下面的命令创建一个符号链接：

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Recovery URL 或 IP范围的出站连接（错误代码 151037 或 151072）

要使 Site Recovery 复制正常运行，必须具有从 VM 到特定 URL 或 IP 范围的出站连接。 如果 VM 位于防火墙之后，或其使用网络安全组 (NSG) 规则来控制出站连接，则可能会出现以下错误消息之一：

**错误代码** | 可能的原因 | **建议**
--- | --- | ---
151037<br></br>消息：无法向 Site Recovery 注册 Azure 虚拟机。 | - 正在使用 NSG 来控制 VM 上的出站访问权限，而所需的 IP 范围未列在出站访问权限允许列表上。</br></br>- 正在使用第三方防火墙工具，而所需的 IP 范围/URL 未列在允许列表上。</br>| - 如果使用防火墙代理控制 VM 上的出站网络连接，请确保先决条件 URL 或数据中心 IP 范围已列入允许列表。 有关信息，请参阅[防火墙代理指南](https://aka.ms/a2a-firewall-proxy-guidance)。</br></br>- 如果使用 NSG 规则来控制 VM 上的出站网络连接，请确保先决条件数据中心 IP 范围已列入允许列表。 有关信息，请参阅[网络安全组指南](https://aka.ms/a2a-nsg-guidance)。
151072<br></br>消息：Site Recovery 配置失败。 | 无法建立与 Site Recovery 服务终结点的连接。 | - 如果使用防火墙代理控制 VM 上的出站网络连接，请确保先决条件 URL 或数据中心 IP 范围已列入允许列表。 有关信息，请参阅[防火墙代理指南](https://aka.ms/a2a-firewall-proxy-guidance)。</br></br>- 如果使用 NSG 规则来控制 VM 上的出站网络连接，请确保先决条件数据中心 IP 范围已列入允许列表。 有关信息，请参阅[网络安全组指南](https://aka.ms/a2a-nsg-guidance)。

### <a name="fix-the-problem"></a>解决问题
要将[所需 URL](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) 或[所需 IP 范围](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges)列入允许列表，请按照[网络指南文档](site-recovery-azure-to-azure-networking-guidance.md)中的步骤进行操作。

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>计算机中找不到磁盘（错误代码 150039）

必须对附加到 VM 的新磁盘进行初始化。

错误代码 | 可能的原因 | 建议
--- | --- | ---
150039<br></br>**消息**：逻辑单元号 (LUN) 为 (LUNValue) 的 Azure 数据磁盘 (DiskName) (DiskURI) 未映射到具有相同 LUN 值的 VM 报告的相应磁盘。 | - 新的数据磁盘已附加到 VM，但未初始化。</br></br>- VM 中的数据磁盘未正确报告附加到 VM 的磁盘的 LUN 值。| 确保数据磁盘已初始化，然后重试该操作。</br></br>对于 Windows：[附加并初始化新磁盘](/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk)。</br></br>对于 Linux：[在 Linux 中初始化新数据磁盘](/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux)。

### <a name="fix-the-problem"></a>解决问题
确保数据磁盘已初始化，然后重试该操作：

- 对于 Windows：[附加并初始化新磁盘](/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk)。
- 对于 Linux：[在 Linux 中初始化新数据磁盘](/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux)。

如果问题仍然存在，请联系支持部门。

## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>在“启用复制”选项中看不到 Azure VM

在[启用复制：步骤 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) 的选项中可能看不到 Azure VM。 此问题可能由留在 Azure VM 上的过时 Site Recovery 配置导致。 在以下情况下，Azure VM 中可能留有过时的配置：

- 使用 Site Recovery 启用了 Azure VM 的复制，然后在删除 Site Recovery 保管库时未显式禁用 VM 上的复制。
- 使用 Site Recovery 启用了 Azure VM 的复制，然后在删除包含 Site Recovery 保管库的资源组时未显式禁用 VM 上的复制。

### <a name="fix-the-problem"></a>解决问题

可使用[删除过时 ASR 配置脚本](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412)，删除 Azure VM 上的过时 Site Recovery 配置。 删除过时配置后，应在[启用复制：步骤 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) 中看到 VM。

## <a name="next-steps"></a>后续步骤
<!-- Update_Description: update meta properties, wording update -->