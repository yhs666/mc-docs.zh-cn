---
title: 了解如何审核虚拟机的内容
description: 了解 Azure Policy 如何使用来宾配置代理审核虚拟机内部的设置。
author: DCtheGeek
ms.author: v-tawe
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: a27e02fedf7c9949bd9da856fa7be9eeaff6976a
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335811"
---
# <a name="understand-azure-policys-guest-configuration"></a>了解 Azure Policy 的来宾配置

除了审核和[修正](../how-to/remediate-resources.md) Azure 资源以外，Azure Policy 还可以审核计算机内部的设置。 验证由来宾配置扩展和客户端执行。 该扩展将通过客户端验证如下所述的设置：

- 操作系统的配置
- 应用程序配置或存在性
- 环境设置

目前，Azure Policy Guest Configuration 只会审核计算机内部的设置。 它不会应用配置。

## <a name="extension-and-client"></a>扩展和客户端

为了审核计算机内部的设置，将会启用一个[虚拟机扩展](../../../virtual-machines/extensions/overview.md)。 该扩展下载适用的策略分配和相应的配置定义。

### <a name="limits-set-on-the-extension"></a>在扩展中设置的限制

为了限制该扩展对计算机内部运行的应用程序造成影响，将不允许 Guest Configuration 的 CPU 利用率超过 5%。 内置定义和自定义定义都存在此限制。

## <a name="register-guest-configuration-resource-provider"></a>注册来宾配置资源提供程序

必须注册资源提供程序，之后才能使用来宾配置。 可以通过门户或通过 PowerShell 注册。 如果 Guest Configuration 策略的分配是通过门户完成的，则会自动注册资源提供程序。

### <a name="registration---portal"></a>注册 - 门户

若要通过 Azure 门户注册资源提供程序的来宾配置，请按照下列步骤操作：

1. 启动 Azure 门户，单击“所有服务”  。 搜索并选择“订阅”  。

1. 找到并单击要启用来宾配置的订阅。

1. 在“订阅”  页的左侧菜单中，单击“资源提供程序”  。

1. 筛选或滚动直至找到“Microsoft.GuestConfiguration”  ，然后在同一行上单击“注册”  。

### <a name="registration---powershell"></a>注册 - PowerShell

若要通过 PowerShell 注册资源提供程序的来宾配置，请运行以下命令：

```powershell
# Login first with Connect-AzAccount -EnvironmentName AzureChinaCloud command
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

## <a name="validation-tools"></a>验证工具

在计算机中，Guest Configuration 客户端使用本地工具运行审核。

下表显示了每个受支持操作系统上本地工具的列表：

|操作系统|验证工具|注释|
|-|-|-|
|Windows|[Windows PowerShell Desired State Configuration ](https://docs.microsoft.com/powershell/scripting/dsc/overview/overview) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby 和 Python 由来宾配置扩展安装。 |

### <a name="validation-frequency"></a>验证频率

来宾配置客户端每 5 分钟检查一次新内容。 在收到来宾分配后，将按 15 分钟的时间间隔检查设置。 在审核完成后，结果会立即发送到来宾配置资源提供程序。 当策略[评估触发器](../how-to/get-compliance-data.md#evaluation-triggers)执行时，会将计算机状态写入到来宾配置资源提供程序。 此项更新会导致 Azure Policy 评估 Azure 资源管理器属性。 按需 Azure Policy 评估从 Guest Configuration 资源提供程序检索最新值。 但是，它不会触发对计算机中的配置执行新的审核。

## <a name="supported-client-types"></a>支持的客户端类型

下表显示了 Azure 映像上支持的操作系统列表：

|发布者|名称|版本|
|-|-|-|
|Canonical|Ubuntu Server|14.04、16.04、18.04|
|Credativ|Debian|8、9|
|Microsoft|Windows Server|2012 Datacenter、2012 R2 Datacenter、2016 Datacenter、2019 Datacenter|
|Microsoft|Windows 客户端|Windows 10|
|OpenLogic|CentOS|7.3、7.4、7.5|
|Red Hat|Red Hat Enterprise Linux|7.4、7.5|
|Suse|SLES|12 SP3|

> [!IMPORTANT]
> Guest Configuration 可以审核运行受支持 OS 的节点。 若要审核使用自定义映像的虚拟机，需要复制 **DeployIfNotExists** 定义，并修改 **If** 节以包含你的映像属性。

### <a name="unsupported-client-types"></a>不支持的客户端类型

任何版本都不支持 Windows Server Nano Server。

## <a name="guest-configuration-extension-network-requirements"></a>Guest Configuration 扩展网络要求

若要与 Azure 中的 Guest Configuration 资源提供程序通信，计算机需要对端口 **443** 上的 Azure 数据中心拥有出站访问权限。 如果在 Azure 中使用不允许出站流量的专用虚拟网络，请使用[网络安全组](../../../virtual-network/manage-network-security-group.md#create-a-security-rule)规则配置例外。 Azure Policy Guest Configuration 目前不存在服务标记。

如需 IP 地址列表，可以下载 [Microsoft Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=41653)。 此文件每周更新，包含当前部署的范围以及即将对 IP 范围进行的更新。 只需允许对部署了 VM 的区域中的 IP 进行出站访问。

> [!NOTE]
> Azure 数据中心 IP 地址 XML 文件列出了 Microsoft Azure 数据中心使用的 IP 地址范围。 文件中包含计算、SQL 和存储范围。 每周都将发布更新的文件。 该文件反映当前已部署的范围和任何即将对 IP 范围进行的更改。 数据中心至少在一周后才会使用文件中显示的新范围。 建议每周下载新的 XML 文件。 然后，更新网站以正确地标识 Azure 中运行的服务。 Azure ExpressRoute 用户应注意，此文件过去经常在每个月的第一周更新 Azure 空间的边界网关协议 (BGP) 播发。

## <a name="guest-configuration-definition-requirements"></a>来宾配置定义要求

Guest Configuration 运行的每个审核需要两个策略定义：**DeployIfNotExists** 定义和 **AuditIfNotExists** 定义。 **DeployIfNotExists** 定义用于在计算机上准备 Guest Configuration 代理，以及其他用于支持[验证工具](#validation-tools)的组件。

“DeployIfNotExists”策略定义验证并更正以下项目  ：

- 验证是否为计算机分配了要评估的配置。 如果当前不存在任何分配，请通过以下方式获取分配并准备计算机：
  - 使用[托管标识](../../../active-directory/managed-identities-azure-resources/overview.md)对计算机进行身份验证
  - 安装 Microsoft.GuestConfiguration  扩展的最新版本
  - 安装[验证工具](#validation-tools)和依赖项（如果需要）

如果 **DeployIfNotExists**  分配不合规，可以使用[修正任务](../how-to/remediate-resources.md#create-a-remediation-task)。

一旦 **DeployIfNotExists** 分配合规，**AuditIfNotExists** 策略分配就会使用本地验证工具来确定配置分配是合规还是不合规。 验证工具向来宾配置客户端提供结果。 客户端将结果转发给来宾扩展，使其可通过来宾配置资源提供程序使用。

Azure Policy 使用来宾配置资源提供程序 complianceStatus  属性在“符合性”  节点中报告符合性。 有关详细信息，请参阅[获取符合性数据](../how-to/get-compliance-data.md)。

> [!NOTE]
> 需有 **DeployIfNotExists** 策略，才能让 **AuditIfNotExists**  策略返回结果。 如果没有 **DeployIfNotExists**，**AuditIfNotExists** 策略会将状态显示为“第 0 个，共 0 个”资源。

来宾配置的所有内置策略包含在一个计划内，以对分配中使用的定义分组。 名为“\[预览\]:  审核 Linux 和 Windows 计算机中的密码安全设置”的内置计划包含 18 个策略。 对于 Windows 有六个 DeployIfNotExists  和 AuditIfNotExists  对，对于 Linux 有三个对。 [策略定义](definition-structure.md#policy-rule)逻辑将验证是否只评估目标操作系统。

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>根据行业基线审核操作系统设置

Azure Policy 中的某个计划提供根据 Microsoft“基线”审核虚拟机中的操作系统设置的功能。 定义“\[预览\]:  审核不匹配 Azure 安全基线设置的 Windows VM”包含一组完整的审核规则，这些规则基于 Active Directory 组策略中的设置。

大多数设置以参数的形式提供。 使用此功能可以自定义要根据策略和组织要求审核的内容，或者将策略映射为行业法规标准等第三方信息。

某些参数支持整数值范围。 例如，可以使用范围运算符设置“最长密码期限”参数，以便为计算机所有者提供灵活性。 可以审核生效的“组策略”设置是否要求用户在 70 天之内，但至少在一天之后更改其密码。 如参数的信息泡泡中所述，若要为此业务策略设置有效的审核值，请将值设置为“1,70”。

如果使用 Azure 资源管理器部署模板分配策略，则可以使用参数文件从源代码管理管理这些设置。 使用 Git 之类的工具管理对审核策略的更改，并在每次签入时提供备注，以阐述为何不能将某个赋值用作预期值的理由。

#### <a name="applying-configurations-using-guest-configuration"></a>使用 Guest Configuration 应用配置

Azure Policy 的最新功能可以配置计算机内部的设置。 定义“在 Windows 计算机上配置时区”通过配置时区对计算机进行更改。 

分配以“配置”开头的定义时，还必须分配定义“部署必备组件以在 Windows VM 上启用 Guest Configuration 策略”。   如果需要，可将这些定义合并到一个计划中。

<!-- Azure Acr is not available in mc -->
<!-- #### Assigning policies to machines outside of Azure -->

### <a name="multiple-assignments"></a>多个分配

Guest Configuration 策略目前仅支持为每台计算机分配相同的来宾分配，即使策略分配使用不同的参数，也是如此。

## <a name="built-in-resource-modules"></a>内置资源模块

安装 Guest Configuration 扩展时，“GuestConfiguration”PowerShell 模块将包含在最新版本的 DSC 资源模块中。 可以使用模块页 [GuestConfiguration](https://www.powershellgallery.com/packages/GuestConfiguration/) 中的“手动下载”链接从 PowerShell 库下载此模块。 可将“.nupkg”文件格式重命名为“.zip”，以便于解压缩和查看。

## <a name="client-log-files"></a>客户端日志文件

Guest Configuration 扩展将日志文件写入以下位置：

Windows： `C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\<version>\dsc\logs\dsc.log`

Linux：`/var/lib/waagent/Microsoft.GuestConfiguration.ConfigurationforLinux-<version>/GCAgent/logs/dsc.log`

其中 `<version>` 表示当前版本号。

### <a name="collecting-logs-remotely"></a>远程收集日志

排查 Guest Configuration 配置或模块问题的第一步应该是遵循[测试 Guest Configuration 包](../how-to/guest-configuration-create.md#test-a-guest-configuration-package)中的步骤使用 `Test-GuestConfigurationPackage` cmdlet。
如果这种做法无效，收集客户端日志可能会有助于诊断问题。

#### <a name="windows"></a>Windows

若要使用 Azure VM 的“运行命令”功能从 Windows 计算机中的日志文件捕获信息，可以参考以下示例 PowerShell 脚本。 有关详细信息，请参阅[使用“运行命令”在 Windows VM 中运行 PowerShell 脚本](../../../virtual-machines/windows/run-command.md)。

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$latestVersion = Get-ChildItem -Path 'C:\Packages\Plugins\Microsoft.GuestConfiguration.ConfigurationforWindows\' | ForEach-Object {$_.FullName} | Sort-Object -Descending | Select-Object -First 1
Select-String -Path "$latestVersion\dsc\logs\dsc.log" -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

若要使用 Azure VM 的“运行命令”功能从 Linux 计算机中的日志文件捕获信息，可以参考以下示例 Bash 脚本。 有关详细信息，请参阅[使用“运行命令”在 Linux VM 中运行 shell 脚本](../../../virtual-machines/linux/run-command.md)

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
latestVersion=$(find /var/lib/waagent/ -type d -name "Microsoft.GuestConfiguration.ConfigurationforLinux-*" -maxdepth 1 -print | sort -z | sed -n 1p)
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' "$latestVersion/GCAgent/logs/dsc.log" | tail
```

## <a name="guest-configuration-samples"></a>Guest Configuration 示例

以下位置提供了 Policy Guest Configuration 的示例：

- [示例索引 - Guest Configuration](../samples/index.md#guest-configuration)
- [GitHub 存储库中的 Azure Policy 示例](https://github.com/Azure/azure-policy/tree/master/samples/GuestConfiguration)

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](../samples/index.md)中查看示例。
- 查看 [Azure Policy 定义结构](definition-structure.md)。
- 查看[了解策略效果](effects.md)。
- 了解如何[以编程方式创建策略](../how-to/programmatically-create.md)。
- 了解如何[获取合规性数据](../how-to/get-compliance-data.md)。
- 了解如何[修正不合规的资源](../how-to/remediate-resources.md)。
- 参阅[使用 Azure 管理组来组织资源](../../management-groups/overview.md)，了解什么是管理组。
