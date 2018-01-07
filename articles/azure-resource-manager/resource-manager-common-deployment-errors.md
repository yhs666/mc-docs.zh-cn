---
title: "排查常见的 Azure 部署错误 | Azure"
description: "说明如何解决使用 Azure Resource Manager 将资源部署到 Azure 时的常见错误。"
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: rockboyfor
manager: digimobile
editor: tysonn
keywords: "部署错误, azure 部署, 部署到 azure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/29/2017
ms.date: 12/25/2017
ms.author: v-yeche
ms.openlocfilehash: 039b2528d5e6a952fbd7e29dadb5ef2ad582e906
ms.sourcegitcommit: 3e0cad765e3d8a8b121ed20b6814be80fedee600
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>排查使用 Azure Resource Manager 时的常见 Azure 部署错误

本文介绍我们可能会遇到的一些常见 Azure 部署错误，并提供有关如何解决这些错误的信息。 如果找不到部署错误的错误代码，请参阅[查找错误代码](#find-error-code)。

## <a name="error-codes"></a>错误代码

| 错误代码 | 缓解措施 | 详细信息 |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | 遵循存储帐户的命名限制。 | [解析存储帐户名称](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | 查看可用的存储帐户属性。 | [storageAccounts](/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | 群集或区域没有可用的资源或无法支持所请求的 VM 大小。 稍后重试请求，或者请求不同的 VM 大小。 | [Linux 预配和分配问题](../virtual-machines/linux/troubleshoot-deployment-new-vm.md)和 [Windows 预配和分配问题](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| AnotherOperationInProgress | 等待并发操作完成。 | |
| AuthorizationFailed | 帐户或服务主体没有完成部署所需的足够访问权限。 请检查帐户所属的角色，及其与部署范围相对应的访问权限。 | [Azure 基于角色的访问控制](../active-directory/role-based-access-control-configure.md) |
| BadRequest | 所发送的部署值与资源管理器所预期的不匹配。 请检查内部状态消息，获取故障排除帮助。 | [模板参考](https://docs.microsoft.com/en-us/azure/templates/)和[支持的位置](resource-manager-templates-resources.md#location) |
| 冲突 | 资源的当前状态不允许你所请求的操作。 例如，仅当创建 VM 或该 VM 已取消分配时，才允许磁盘重设大小。 | |
| DeploymentActive | 等待目标为此资源组的并发部署完成。 | |
| DnsRecordInUse | DNS 记录名称必须唯一。 提供其他名称，或者修改现有的记录。 | |
| ImageNotFound | 检查 VM 映像设置。 | [排查 Linux 映像问题](../virtual-machines/linux/troubleshoot-deployment-new-vm.md)和[排查 Windows 映像问题](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| InUseSubnetCannotBeDeleted | 如果尝试更新某个资源时，系统却通过删除和创建该资源来处理该请求，这种情况下可能会遇到该错误。 请确保指定所有未更改的值。 | [更新资源](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | 获取相应租户的访问令牌。 只能从帐户所属的租户获取该令牌。 | |
| InvalidContentLink | 原因很可能是你尝试链接到一个不可用的嵌套模板。 请仔细检查提供给嵌套模板的 URI。 如果模板在存储帐户中存在，请确保 URI 可访问。 可能需要传递 SAS 令牌。 | [链接的模板](resource-group-linked-templates.md) |
| InvalidParameter | 为资源提供的某个值与预期的值不符。 许多不同的条件可能会导致此错误。 例如，密码可能不符合要求，或者 blob 名称可能不正确。 请检查错误消息，确定哪个值需要更正。 | |
| InvalidRequestContent | 部署值包括非预期的值，或者缺少必需的值。 确认资源类型的值。 | [模板参考](https://docs.microsoft.com/en-us/azure/templates/) |
| InvalidRequestFormat | 在执行部署时启用调试日志记录，并验证请求的内容。 | [调试日志记录](resource-manager-troubleshoot-tips.md#enable-debug-logging) |
| InvalidResourceNamespace | 检查在 type 属性中指定的资源命名空间。 | [模板参考](https://docs.microsoft.com/en-us/azure/templates/) |
| InvalidResourceReference | 资源尚不存在，或者对其进行了不正确的引用。 请检查是否需要添加依赖项， 并验证所使用的 reference 函数是否包括方案所需的参数。 | [解决依赖项问题](resource-manager-not-found-errors.md) |
| InvalidResourceType | 检查在 type 属性中指定的资源类型。 | [模板参考](https://docs.microsoft.com/en-us/azure/templates/) |
| InvalidTemplate | 检查模板语法是否存在错误。 | [解决模板无效的问题](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | 检查帐户与要部署到的资源组是否属于同一租户。 | |
| LinkedInvalidPropertyId | 无法正确解析资源的资源 ID。 请检查是否提供了资源 ID 的所有必需值，包括订阅 ID、资源组名称、资源类型、父资源名称（如果需要）、资源名称。 | |
| LocationRequired | 提供资源的位置。 | [设置位置](resource-manager-templates-resources.md#location) |
| MissingRegistrationForLocation | 检查资源提供程序注册状态，以及支持的位置。 | [解决注册问题](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | 向资源提供程序注册订阅。 | [解决注册问题](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | 检查资源提供程序注册状态。 | [解决注册问题](resource-manager-register-provider-errors.md) |
| NotFound | 你可能在尝试部署与父资源并行的依赖资源。 请检查是否需要添加依赖项。 | [解决依赖项问题](resource-manager-not-found-errors.md) |
| OperationNotAllowed | 部署所尝试的操作超出订阅、资源组或区域的配额。 请尽可能修改部署，使之保持在配额范围内。 否则，请考虑请求更改配额。 | [解决配额问题](resource-manager-quota-errors.md) |
| ParentResourceNotFound | 确保在创建子资源之前存在父资源。 | [解决父资源问题](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | 指定的 IP 地址包括 Azure 所需的地址范围。 请更改 IP 地址，避免使用保留的范围。 | [IP 地址](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | 指定的 IP 地址位于子网范围之外。 请更改 IP 地址，使之位于子网范围之内。 | [IP 地址](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | 已部署资源上的某些属性不能更改。 更新资源时，请仅更改允许的属性。 | [更新资源](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | 订阅中的某个资源策略阻止你在部署期间尝试执行的操作。 请找出阻止该操作的策略。 如果可能，请修补部署，使之符合策略的限制。 | [解决策略问题](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | 提供不包含保留名称的资源名称。 | [保留的资源名称](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | 等待删除操作完成。 | |
| ResourceGroupNotFound | 检查部署的目标资源组的名称。 它必须已存在于订阅中。 请检查订阅上下文。 | [Azure CLI](https://docs.azure.cn/zh-cn/cli/account?view=azure-cli-latest#az_account_set) [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | 部署所引用的资源不能解析。 请验证所使用的 reference 函数是否包括方案所需的参数。 | [解决引用问题](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | 部署所尝试创建的资源超出订阅、资源组或区域的配额。 请尽可能修改基础结构，使之保持在配额范围内。 否则，请考虑请求更改配额。 | [解决配额问题](resource-manager-quota-errors.md) |
| SkuNotAvailable | 选择适用于所选位置的 SKU（例如 VM 大小）。 | [解决 SKU 问题](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | 为存储帐户提供唯一名称。 | [解析存储帐户名称](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | 为存储帐户提供唯一名称。 | [解析存储帐户名称](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | 检查订阅、资源组以及尝试使用的存储帐户的名称。 | |
| SubnetsNotInSameVnet | 一个虚拟机只能有一个虚拟网络。 部署多个 NIC 时，请确保其属于同一虚拟网络。 | [多个 NIC](../virtual-machines/windows/multiple-nics.md) |

## <a name="find-error-code"></a>查找错误代码

当你在部署期间遇到错误时，资源管理器会返回错误代码。 可以通过门户、PowerShell 或 Azure CLI 查看错误消息。 外部错误消息对于故障排除而言可能过于泛泛。 请查找包含详细错误信息的内部消息。 有关详细信息，请参阅[确定错误代码](resource-manager-troubleshoot-tips.md#determine-error-code)。

## <a name="next-steps"></a>后续步骤
* 若要了解审核操作，请参阅[使用 Resource Manager 执行审核操作](resource-group-audit.md)。
* 若要了解部署期间为确定错误需要执行哪些操作，请参阅[查看部署操作](resource-manager-deployment-operations.md)。

<!--Update_Description: update meta properties, wording update -->