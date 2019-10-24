---
title: 对 Azure Stack 应用原始设备制造商 (OEM) 更新 | Microsoft Docs
description: 了解如何对 Azure Stack 应用原始设备制造商 (OEM) 更新。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/28/2019
ms.date: 10/21/2019
ms.author: v-jay
ms.lastreviewed: 08/15/2019
ms.reviewer: ppacent
ms.openlocfilehash: 6f773fd405b32546cebf6915484e6c09bc8ca4b0
ms.sourcegitcommit: 713bd1d1b476cec5ed3a9a5615cfdb126bc585f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578328"
---
# <a name="apply-azure-stack-original-equipment-manufacturer-oem-updates"></a>应用 Azure Stack 原始设备制造商 (OEM) 更新

*适用于：Azure Stack 集成系统*

可以将原始设备制造商 (OEM) 更新应用到 Azure Stack 硬件组件，以便接收驱动程序和固件改进以及安全修补程序，同时尽量减少对用户的影响。 本文介绍 OEM 更新、OEM 联系人信息，以及如何应用 OEM 更新。

## <a name="overview-of-oem-updates"></a>OEM 更新概述

除了 Azure Stack 更新，许多 OEM 还发布 Azure Stack 硬件的定期更新，例如驱动程序和固件的更新。 这些更新称为“OEM 包更新”。  若要了解 OEM 是否发布了 OEM 包更新，请查看 [OEM 的 Azure Stack 文档](#oem-contact-information)。

这些 OEM 包更新会上传到 **updateadminaccount** 存储帐户中，并通过 Azure Stack 管理员门户应用。 有关详细信息，请参阅[应用 OEM 更新](#apply-oem-updates)。

有关原始设备制造商 (OEM) 为确保 OEM 包更新通知到达你的组织而采取的具体通知流程，请咨询原始设备制造商。

某些硬件供应商可能会要求使用硬件供应商 VM 来处理内部固件更新过程。  有关详细信息，请参阅[配置硬件供应商 VM](#configure-hardware-vendor-vm)。

## <a name="oem-contact-information"></a>OEM 联系人信息 

此部分包含 OEM 联系人信息以及 OEM Azure Stack 参考资料的链接。

| 硬件合作伙伴 | 区域 | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | 全部 | [Cisco Azure Stack 支持/固件更新 - 自动通知（需要帐户/登录名）](https://software.cisco.com/download/redirect?i=!y&mdfid=283862063&softwareid=286320368&release=1.0(0)&os=)<br><br>[适用于 Azure Stack 的 Cisco 集成系统的发行说明](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) |
| Dell EMC | 全部 | [Cloud for Azure Stack 14G（需要帐户和登录）](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Cloud for Azure Stack 13G（需要帐户和登录）](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| HPE | 全部 | [HPE ProLiant for Azure Stack](http://www.hpe.com/info/MASupdates) |
| Lenovo | 全部 | [ThinkAgile SXM 最佳食谱](https://datacentersupport.lenovo.com/us/en/solutions/ht505122)
| Wortmann |  | [OEM/固件包](https://drive.terracloud.de/dl/fiTdTb66mwDAJWgUXUW8KNsd/OEM)<br>[terra Azure Stack 文档（包括 FRU）](https://drive.terracloud.de/dl/fiWGZwCySZSQyNdykXCFiVCR/TerraAzSDokumentation)

## <a name="apply-oem-updates"></a>应用 OEM 更新

按以下步骤应用 OEM 包：

1. 以下事项需联系 OEM：
      - 确定 OEM 包的当前版本。  
      - 查找下载 OEM 包的最佳方法。  
2. 在应用 OEM 包更新之前，应始终应用系统当前 Azure Stack 版本上可用的最新 Azure Stack 修补程序。 有关修补程序的详细信息，请参阅 [Azure Stack 修补程序](/azure-stack/operator/azure-stack-servicing-policy)。
3. 按照[下载集成系统的更新包](azure-stack-servicing-policy.md)中介绍的步骤准备 OEM 包。
4. 使用[在 Azure Stack 中应用更新](azure-stack-apply-updates.md)中介绍的步骤应用更新。

## <a name="configure-hardware-vendor-vm"></a>配置硬件供应商 VM

某些硬件供应商可能会要求使用 VM 来处理 OEM 更新过程。 如果在运行 **Set-OEMExternalVM**cmdlet 时需要将 `ProxyVM` 或 `HardwareManager` 用于 **-VMType**，以及应将哪个凭据用于 **-Credential**，则硬件供应商将负责创建这些 VM 并进行记录。 这些 VM 创建好以后，请通过特权终结点使用 **Set-OEMExternalVM** 配置它们。

有关 Azure Stack 上的特权终结点的详细信息，请参阅[使用 Azure Stack 中的特权终结点](azure-stack-privileged-endpoint.md)。

1.  访问特权终结点。

    ```powershell  
    $cred = Get-Credential
    $session = New-PSSession -ComputerName <IP Address of ERCS>
    -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```

2. 使用 **Set-OEMExternalVM** cmdlet 配置硬件供应商 VM。 此 cmdlet 针对 **-VMType** `ProxyVM` 验证 IP 地址和凭据。 对于 **-VMType** `HardwareManager`，此 cmdlet 不会验证输入。 提供给 **Set-OEMExternalVM** 的 **-Credential** 参数将由硬件供应商文档明确记录。  它不是用于特权终结点的 CloudAdmin 凭据，也不是任何其他现有 Azure Stack 凭据。

    ```powershell  
    $VmCred = Get-Credential
    Invoke-Command -Session $session
        { 
    Set-OEMExternalVM -VMType <Either "ProxyVM" or "HardwareManager">
        -IPAddress <IP Address of hardware vendor VM> -Credential $using:VmCred
        }
    ```

## <a name="next-steps"></a>后续步骤

[Azure Stack 更新](azure-stack-updates.md)