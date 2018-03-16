---
title: "Azure 文件常见问题解答 | Microsoft 文档"
description: "查看有关 Azure 文件的常见问题解答。"
services: storage
documentationcenter: 
author: yunan2016
manager: digimobile
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
origin.date: 12/04/2017
ms.date: 01/22/2018
ms.author: v-nany
ms.openlocfilehash: fc23478e4bb66143f8f14b4319acf4366242186e
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="frequently-asked-questions-about-azure-files"></a>有关 Azure 文件的常见问题解答
[Azure 文件](storage-files-introduction.md)在云中提供可以通过行业标准的[服务器消息块 (SMB) 协议](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)（也称为通用 Internet 文件系统，简称 CIFS）访问的完全托管文件共享。 你可以在云或 Windows、Linux 和 macOS 的本地部署同时装载 Azure 文件共享。 另外，你也可以使用 Azure 文件同步（预览版）在 Windows Server 计算机上缓存 Azure 文件共享，以在靠近使用数据的位置实现快速访问。


## <a name="general"></a>常规
* <a id="why-files-useful"></a>
**Azure 文件如何发挥作用？**  
   你可以使用 Azure 文件在云中创建文件共享，而无需应对物理服务器、设备或装置的开销。 诸如应用操作系统更新和更换损坏的磁盘等琐碎枯燥的工作，我们可为你逐一代劳。 若要详细了解 Azure 文件适用的应用场景，请参阅[为何 Azure 文件很有用](storage-files-introduction.md#why-azure-files-is-useful)。

* <a id="file-access-options"></a>
**访问 Azure 文件中的文件有哪些不同方式？**  
    可以使用 SMB 3.0 协议将文件共享装载到本地计算机上，也可以使用[存储资源管理器](http://storageexplorer.com/)等工具访问文件共享中的文件。 在应用程序中，可以使用存储客户端库、REST API、PowerShell 或 Azure CLI 来访问 Azure 文件共享中的文件。


* <a id="files-versus-blobs"></a>
**相对于 Azure Blob 存储，我为什么要对数据使用 Azure 文件共享？**  
    Azure 文件和 Azure Blob 存储均提供在云中存储大量数据的方法，但是用途略有不同。 
    
    Azure Blob 存储适用于需要存储非结构化数据且具有大规模缩放性的云本机应用程序。 为了更大程度地提升性能和可缩放性，相对于真实的文件系统而言，Azure Blob 存储是更简单的存储抽象。 此外，只可通过基于 REST 的客户端库访问 Azure Blob 存储（或直接通过基于 REST 的协议访问）。

    Azure 文件是一个专门的文件系统， 具有你在使用本地操作系统多年来所熟知和喜爱的所有文件抽象。 例如 Azure Blob 存储，Azure 文件提供了一个 REST 接口和基于 REST 的客户端库。 与 Azure Blob 存储不同的是，Azure 文件提供了对 Azure 文件共享的 SMB 访问权限。 通过使用 SMB，无需对文件系统写入任何代码或附加任何特殊驱动程序，即可在 Windows、Linux 或 macOS 上及本地或云 VM 中直接装载 Azure 文件共享。 此外，你也可以使用 Azure 文件同步在本地文件服务器上缓存 Azure 文件共享，以在靠近使用数据的位置实现快速访问。 
   
    有关 Azure 文件和 Azure Blob 存储之间差异的深入描述，请参阅[确定何时使用 Azure Blob 存储、Azure 文件或 Azure 磁盘](../common/storage-decide-blobs-files-disks.md?toc=%2fstorage%2ffiles%2ftoc.json)。 若要了解有关 Azure Blob 存储的详细信息，请参阅 [Blob 存储简介](../blobs/storage-blobs-introduction.md)。

* <a id="files-versus-disks"></a>**相对于 Azure 磁盘，我为什么要使用 Azure 文件共享？**  
    Azure 磁盘中的磁盘只是一个磁盘。 独立磁盘本身并不能发挥太大作用。 若要充分利用 Azure 磁盘，必须将其与在 Azure 中运行的虚拟机相关联。 Azure 磁盘可用于在本地服务器上使用磁盘的所有内容。 你可将其用作操作系统磁盘、操作系统的交换空间，或者应用程序的专用存储空间。 Azure 磁盘其中一个有趣的用途是，可在云中创建一个文件服务器，以在可能使用 Azure 文件共享的相同位置使用。 当需要 Azure 文件当前不支持的部署选项（例如，NFS 协议支持或高级存储）时，在 Azure 虚拟机中部署文件服务器则是一种非常行之有效的获取 Azure 中文件存储的方法。 

    但是，相比使用 Azure 文件共享，通过将 Azure 磁盘作为后端存储来运行文件服务器的方式，由于多方面的原因，其经济成本通常会更高。 首先，除了为磁盘存储付费之外，还必须为运行一个或多个 Azure VM 的成本付费。 其次，你还必须管理用于运行文件服务器的 VM。 例如，负责操作系统升级。 最后，如果你最终需要在本地缓存数据，则还要自行安装和管理复制技术（例如，分布式文件系统复制 (DFSR)）来实现此目的。


    有关在 Azure 中设置高性能和高可用性文件服务器的选项的信息，请参阅 [Deploying IaaS VM guest clusters in Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)（在 Azure 中部署 IaaS VM 来宾群集）。 有关 Azure 文件和 Azure 磁盘之间差异的深入描述，请参阅[确定何时使用 Azure Blob 存储、Azure 文件或 Azure 磁盘](../common/storage-decide-blobs-files-disks.md?toc=%2fstorage%2ffiles%2ftoc.json)。 若要详细了解 Azure 磁盘，请参阅 [Azure 托管磁盘概述](../../virtual-machines/windows/managed-disks-overview.md)。

* <a id="get-started"></a>
**如何开始使用 Azure 文件？**  
   开始使用 Azure 文件非常简单。 首先，[创建文件共享](storage-how-to-create-file-share.md)，然后再将其装载到首选操作系统中： 

    * [在 Windows 中装载](storage-how-to-use-files-windows.md)
    * [在 Linux 中装载](storage-how-to-use-files-linux.md)
    * [在 macOS 中装载](storage-how-to-use-files-mac.md)

   有关部署 Azure 文件共享以替换组织中生产文件共享的详细指南，请参阅[规划 Azure 文件部署](storage-files-planning.md)。

* <a id="redundancy-options"></a>
**Azure 文件支持哪些存储冗余选项？**  
    当前，Azure 文件仅支持本地冗余存储 (LRS) 和异地冗余存储 (GRS)。 将来我们计划支持区域冗余存储空间 (ZRS) 和读取访问权限异地冗余存储 (RA-GRS)，但目前还没有可分享的日程表。

- <a id="tier-options"></a>
**Azure 文件支持哪些存储层？**  
    Azure 文件目前仅支持标准存储层。 当前我们暂无有关高级存储和冷存储支持的日程表可供分享。 
    
    > [!NOTE]
    > 你无法使用仅限 Blob 存储帐户或高级存储帐户创建 Azure 文件共享。



## <a name="security-authentication-and-access-control"></a>安全性、身份验证和访问控制
* <a id="ad-support"></a>
**Azure 文件是否支持基于 Active Directory 的身份验证和访问控制？**  
    Azure 文件提供了两种方法来管理访问控制：

    - 你可以使用共享访问签名 (SAS) 生成在指定时间间隔内有效的具有特定权限的令牌。 例如，可以生成在 10 分钟后到期、对特定文件具有只读访问权限的令牌。 只要拥有此有效令牌，就可以在 10 分钟内拥有对给定文件的只读访问权限。 目前，仅通过 REST API 或客户端库支持共享的访问签名密钥。 你必须使用存储帐户密钥通过 SMB 装载 Azure 文件共享。


    当前，Azure 文件不直接支持 Active Directory。

* <a id="encryption-at-rest"></a>
**如何确保已静态加密 Azure 件共享？**  
    Azure 存储服务加密默认在所有区域启用。 对于这些区域，你无需执行任何操作来启用加密。 对其他区域，请参阅[服务器端加密](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)。

* <a id="access-via-browser"></a>
**如何使用 Web 浏览器提供对特定文件的访问权限？**  
    你可以使用共享访问签名生成在指定时间间隔内有效的、具有特定权限的令牌。 例如，可以生成一个在设定时段内对特定文件具有只读访问权限的令牌。 只要拥有此 URL，就可以在令牌有效期间，直接通过任意 Web 浏览器访问文件。 你可以从类似存储资源管理器的 UI 轻松生成共享的访问签名密钥。

* <a id="file-level-permissions"></a>
**能否对共享中的文件夹指定只读或只写权限？**  
    如果使用 SMB 装载文件共享，则不具有文件夹级的控制权限。 但是，如果使用 REST API 或客户端库创建共享访问签名，则可以在共享内的文件夹上指定只读或只写权限。


* <a id="data-compliance-policies"></a>
**Azure 文件支持哪些数据符合性策略？**  
   Azure 文件所依据的存储体系结构与 Azure 存储中的其他存储服务使用的相同。 Azure 文件实施的数据符合性策略也与其他 Azure 存储服务使用的相同。 有关 Azure 存储数据符合性的详细信息，可以下载并参阅 [Azure 数据保护文档](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409)，以及访问 [Azure 信任中心](https://www.azure.cn/support/trust-center/)。

## <a name="on-premises-access"></a>本地访问
* <a id="expressroute-not-required"></a>
**必须使用 Azure ExpressRoute 才能连接到 Azure 文件吗？**  
    否。 ExpressRoute 不是访问 Azure 文件共享的必要条件。 如果要直接在本地装载 Azure 文件共享，则只需打开端口 445（TCP 出站）即可进行 Internet 访问（这是 SMB 用于进行通信的端口）。 如果正在使用 Azure 文件同步，则只需端口 443（TCP 出站）即可进行 HTTPS 访问（无需 SMB）。 但是，你可以将 ExpressRoute 与这些访问选项中任意一项一起使用。

* <a id="mount-locally"></a>
**如何才能在本地计算机上装载 Azure 文件共享？**  
    可以使用 SMB 协议装载文件共享，只要端口 445（TCP 出站）处于打开状态，且客户端支持 SMB 3.0 协议（例如，如果使用的是 Windows 10 或 Windows Server 2016）。 如果端口 445 被组织的策略或 ISP 阻止，则可使用 Azure 文件同步访问 Azure 文件共享。



## <a name="billing-and-pricing"></a>计费和定价
* <a id="vm-file-share-network-traffic"></a>
**Azure VM 与 Azure 文件共享之间的网络流量是否作为外部带宽计入订阅费用？**  
    如果文件共享和 VM 位于同一 Azure 区域，则它们之间的流量是不会额外收费的。 如果文件共享和 VM 位于不同的区域，则它们之间的流量会作为外部带宽收费。


## <a name="scale-and-performance"></a>缩放和性能
* <a id="files-scale-limits"></a>
**Azure 文件存在哪些缩放限制？**  
    有关 Azure 文件的可伸缩性和性能目标的信息，请参阅 [Azure 文件可伸缩性和性能目标](storage-files-scale-targets.md)。

* <a id="need-larger-share"></a>
**我需要大于 Azure 文件目前提供的文件共享的文件共享。我是否可以增加 Azure 文件共享的大小？**  
    否。 Azure 文件共享的上限是 5 TiB。 当前，这是硬限制，无法调整。 我们正致力于寻找将共享大小提升至 100 TiB 的解决方案，但当前尚无可供分享的时间表。

* <a id="open-handles-quota"></a>
**多少个客户端可以同时访问同一文件？**   
    单个文件有 2000 个打开句柄配额。 当你拥有 2000 个打开句柄时，会显示一条错误消息，指示已达到此配额。

* <a id="zip-slow-performance"></a>
**解压 Azure 文件中的文件时性能较慢。**  
    若要将大量文件传输到 Azure 文件，建议使用 AzCopy（Windows 版；Linux 和 Unix 预览版）或 Azure Powershell。 这些工具已针对网络传输进行了优化。

* <a id="slow-perf-windows-81-2012r2"></a>
**为什么在 Windows Server 2012 R2 或 Windows 8.1 上装载了 Azure 文件共享后性能较慢？**  
    在 Windows Server 2012 R2 和 Windows 8.1 上装载 Azure 文件共享后，有一个已知的问题。 在 2014 年 4 月的 Windows 8.1 和 Windows Server 2012 R2 累积更新中已修补了此问题。 请确保 Windows Server 2012 R2 和 Windows 8.1 的所有实例均应用了此修补程序，以获得最佳性能。 （你应始终通过 Windows 更新获取 Windows 修补程序。）有关详细信息，请查看相关的 Azure 识库文章：[从 Windows 8.1 或 Server 2012 R2 访问 Azure 文件时性能降低](https://support.microsoft.com/kb/3114025)。

## <a name="features-and-interoperability-with-other-services"></a>功能以及与其他服务的互操作性
* <a id="cluster-witness"></a>
**是否可以将 Azure 文件共享作为 Windows 服务器故障转移群集的文件共享见证？**  
    Azure 文件共享目前不支持此配置。 有关如何为 Azure Blob 存储设置此服务的详细信息，请参阅[部署故障转移群集的云见证](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。


* <a id="rest-rename"></a>
**REST API 中是否有重命名操作？**  
    目前没有。

* <a id="nested-shares"></a>
**是否可以设置嵌套共享？也就是说，能否在共享下使用共享？**  
    否。 文件共享是可以装载的虚拟驱动程序，因此不支持嵌套共享。

* <a id="ibm-mq"></a>
**如何将 Azure 文件与 IBM MQ 配合使用？**  
    IBM 已发布相关文档，帮助 IBM MQ 客户通过 IBM 服务配置 Azure 文件。 有关详细信息，请参阅 [How to set up an IBM MQ multi-instance queue manager with  Azure Files service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)（如何通过 Azure 文件服务设置 IBM MQ 多实例队列管理器）。

## <a name="see-also"></a>另请参阅
* [在 Windows 中排查 Azure 文件问题](storage-troubleshoot-windows-file-connection-problems.md)
* [在 Linux 中排查 Azure 文件问题](storage-troubleshoot-linux-file-connection-problems.md)
