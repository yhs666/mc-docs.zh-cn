有时 Azure 虚拟机 (VM) 可能重启，即使没有明显原因，也没有证据表明用户发起重启操作。 本文列出了可导致 VM 重启的操作和事件，并针对如何避免意外重启问题或减少该问题影响提供见解。

## 配置 VM 以实现高可用性
<a id="configure-the-vms-for-high-availability" class="xliff"></a>
若要防止 Azure 上运行的应用程序出现任何类型的 VM 重启和停机问题，最佳方式是配置 VM 以实现高可用性。

若要为应用程序提供此级别的冗余，建议两个或更多 VM 组合到一个可用性集中。 这种配置可确保发生计划内或计划外维护事件时，至少有一个 VM 可用，并满足 99.95% 的 [Azure SLA](https://www.azure.cn/support/sla/virtual-machines/) 要求。

有关可用性集的详细信息，请参阅以下文章：

- [管理 VM 的可用性](../articles/virtual-machines/windows/manage-availability.md)
- [配置 VM 的可用性](../articles/virtual-machines/windows/classic/configure-availability.md)

## 可能导致 VM 重启的操作和事件
<a id="actions-and-events-that-can-cause-the-vm-to-reboot" class="xliff"></a>

### 计划内维护
<a id="planned-maintenance" class="xliff"></a>
Azure 在全球范围内定期执行更新，提高 VM 所基于主机基础结构的可靠性、性能及安全性。 许多更新（包括内存保留更新）在执行时不会对 VM 或云服务产生任何影响。

但是，有些更新确实需要重启。 VM 会在修补基础结构期间关闭，随后 VM 会重启。

若要了解什么是 Azure 计划内维护，及其如何影响 Linux VM 的可用性，请参阅以下文章。 这些文章介绍了 Azure 计划内维护过程的背景，以及如何安排计划内维护以进一步减少影响。

- [Azure VM 的计划内维护](../articles/virtual-machines/windows/planned-maintenance.md)
- [如何在 Azure VM 上安排计划内维护](../articles/virtual-machines/windows/planned-maintenance-schedule.md)

### 内存保留更新
<a id="memory-preserving-updates" class="xliff"></a>   
对于 Azure 中的这类更新，客户会发现它们对运行中 VM 没有任何影响。 其中一些更新主要面向组件或服务，更新时不会干扰正在运行的实例。 还有一些是主机操作系统上的平台基础结构更新，应用时无需重启 VM。

这些内存保留更新通过启用就地实时迁移技术实现。 更新时，VM 进入“暂停”状态以保留 RAM 中的内存，基础主机操作系统则接收必要的更新和补丁。 VM 在暂停后 30 秒内恢复正常。 恢复后，VM 的时钟将自动同步。

并非所有更新都可通过此机制进行部署，但如果暂停时间较短，使用此方法部署更新可大大减少对 VM 的影响。

多实例更新（针对可用性集中的 VM）一次应用一个更新域。

> [!Note]
> 具有旧内核版本的 Linux 计算机在此更新方法期间受内核错误影响。 若要避免此问题，请更新到内核版本 3.10.0-327.10.1 或更高版本。 有关详细信息，请参阅[主机节点升级后基于 3.10 内核的 Azure Linux VM 出现错误](https://support.microsoft.com/help/3212236)。     

### 用户发起的重启/关闭操作
<a id="user-initiated-rebootshutdown-actions" class="xliff"></a>

如果从 Azure 门户、Azure PowerShell、命令行接口或重置 API 执行重启，则可在 [Azure 活动日志](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md)中找到该事件。

如果从 VM 的操作系统执行重启，则可在系统日志中找到该事件。

通常导致 VM 重启的其他方案包括多个配置更改操作。 通常情况下，用户会看到一条指示执行特定操作将导致 VM 重启的警告消息。 示例包括任意 VM 大小调整操作、更改管理帐户密码和设置静态 IP 地址。

### 影响 VM 可用性的其他情况
<a id="other-situations-affecting-the-availability-of-your-vm" class="xliff"></a>
在其他情况下，Azure 可能主动暂停使用 VM。 用户可在执行此操作前收到电子邮件通知，以便他们有机会解决该基础问题。 示例包括安全冲突和已过期的过期付款方式。

### 主机服务器错误
<a id="host-server-faults" class="xliff"></a> 
在 Azure 数据中心内运行的物理服务器上托管 VM。 除了其他几个 Azure 组件外，物理服务器也运行名为“主机代理”的代理。 如果物理服务器上的这些 Azure 软件组件无响应，则监视系统会触发主机服务器重启，尝试恢复。 VM 通常在五分钟内再次可用，并继续像以前一样存在于同一主机上。

服务器错误通常由硬盘或固态硬盘等硬件故障引起。 Azure 持续监视这些事件，确定基础 bug，并在实现和测试缓解举措后推出更新。

由于某些主机服务器错误可能特定于该服务器，因此可通过手动将其重新部署到其他主机服务器来改善 VM 重复重启的情况。 在 VM 详细信息页上使用“重新部署”选项，或在 Azure 门户中停止并重启 VM，可触发此操作。

### 自动恢复
<a id="auto-recovery" class="xliff"></a>
如果出于某种原因，主机服务器不能重启，Azure 平台会启动自动恢复操作，使发生故障的主机服务器脱离轮换，以便展开进一步调查。 该主机上的所有 VM 均自动重新定位到其他运行正常的主机服务器。 此过程通常在 15 分钟内完成。 此博客介绍了自动恢复过程：[VM 自动恢复](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines)。

### 计划外维护
<a id="unplanned-maintenance" class="xliff"></a>
在少数情况下，Azure 运营团队可能需要执行维护活动，确保 Azure 平台整体运行正常。 此行为可能影响 VM 可用性，并且通常会引发与前述相同的自动恢复操作。  

计划外维护包括以下内容：

- 紧急节点碎片整理
- 紧急网络交换机更新

### VM 故障
<a id="vm-crashes" class="xliff"></a>
VM 可能因 VM 本身问题重启。 在 VM 上运行的工作负荷或角色可能触发来宾操作系统内的 bug 检查。 为帮助确定故障原因，请查看系统和应用程序日志（适用于 Windows VM）和串行日志（适用于 Linux VM）。

### 与存储相关的强制关机
<a id="storage-related-forced-shutdowns" class="xliff"></a>
对于在 Azure 存储基础结构上托管的操作系统和数据存储，Azure 中的 VM 依赖于虚拟磁盘。 每当 VM 和关联虚拟磁盘之间的可用性或连接性受影响超过 120 秒时，Azure 平台会强制关闭 VM，避免数据损坏。 存储连接还原后，VM 自动重启。 

关机持续时间可短至 5 分钟，也可能非常久。 下面是与存储相关的强制关机具体情况之一： 

超过 IO 限制

如果 I/O 请求因每秒输入/输出操作数 (IOPS) 超出磁盘 I/O 限制（标准磁盘存储限制为 500 IOPS）而持续受到限制，则可能暂时关闭 VM。 为缓解此问题，请在来宾 VM 中使用磁盘剥离或配置存储空间，具体情况取决于工作负荷。 有关详细信息，请参阅[配置 Azure VM 以获得最佳存储性能](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)。

通过 Azure 高级存储提供高达 80,000 IOPS 的 IOPS 限制。 有关详细信息，请参阅[高性能高级存储](../articles/storage/storage-premium-storage.md)。

### 其他事件
<a id="other-incidents" class="xliff"></a>
在极少数情况下，普遍的问题可能影响 Azure 数据中心内的多台服务器。  如果发生这种情况，Azure 团队会向受影响订阅者发送电子邮件通知。 可查看 [Azure 服务运行状况仪表板](https://www.azure.cn/support/service-dashboard/)和 Azure 门户，了解正在进行的服务中断和过去事件的状态。