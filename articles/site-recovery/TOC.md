# 概述
## [什么是 Site Recovery？](site-recovery-overview.md)
## Site Recovery 的工作原理是什么？
### [Azure 到 Azure 体系结构](site-recovery-azure-to-azure-architecture.md)
### [复制到辅助站点体系结构](site-recovery-architecture-to-secondary-site.md)
## [可以保护哪些工作负荷？](site-recovery-workload.md)
## Site Recovery 支持矩阵
### [Azure 到 Azure 支持](site-recovery-support-matrix-azure-to-azure.md)
### [本地到 Azure 支持](site-recovery-support-matrix-to-azure.md)
### [本地到辅助站点支持](site-recovery-support-matrix-to-sec-site.md)
## [常见问题](site-recovery-faq.md)

# 入门
## [将 Hyper-V VM 复制到 Azure（包含 VMM）](site-recovery-vmm-to-azure.md)
## [将 Hyper-V VM 复制到 Azure](site-recovery-hyper-v-site-to-azure.md)
## [将 Hyper-V VM 复制到辅助站点（包含 VMM）](site-recovery-vmm-to-vmm.md)


# 如何
## 计划
### [Azure 复制的先决条件](site-recovery-azure-to-azure-prereq.md)
### 规划网络
#### [为 Azure 到 Azure 复制规划网络（预览）](site-recovery-azure-to-azure-networking-guidance.md)
#### [为本地计算机复制规划网络](site-recovery-network-design.md)
#### [为 Hyper-V VM 复制规划网络映射](site-recovery-network-mapping.md)
### 规划容量和可伸缩性
#### [用于 Hyper-V 复制的 Capacity Planner](site-recovery-capacity-planner.md)
### [为 VM 复制规划基于角色的访问](site-recovery-role-based-linked-access-control.md)
## 部署


## 故障转移和故障回复
### [设置恢复计划](site-recovery-create-recovery-plans.md)
#### [将 Azure Runbook 添加到恢复计划](site-recovery-runbook-automation.md)
### 运行测试故障转移
#### [运行到 Azure 的测试故障转移](site-recovery-test-failover-to-azure.md)
#### [运行 VMM 云之间的测试故障转移](site-recovery-test-failover-vmm-to-vmm.md)
### [故障转移受保护的计算机](site-recovery-failover.md)

### 从 Azure 回复故障
## 迁移
### [迁移到 Azure](site-recovery-migrate-to-azure.md)
### [在 Azure 区域之间迁移](site-recovery-migrate-azure-to-azure.md)
### [将 AWS Windows 实例迁移到 Azure](site-recovery-migrate-aws-to-azure.md)
## 工作负荷
### [Active Directory 和 DNS](site-recovery-active-directory.md)
### [复制 SQL Server](site-recovery-sql.md)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [其他工作负荷](site-recovery-workload.md#workload-summary)
## 自动复制
### [将 Hyper-V 自动复制到 Azure（不包含 VMM）](site-recovery-deploy-with-powershell-resource-manager.md)
### [将 Hyper-V 自动复制到 Azure（包含 VMM）](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [将 Hyper-V 自动复制到辅助站点（包含 VMM）](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## 管理

### [删除服务器并禁用保护](site-recovery-manage-registration-and-protection.md)
## 监视和故障排除
### [Azure 到 Azure 复制问题](site-recovery-azure-to-azure-troubleshoot-errors.md)
### [收集日志并解决本地问题](site-recovery-monitoring-and-troubleshooting.md)

# 引用
## [PowerShell](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.siterecovery)
## [PowerShell 经典](https://docs.microsoft.com/zh-cn/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/zh-cn/library/mt750497)

# 相关内容
## [Azure 自动化](https://docs.azure.cn/zh-cn/automation/)

# 资源
## [论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)
## [博客](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [价格](https://www.azure.cn/pricing/details/site-recovery/)
