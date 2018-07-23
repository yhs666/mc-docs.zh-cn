---
title: Azure 到 Azure 复制问题和错误的 Azure Site Recovery 故障排除 | Azure
description: 排查复制 Azure 虚拟机进行灾难恢复时出现的错误和问题
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.devlang: na
ms.topic: article
origin.date: 07/06/2018
ms.date: 07/23/2018
ms.author: v-yeche
ms.openlocfilehash: a44486c5653e4c6d9d4bcd8d985b41a14a99cd79
ms.sourcegitcommit: c82fb6f03079951442365db033227b07c55700ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2018
ms.locfileid: "39168478"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure 到 Azure VM 复制问题故障排除

本文介绍将 Azure 虚拟机从一个区域复制和恢复到另一个区域时的 Azure Site Recovery 常见问题，并介绍了如何排查这些问题。 有关支持的配置的详细信息，请参阅 [support matrix for replicating Azure VMs](site-recovery-support-matrix-azure-to-azure.md)（复制 Azure VM 的支持矩阵）。

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure 资源配额问题（错误代码 150097）
应启用订阅，在计划用作灾难恢复区域的目标区域中创建 Azure VM。 此外，订阅应启用充足的配额以创建特定大小的 VM。 默认情况下，Site Recovery 选取与源 VM 同样大小的目标 VM。 如果无法选择匹配的大小，则会自动选取最接近的大小。 如果没有支持源 VM 配置的匹配大小，将出现下面的错误消息：

错误代码 | **可能的原因** | 建议
--- | --- | ---
150097<br></br>消息：无法为虚拟机 VmName 启用复制。 | - 可能未启用订阅 ID，无法在目标区域位置中创建任何 VM。</br></br>- 可能未启用订阅 ID 或没有足够的配额，无法在目标区域位置中创建特定大小的 VM。</br></br>- 对于订阅 ID，在目标区域位置中找不到与源 VM NIC 计数 (2) 匹配的合适的目标 VM 大小。| 联系 [Azure 计费支持](https://support.windowsazure.cn/support/support-azure)，对订阅启用 VM 创建，以便在目标位置中创建所需大小的 VM。 启用后，重试失败的操作。

### <a name="fix-the-problem"></a>解决问题
可联系 [Azure 计费支持](https://support.windowsazure.cn/support/support-azure)启用订阅，以便在目标位置中创建所需大小的 VM。
<!-- SHOUD BE https://support.windowsazure.cn/support/support-azure FOR /azure-supportability/resource-manager-core-quotas-request -->

如果目标位置存在容量约束，可禁用复制然后在订阅拥有充足配额的其他位置启用复制，以便创建所需大小的 VM.

## <a name="trusted-root-certificates-error-code-151066"></a>受信任的根证书（错误代码 151066）

如果 VM 上不存在任何最新的受信任根证书，则“启用复制”作业可能会失败。 没有证书，VM 发出的 Site Recovery 服务调用的身份验证和授权会失败。 将显示失败的“启用复制”Site Recovery 作业的错误消息：

错误代码 | 可能的原因 | **建议**
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

2.  运行此命令以更改目录。

      ``# cd /etc/ssl/certs``

3. 检查 Symantec 根 CA 证书是否存在。

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. 如果未找到 Symantec 根 CA 证书，则运行以下命令下载文件。 检查是否有任何错误，对于网络故障执行建议的操作。

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. 检查 Baltimore 根 CA 证书是否存在。

      ``# ls Baltimore_CyberTrust_Root.pem``

6. 如果未找到 Baltimore 根 CA 证书，则下载该证书。  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. 检查 DigiCert_Global_Root_CA 证书是否存在。

    ``# ls DigiCert_Global_Root_CA.pem``

8. 如果未找到 DigiCert_Global_Root_CA，则运行以下命令下载该证书。

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. 运行 rehash 脚本更新新下载的证书的证书使用者哈希。

    ``# c_rehash``

10. 检查是否已为证书创建使用者哈希作为符号链接。

    - 命令

      ``# ls -l | grep Baltimore``

    - 输出

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - 命令

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - 输出

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - 命令

      ``# ls -l | grep DigiCert_Global_Root``

    - 输出

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. 使用文件名 b204d74a.0 创建文件 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem 的副本

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. 使用文件名 653b494a.0 创建文件 Baltimore_CyberTrust_Root.pem 的副本

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. 使用文件名 3513523f.0 创建文件 DigiCert_Global_Root_CA.pem 的副本

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  

14. 检查文件是否存在。  

    - 命令

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - 输出

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Recovery URL 或 IP范围的出站连接（错误代码 151037 或 151072）

要使 Site Recovery 复制正常运行，必须具有从 VM 到特定 URL 或 IP 范围的出站连接。 如果 VM 位于防火墙之后，或其使用网络安全组 (NSG) 规则来控制出站连接，则可能会出现以下错误消息之一：

**错误代码** | 可能的原因 | **建议**
--- | --- | ---
151037<br></br>消息：无法向 Site Recovery 注册 Azure 虚拟机。 | - 正在使用 NSG 来控制 VM 上的出站访问权限，而所需的 IP 范围未列在出站访问权限允许列表上。</br></br>- 正在使用第三方防火墙工具，而所需的 IP 范围/URL 未列在允许列表上。</br>| - 如果使用防火墙代理控制 VM 上的出站网络连接，请确保先决条件 URL 或数据中心 IP 范围已列入允许列表。 有关信息，请参阅[防火墙代理指南](https://aka.ms/a2a-firewall-proxy-guidance)。</br></br>- 如果使用 NSG 规则来控制 VM 上的出站网络连接，请确保先决条件数据中心 IP 范围已列入允许列表。 有关信息，请参阅[网络安全组指南](https://aka.ms/a2a-nsg-guidance)。
151072<br></br>消息：Site Recovery 配置失败。 | 无法建立与 Site Recovery 服务终结点的连接。 | - 如果使用防火墙代理控制 VM 上的出站网络连接，请确保先决条件 URL 或数据中心 IP 范围已列入允许列表。 有关信息，请参阅[防火墙代理指南](https://aka.ms/a2a-firewall-proxy-guidance)。</br></br>- 如果使用 NSG 规则来控制 VM 上的出站网络连接，请确保先决条件数据中心 IP 范围已列入允许列表。 有关信息，请参阅[网络安全组指南](https://aka.ms/a2a-nsg-guidance)。

### <a name="fix-the-problem"></a>解决问题
要将[所需 URL](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) 或[所需 IP 范围](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges)列入允许列表，请按照[网络指南文档](site-recovery-azure-to-azure-networking-guidance.md)中的步骤进行操作。

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>计算机中找不到磁盘（错误代码 150039）

必须对附加到 VM 的新磁盘进行初始化。

错误代码 | 可能的原因 | 建议
--- | --- | ---
150039<br></br>**消息**：逻辑单元号 (LUN) 为 (LUNValue) 的 Azure 数据磁盘 (DiskName) (DiskURI) 未映射到具有相同 LUN 值的 VM 报告的相应磁盘。 | - 新的数据磁盘已附加到 VM，但未初始化。</br></br>- VM 中的数据磁盘未正确报告附加到 VM 的磁盘的 LUN 值。| 确保数据磁盘已初始化，然后重试该操作。</br></br>对于 Windows：[附加并初始化新磁盘](/virtual-machines/windows/attach-managed-disk-portal)。</br></br>对于 Linux：[在 Linux 中初始化新数据磁盘](/virtual-machines/linux/add-disk)。

### <a name="fix-the-problem"></a>解决问题
确保数据磁盘已初始化，然后重试该操作：

- 对于 Windows：[附加并初始化新磁盘](/virtual-machines/windows/attach-managed-disk-portal)。
- 对于 Linux：[在 Linux 中添加新数据磁盘](/virtual-machines/linux/add-disk)。

如果问题仍然存在，请联系支持部门。

## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>无法查看 Azure VM 中“启用复制”的选择

如果看不到要为其启用复制的虚拟机，可能是因为有过时的 Site Recovery 配置保留在 Azure VM 中。 在以下情况下，Azure VM 中可能留有过时的配置：

- 使用 Site Recovery 启用了 Azure VM 的复制，然后在删除 Site Recovery 保管库时未显式禁用 VM 上的复制。
- 使用 Site Recovery 启用了 Azure VM 的复制，然后在删除包含 Site Recovery 保管库的资源组时未显式禁用 VM 上的复制。

### <a name="fix-the-problem"></a>解决问题

可以使用[删除过时的 ASR 配置脚本](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412)，删除 Azure VM 上过时的 Site Recovery 配置。 删除过时配置后，应能够看到该 VM。

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>VM 的预配状态无效（错误代码 150019）

若要在 VM 上启用复制，则预配状态应该是“成功”。 可以通过执行以下步骤来检查 VM 状态。

1.  在 Azure 门户中，从“所有服务”中选择“资源浏览器”。
2.  展开“订阅”列表并选择你的订阅。
3.  展开“资源组”并选择 VM 的资源组。
4.  展开“资源”列表并选择你的虚拟机
5.  在右侧的实例视图中检查“预配状态”字段。

### <a name="fix-the-problem"></a>解决问题

- 如果“预配状态”为“失败”，请联系支持人员并提供详细信息以排除故障。
- 如果“预配状态”为“正在更新”，则无法部署另一扩展。 检查 VM 上是否有任何正在进行的操作，等待它们完成，然后重试失败的 Site Recovery“启用复制”作业。

## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM+/卷影复制服务错误（错误代码 151025）
错误代码 | 可能的原因 | **建议**
--- | --- | ---
151025<br></br>**消息**：Site Recovery 扩展安装失败 | - 禁用了“COM + 系统应用程序”服务。</br></br>- 禁用了“卷影复制”服务。| 将“COM + 系统应用程序”和“卷影复制”服务设置为自动或手动启动模式。

### <a name="fix-the-problem"></a>解决问题

可以打开“服务”控制台并确保“COM + 系统应用程序”和“卷影复制”的“启动类型”未设置为“已禁用”。
  ![com-error](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>后续步骤
[复制 Azure 虚拟机](site-recovery-replicate-azure-to-azure.md)
<!-- Update_Description: update meta properties, wording update -->