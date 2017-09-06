# 概述
## [什么是 Resource Manager？](resource-group-overview.md)
## [资源提供程序和类型](resource-manager-supported-services.md)
## [Resource Manager 部署和经典部署](resource-manager-deployment-model.md)
## [订阅管理](resource-manager-subscription-governance.md)
<!-- Not Available ## [Managed Applications](managed-application-overview.md)-->

# 入门
## [导出模板](resource-manager-export-template.md)
## [创建和部署模板](resource-manager-create-first-template.md)
## [Visual Studio 与 Resource Manager](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# 示例
## [代码示例](https://azure.microsoft.com/resources/samples/?service=azure-resource-manager)
## PowerShell
### [部署模板](resource-manager-samples-powershell-deploy.md)

## Azure CLI
### [部署模板](resource-manager-samples-cli-deploy.md)

# 如何
## 创建模板
### [模板最佳实践](resource-manager-template-best-practices.md)
### [模板节](resource-group-authoring-templates.md)
### [链接到其他模板](resource-group-linked-templates.md)
### [定义资源之间的依赖关系](resource-group-define-dependencies.md)
### [创建多个实例](resource-group-create-multiple.md)
### [设置位置](resource-manager-template-location.md)
### [分配标记](resource-manager-template-tags.md)
### [设置子资源名称和类型](resource-manager-template-child-resource.md)
### [更新资源](resource-manager-update.md)
<!-- Not Availble ### [Use objects for parameters](resource-manager-objects-as-parameters.md) -->
### [在链接模板之间共享状态](best-practices-resource-manager-state.md)
### [用于设计模板的模式](best-practices-resource-manager-design-templates.md)

## 部署
### PowerShell
#### [部署模板](resource-group-template-deploy.md)
#### [使用 SAS 令牌部署专用模板](resource-manager-powershell-sas-token.md)
#### [导出模板并进行重新部署](resource-manager-export-template-powershell.md)
### Azure CLI
#### [部署模板](resource-group-template-deploy-cli.md)
#### [使用 SAS 令牌部署专用模板](resource-manager-cli-sas-token.md)
#### [导出模板并进行重新部署](resource-manager-export-template-cli.md)
### [门户](resource-group-template-deploy-portal.md)
### [REST API](resource-group-template-deploy-rest.md)
### [跨资源组部署](resource-manager-cross-resource-group-deployment.md)
### [与 Visual Studio Team Services 的持续集成](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure-resource-manager%2ftoc.json)
### [在部署期间传递安全值](resource-manager-keyvault-parameter.md)

## 管理
### [PowerShell](powershell-azure-resource-manager.md)
### [Azure CLI](xplat-cli-azure-resource-manager.md)
### [门户](resource-group-portal.md)
### [REST API](resource-manager-rest-api.md)
### [使用标记来组织资源](resource-group-using-tags.md)
### [将资源移到新组或订阅](resource-group-move-resources.md)
### [管理示例](resource-manager-subscription-examples.md)

## 控制访问
### 创建服务主体
#### [PowerShell](resource-group-authenticate-service-principal.md)
#### [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Azure CLI 1.0](resource-group-authenticate-service-principal-cli.md)
#### [门户](resource-group-create-service-principal-portal.md)
### [用于访问订阅的身份验证 API](resource-manager-api-authentication.md)
### [锁定资源](resource-group-lock-resources.md)

## 设置资源策略
### [什么是资源策略？](resource-manager-policy.md)
<!-- Not Available ### [Use portal to assign policy](resource-manager-policy-portal.md) -->
<!-- Not Available ### [Use scripts to assign policy](resource-manager-policy-create-assign.md) -->
### 示例
<!-- Not Available #### [Resource tags](resource-manager-policy-tags.md) -->
<!-- Not Available #### [Naming conventions](resource-manager-policy-naming-convention.md) -->
<!-- Not Available #### [Network](resource-manager-policy-network.md) -->
<!-- Not Available #### [Storage](resource-manager-policy-storage.md) -->
#### [Linux VM](../virtual-machines/linux/policy.md?toc=%2fazure-resource-manager%2ftoc.json)
#### [Windows VM](../virtual-machines/windows/policy.md?toc=%2fazure-resource-manager%2ftoc.json)

<!-- Not Available ## Use managed applications -->
<!-- Not Available ### [Publish service catalog application](managed-application-publishing.md) -->
<!-- Not Available ### [Consume service catalog application](managed-application-consumption.md) -->
<!-- Not Available ### [Create UI definitions](managed-application-createuidefinition-overview.md) -->

## 审核
### [查看活动日志](resource-group-audit.md)
### [查看部署操作](resource-manager-deployment-operations.md)

## 故障排除
### [常见部署错误](resource-manager-common-deployment-errors.md)
### [了解部署错误](resource-manager-troubleshoot-tips.md)
### [RequestDisallowedByPolicy 错误](resource-manager-policy-requestdisallowedbypolicy-error.md)
### 虚拟机部署错误
#### [Linux](../virtual-machines/linux/troubleshoot-deploy-vm.md)
#### [Windows](../virtual-machines/windows/troubleshoot-deploy-vm.md)

# 引用
<!-- Not Available ## [Template format](/templates/) -->
## [模板函数](resource-group-template-functions.md)
### [数组和对象函数](resource-group-template-functions-array.md)
### [比较函数](resource-group-template-functions-comparison.md)
### [部署函数](resource-group-template-functions-deployment.md)
### [逻辑函数](resource-group-template-functions-logical.md)
### [数值函数](resource-group-template-functions-numeric.md)
### [资源函数](resource-group-template-functions-resource.md)
### [字符串函数](resource-group-template-functions-string.md)
<!-- Not Available ## [UI definition functions](managed-application-createuidefinition-functions.md) -->
<!-- Not Available ## [UI definition elements](managed-application-createuidefinition-elements.md) -->
<!-- Not Available ### [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md) -->
<!-- Not Available ### [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md) -->
<!-- Not Available ### [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md) -->
<!-- Not Available ### [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md) -->
<!-- Not Available ### [Microsoft.Common.Section](managed-application-microsoft-common-section.md) -->
<!-- Not Available ### [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md) -->
<!-- Not Available ### [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md) -->
<!-- Not Available ### [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md) -->
<!-- Not Available ### [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md) -->
<!-- Not Available ### [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md) -->
<!-- Not Available ### [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md) -->
<!-- Not Available ### [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md) -->
<!-- Not Available ### [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md) -->
## [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.resources)
## [Azure CLI](https://docs.microsoft.com/cli/azure/resource)
## [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](https://docs.azure.cn/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](https://docs.microsoft.com/rest/api/resources/)

# 资源
<!-- Not Available ## [Service updates](https://azure.microsoft.com/updates/?product=azure-resource-manager) -->
## [定价计算器](https://www.azure.cn/pricing/calculator/)
## [服务更新](https://www.azure.cn/what-is-new/)
## [堆栈溢出](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [限制请求](resource-manager-request-limits.md)
## [跟踪异步操作](resource-manager-async-operations.md)
<!-- Not Available ## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager) -->
