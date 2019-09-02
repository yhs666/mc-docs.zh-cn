## <a name="understanding-azure-resource-templates-and-resource-groups"></a>了解 Azure 资源模板和资源组

在 Microsoft Azure 中部署和运行的大多数应用程序是通过不同云资源类型的组合（例如，一个或多个 VM 和存储帐户、一个 SQL 数据库、一个虚拟网络、一个 CDN，等等）构建的。  使用 [Azure Resource Manager 模板](../articles/azure-resource-manager/resource-group-authoring-templates.md)可以集中部署和管理这些不同的资源，只需对资源和关联的配置及部署参数进行 JSON 说明即可。

定义基于 JSON 的资源模板之后，就可以执行它，并使用 PowerShell 命令将其中定义的资源部署到 Azure 中。  可以在 PowerShell 命令外壳中单独运行此 PowerShell 命令，也可以将其集成到包含其他自动化逻辑的 PowerShell 脚本中。

使用 Azure Resource Manager 模板创建的资源将部署到新的或现有的 Azure 资源组。  Azure 资源组允许以单个逻辑组的形式集中管理多个已部署的资源。 通常，一个组包含与特定应用程序相关的资源。  Azure 资源组提供一种方法来管理组/应用程序的整个生命周期，并提供用于执行以下操作的管理 API：一次性停止/启动/删除组内的所有资源，应用基于角色的访问控制 (RBAC) 规则来锁定其中的安全权限，审核操作，以及使用其他元数据来标记资源以方便跟踪。 若要详细了解 Azure 资源组，请参阅 [Azure Resource Manager 概述](../articles/azure-resource-manager/resource-group-overview.md)。 

以下自动化示例演示如何使用 Azure Resource Manager 模板，以及如何使用 PowerShell 或 CLI 部署资源组。