---
title: 如何使用服务管理 API (Python) - 功能指南
description: 了解如何以编程方式执行 Python 提供的常见服务管理任务。
services: cloud-services
documentationCenter: python
authors: lmazuel
manager: wpickett
editor: ''

ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
wacn.date: 12/12/2016
ms.author: lmazuel
---

# 如何从 Python 使用服务管理

> [!NOTE]
> 服务管理 API 将被新的资源管理 API 替换，后者目前已发布预览版。请参阅 [Azure 资源管理文档](http://azure-sdk-for-python.readthedocs.org/)，详细了解如何使用 Python 推出的全新资源管理 API。

本指南说明如何以编程方式从 Python 执行常见服务管理任务。[用于 Python 的 Azure SDK](https://github.com/Azure/azure-sdk-for-python) 中的 **ServiceManagementService** 类支持以编程方式访问 [Azure 经典管理门户][management-portal]中提供的众多与服务管理相关的功能（例如**创建、更新和删除云服务、部署、数据管理服务和虚拟机**）。此功能可用于构建需要以编程方式访问服务管理的应用程序。

## <a name="WhatIs"> </a>什么是服务管理？
利用服务管理 API，可以编程方式访问通过[经典管理门户][management-portal]提供的众多服务管理功能。Azure SDK for Python 允许你管理云服务和存储帐户。

若要使用服务管理 API，需要[创建 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="Concepts"> </a>概念
Azure SDK for Python 可包装 [Azure 服务管理 API][svc-mgmt-rest-api]，后者是 REST API。所有 API 操作都通过 SSL 执行，并使用 X.509 v3 证书进行互相验证。可以从在 Azure 中运行的服务访问管理服务，或直接通过 Internet 从可发送 HTTPS 请求和接收 HTTPS 响应的任意应用程序访问管理服务。

## <a name="Installation"></a>安装

`azure-servicemanagement-legacy` 包中提供本文介绍的所有功能，可以使用 pip 安装该包。有关安装的更多详细信息（例如，如果刚开始接触 Python），请参阅此文：[安装 Python 和 Azure SDK](../python-how-to-install.md)。

## <a name="Connect"> </a>如何：连接到服务管理
若要连接到服务管理终结点，你需要 Azure 订阅 ID 和有效管理证书。可以通过 [Azure 经典管理门户][management-portal]获取订阅 ID。

> [!NOTE]
> 从 Azure SDK for Python v0.8.0 开始，在 Windows 上运行时，可以使用通过 OpenSSL 创建的证书。需要 Python 2.7.4 或更高版本。我们建议用户使用 OpenSSL 而不是 .pfx，因为将来可能会取消对 .pfx 证书的支持。

### Windows/Mac/Linux 上的管理证书 (OpenSSL)
可使用 [OpenSSL](http://www.openssl.org/) 创建管理证书。你实际上需要创建两个证书，一个用于服务器（`.cer` 文件），一个用于客户端（`.pem` 文件）。若要创建 `.pem` 文件，请执行以下代码：

```
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
```

若要创建 `.cer` 证书，请执行以下代码：

```
openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer
```

有关 Azure 证书的详细信息，请参阅 [Azure 云服务证书概述](./cloud-services-certs-create.md)。

创建这些文件后，需要通过 [Azure 经典管理门户][management-portal]中“设置”选项卡的“上传”操作将 `.cer` 文件上传到 Azure，并且需要记下 `.pem` 文件的保存位置。

在你获取订阅 ID，创建证书并将 `.cer` 文件上载到 Azure 之后，你可以通过将订阅 ID 和 `.pem` 文件的路径传递给 **ServiceManagementService** 来连接到 Azure 管理终结点：

```
from azure import *
from azure.servicemanagement import *

subscription_id = '<your_subscription_id>'
certificate_path = '<path_to_.pem_certificate>'

sms = ServiceManagementService(subscription_id, certificate_path)
```

在前面的示例中，`sms` 是一个 **ServiceManagementService** 对象。**ServiceManagementService** 类是用于管理 Azure 服务的主类。

### Windows 上的管理证书 (MakeCert)

可使用 `makecert.exe` 在计算机上创建自签名管理证书。以**管理员**身份打开 **Visual Studio 命令提示符**并使用以下命令，用要使用的证书名称替换 *AzureCertificate*。

```
makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"
```

该命令将创建 `.cer` 文件，然后将该文件安装到“个人”证书存储区中。有关详细信息，请参阅 [Azure 云服务证书概述](./cloud-services-certs-create.md)。

创建证书后，需通过[经典管理门户][management-portal]“设置”选项卡的“上传”操作，将 `.cer` 文件上传到 Azure。

获取订阅 ID、创建证书并将 `.cer` 文件上传到 Azure 后，可将订阅 ID 以及“个人”证书存储区中的证书位置传递到 **ServiceManagementService**（并用证书名称替代 *AzureCertificate*），连接到 Azure 管理终结点：

```
from azure import *
from azure.servicemanagement import *

subscription_id = '<your_subscription_id>'
certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

sms = ServiceManagementService(subscription_id, certificate_path)
```

在上面的示例中，`sms` 是一个 **ServiceManagementService** 对象。**ServiceManagementService** 类是用于管理 Azure 服务的主类。

## <a name="ListAvailableLocations"> </a>如何：列出可用位置

若要列出可用于托管服务的位置，请使用 **list\_locations** 方法：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

result = sms.list_locations()
for location in result:
    print(location.name)
```

创建云服务或存储服务时，需要提供有效位置。**list\_locations** 方法将始终返回当前可用位置的最新列表。截止到本文撰写时为止，可用位置为：

- 中国东部 
- 中国北部

## <a name="CreateCloudService"> </a>如何：创建云服务

创建应用程序并在 Azure 中运行该应用程序时，相关代码和配置统称为 Azure [云服务]（在早期版本的 Azure 中称为*托管服务*）。**create\_hosted\_service** 方法允许通过提供托管服务名称（它在 Azure 中必须是唯一的）、标签（自动编码为 base64）、说明和位置来创建新的托管服务。

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

name = 'myhostedservice'
label = 'myhostedservice'
desc = 'my hosted service'
location = 'China East'

sms.create_hosted_service(name, label, desc, location)
```

可使用 **list\_hosted\_services** 方法列出订阅的所有托管服务：

```
result = sms.list_hosted_services()

for hosted_service in result:
    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)
    print('')
```

如果希望获得有关特定托管服务的信息，可以通过将托管服务名称传递给 **get\_hosted\_service\_properties** 方法来实现此目的：

```
hosted_service = sms.get_hosted_service_properties('myhostedservice')

print('Service name: ' + hosted_service.service_name)
print('Management URL: ' + hosted_service.url)
print('Location: ' + hosted_service.hosted_service_properties.location)
```

创建云服务后，可使用 **create\_deployment** 方法将代码部署到服务。

## <a name="DeleteCloudService"> </a>如何：删除云服务

可通过将服务名称传递给 **delete\_hosted\_service** 方法来删除云服务：

```
sms.delete_hosted_service('myhostedservice')
```

请注意，必须先删除服务的所有部署，然后才能删除服务。（有关详细信息，请参阅[如何：删除部署](#DeleteDeployment)。）

## <a name="DeleteDeployment"> </a>如何：删除部署

若要删除部署，请使用 **delete\_deployment** 方法。下面的示例演示如何删除名为 `v1` 的部署。

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

sms.delete_deployment('myhostedservice', 'v1')
```

## <a name="CreateStorageService"> </a>如何：创建存储服务

利用[存储服务](../storage/storage-create-storage-account.md)，可以访问 Azure [Blob](../storage/storage-python-how-to-use-blob-storage.md)、[表](../storage/storage-python-how-to-use-table-storage.md)和[队列](../storage/storage-python-how-to-use-queue-storage.md)。若要创建存储服务，需要服务名称（3 至 24 个小写字符，且在 Azure 中是唯一的）、说明、标签（最多 100 个字符，自动编码为 base64）以及位置。下面的示例演示如何通过指定位置来创建存储服务。

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

name = 'mystorageaccount'
label = 'mystorageaccount'
location = 'China East'
desc = 'My storage account description.'

result = sms.create_storage_account(name, desc, label, location=location)

operation_result = sms.get_operation_status(result.request_id)
print('Operation status: ' + operation_result.status)
```

请注意，在上面的示例中，可以通过将 **create\_storage\_account** 返回的结果传递给 **get\_operation\_status** 方法来检索 **create\_storage\_account** 操作的状态。

可使用 **list\_storage\_accounts** 方法列出存储帐户及其属性：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

result = sms.list_storage_accounts()
for account in result:
    print('Service name: ' + account.service_name)
    print('Location: ' + account.storage_service_properties.location)
    print('')
```

## <a name="DeleteStorageService"> </a>如何：删除存储服务

可通过将存储服务名称传递给 **delete\_storage\_account** 方法来删除存储服务。删除存储服务会删除该服务中存储的所有数据（Blob、表和队列）。

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

sms.delete_storage_account('mystorageaccount')
```

## <a name="ListOperatingSystems"> </a>如何：列出可用操作系统

若要列出可用于托管服务的操作系统，请使用 **list\_operating\_systems** 方法：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

result = sms.list_operating_systems()

for os in result:
    print('OS: ' + os.label)
    print('Family: ' + os.family_label)
    print('Active: ' + str(os.is_active))
```

或者，可使用 **list\_operating\_system\_families** 方法，按系列对操作系统进行分组：

```
result = sms.list_operating_system_families()

for family in result:
    print('Family: ' + family.label)
    for os in family.operating_systems:
        if os.is_active:
            print('OS: ' + os.label)
            print('Version: ' + os.version)
    print('')
```

## <a name="CreateVMImage"> </a>如何：创建操作系统映像

若要将操作系统映像添加到映像存储库，请使用 **add\_os\_image** 方法：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

name = 'mycentos'
label = 'mycentos'
os = 'Linux' # Linux or Windows
media_link = 'url_to_storage_blob_for_source_image_vhd'

result = sms.add_os_image(label, media_link, name, os)

operation_result = sms.get_operation_status(result.request_id)
print('Operation status: ' + operation_result.status)
```

若要列出可用的操作系统映像，请使用 **list\_os\_images** 方法。这包括所有平台映像和用户映像：

```
result = sms.list_os_images()

for image in result:
    print('Name: ' + image.name)
    print('Label: ' + image.label)
    print('OS: ' + image.os)
    print('Category: ' + image.category)
    print('Description: ' + image.description)
    print('Location: ' + image.location)
    print('Media link: ' + image.media_link)
    print('')
```

## <a name="DeleteVMImage"> </a>如何：删除操作系统映像

若要删除用户映像，请使用 **delete\_os\_image** 方法：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

result = sms.delete_os_image('mycentos')

operation_result = sms.get_operation_status(result.request_id)
print('Operation status: ' + operation_result.status)
```

## <a name="CreateVM"> </a>如何：创建虚拟机

若要创建虚拟机，首先需要创建[云服务](#CreateCloudService)。然后使用 **create\_virtual\_machine\_deployment** 方法来创建虚拟机部署：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

name = 'myvm'
location = 'China East'

#Set the location
sms.create_hosted_service(service_name=name,
    label=name,
    location=location)

# Name of an os image as returned by list_os_images
image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-zh-cn-30GB.vhd'

# Destination storage account container/blob where the VM disk
# will be created
media_link = 'url_to_target_storage_blob_for_vm_hd'

# Linux VM configuration, you can use WindowsConfigurationSet
# for a Windows VM instead
linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

os_hd = OSVirtualHardDisk(image_name, media_link)

sms.create_virtual_machine_deployment(service_name=name,
    deployment_name=name,
    deployment_slot='production',
    label=name,
    role_name=name,
    system_config=linux_config,
    os_virtual_hard_disk=os_hd,
    role_size='Small')
```

## <a name="DeleteVM"> </a>如何：删除虚拟机

若要删除虚拟机，请首先使用 **delete\_deployment** 方法来删除部署：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

sms.delete_deployment(service_name='myvm',
    deployment_name='myvm')
```

然后可以使用 **delete\_hosted\_service** 方法来删除云服务：

```
sms.delete_hosted_service(service_name='myvm')
```

##如何：基于捕获的虚拟机映像创建虚拟机

若要捕获 VM 映像，请先调用 **capture\_vm\_image** 方法：

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

# replace the below three parameters with actual values
hosted_service_name = 'hs1'
deployment_name = 'dep1'
vm_name = 'vm1'

image_name = vm_name + 'image'
image = CaptureRoleAsVMImage	('Specialized',
    image_name,
    image_name + 'label',
    image_name + 'description',
    'english',
    'mygroup')

result = sms.capture_vm_image(
        hosted_service_name,
        deployment_name,
        vm_name,
        image
    )
```

接下来，为确保成功捕获映像，请使用 **list\_vm\_images** API 并确保映像显示在结果中：

```
images = sms.list_vm_images()
```

最后，为使用捕获的映像创建虚拟机，请依旧使用 **create\_virtual\_machine\_deployment** 方法，但这次传入 vm\_image\_name

```
from azure import *
from azure.servicemanagement import *

sms = ServiceManagementService(subscription_id, certificate_path)

name = 'myvm'
location = 'China North'

#Set the location
sms.create_hosted_service(service_name=name,
    label=name,
    location=location)

sms.create_virtual_machine_deployment(service_name=name,
    deployment_name=name,
    deployment_slot='production',
    label=name,
    role_name=name,
    system_config=linux_config,
    os_virtual_hard_disk=None,
    role_size='Small',
    vm_image_name = image_name)
```

若要了解有关如何捕获 Linux 虚拟机的详细信息，请参阅[如何捕获 Linux 虚拟机](../virtual-machines/virtual-machines-linux-classic-capture-image.md)。

若要了解有关如何捕获 Windows 虚拟机的详细信息，请参阅[如何捕获 Windows 虚拟机](../virtual-machines/virtual-machines-windows-classic-capture-image.md)。

## <a name="What's Next"></a>后续步骤

既已了解有关服务管理的基础知识，现可访问 [Azure Python SDK 的完整 API 参考文档](http://azure-sdk-for-python.readthedocs.org/)，轻松执行复杂任务来管理 Python 应用程序。

有关详细信息，请参阅 [Python 开发人员中心](https://www.azure.cn/develop/python/)。

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.cn/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/zh-cn/library/windowsazure/ee460799.aspx

[download-SDK-Python]: ../python-how-to-install.md
[云服务]: ./index.md
[service package]: http://msdn.microsoft.com/zh-cn/library/windowsazure/jj155995.aspx
[Azure PowerShell cmdlets]: ../powershell-install-configure.md
[cspack commandline tool]: http://msdn.microsoft.com/zh-cn/library/windowsazure/gg432988.aspx
[Deploying an Azure Service]: http://msdn.microsoft.com/zh-cn/library/windowsazure/gg433027.aspx
[存储服务]: ../storage/storage-introduction.md
[azure-blobs]: ../storage/storage-python-how-to-use-blob-storage.md
[azure-tables]: ../storage/storage-python-how-to-use-table-storage.md
[azure-queues]: ../storage/storage-python-how-to-use-queue-storage.md
[Azure Service Configuration Schema (.cscfg)]: http://msdn.microsoft.com/zh-cn/library/windowsazure/ee758710.aspx
[Cloud Services]: http://msdn.microsoft.com/zh-cn/library/windowsazure/jj155995.aspx
[Virtual Machines]: http://msdn.microsoft.com/zh-cn/library/windowsazure/jj156003.aspx

<!---HONumber=Mooncake_Quality_Review_1118_2016-->