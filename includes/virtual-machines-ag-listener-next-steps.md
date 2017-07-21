除了将客户端自动连接到主要副本之外，还可以使用侦听器将只读工作负载重定向到次要副本。 这可以提高整个解决方案的性能和可伸缩性。 有关详细信息，请参阅[使用 Azure AlwaysOn 可用性组侦听器的 ReadIntent 路由](https://blogs.msdn.microsoft.com/alwaysonpro/2014/03/31/use-readintent-routing-with-azure-alwayson-availability-group-listener/)。

> [!NOTE]
> 有关 Azure 侦听器的故障排除提示，请参阅 AlwaysOn 支持团队[博客](http://blogs.msdn.com/b/alwaysonpro/)中的[排查 Azure 中的可用性组侦听器问题](https://blogs.msdn.microsoft.com/alwaysonpro/2017/02/22/troubleshooting-internal-load-balancer-listener-connectivity-in-azure)。
> 
> 

有关在 Azure 中使用 SQL Server 的其他信息，请参阅 [SQL Server on Azure Virtual Machines](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)（Azure 虚拟机上的 SQL Server）。