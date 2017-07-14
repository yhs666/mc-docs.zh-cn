---
title: "在 Azure 中使用 Python 创建 Windows VM | Azure"
description: "了解如何使用 Python 在 Azure 中创建 Windows VM。"
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 06/05/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: 280523411728b6699b1f5cef9ee1b5215ccb024d
ms.sourcegitcommit: 51a25dbbf5f32fe524860b1bb107108122b47bf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# 在 Azure 中使用 Python 创建 Windows VM
<a id="create-a-windows-vm-in-azure-using-python" class="xliff"></a>

[Azure 虚拟机](overview.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json) (VM) 需要多个支持性 Azure 资源。 本文介绍如何使用 Python 创建和删除 VM 资源。 你将学习如何执行以下操作：

> [!div class="checklist"]
> * 创建 Visual Studio 项目
> * 安装包
> * 添加用于创建凭据的代码
> * 添加用于创建资源的代码
> * 添加用于删除资源的代码
> * 运行应用程序

完成这些步骤大约需要 20 分钟。

## 创建 Visual Studio 项目
<a id="create-a-visual-studio-project" class="xliff"></a>

1. 如果尚未安装，请安装 [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio)。 在“工作负荷”页上选择“Python 开发”，然后单击“安装”。 在摘要中，可以看到系统已自动选择“Python 3 64 位 (3.6.0)”。 如果已安装 Visual Studio，则可以使用 Visual Studio 启动器添加 Python 工作负荷。
2. 安装并启动 Visual Studio 后, 单击“文件” > “新建” > “项目”。
3. 单击“模板” > “Python” > “Python 应用程序”，输入“myPythonProject” 作为项目名称并选择该项目的位置，然后单击“确定”。

## 安装包
<a id="install-packages" class="xliff"></a>

1. 在解决方案资源管理器的“myPythonProject”下，右键单击“Python 环境”，然后选择“添加虚拟环境”。
2. 在“添加虚拟环境”屏幕上，接受默认名称“env”，确保已选择“Python 3.6（64 位）”作为基础解释器，然后单击“创建”。
3. 右键单击所创建的 env 环境，然后单击“安装 Python 包”，并在搜索框中输入“azure”，然后按 Enter 键。

应在输出窗口中看到 azure 包已成功安装。 

## 添加用于创建凭据的代码
<a id="add-code-to-create-credentials" class="xliff"></a>

在开始此步骤前，请确保具有 [Active Directory 服务主体](../../azure-resource-manager/resource-group-create-service-principal-portal.md)。 此外，应记下应用程序 ID、身份验证密钥和租户 ID，以便在后面的步骤中使用。

1. 打开创建的 myPythonProject.py 文件，然后添加此代码，使应用程序运行：

    ```python
    if __name__ == "__main__":
    ```

2. 若要导入所需代码，请将以下语句添加到 .py 文件的顶部：

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    ```

3. 接下来在 .py 文件的导入语句后添加变量，指定代码中使用的公用值：

    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'chinanorth'
    ```

    将 subscription-id 替换为订阅标识符。

4. 若要创建发出请求所需的 Active Directory 凭据，请将此函数添加到 .py 文件中的变量之后：

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id',
            china = True
        )

        return credentials
    ```

    将 application-id、authentication-key 和 tenant-id 替换为前面在创建 Azure Active Directory 服务主体时收集的值。

5. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    credentials = get_credentials()
    ```

## 添加用于创建资源的代码
<a id="add-code-to-create-resources" class="xliff"></a>

### 初始化管理客户端
<a id="initialize-management-clients" class="xliff"></a>

在 Azure 中使用 Python SDK 创建和管理资源时需要用到管理客户端。 若要创建管理客户端，请在 .py 文件末尾处的 if 语句下添加此代码：

```python
resource_group_client = ResourceManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
network_client = NetworkManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
compute_client = ComputeManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
```

### 创建 VM 和支持资源
<a id="create-the-vm-and-supporting-resources" class="xliff"></a>

必须在[资源组](../../azure-resource-manager/resource-group-overview.md)中包含所有资源。

1. 若要创建资源组，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter to continue...')
    ```

使用[可用性集](tutorial-availability-sets.md)可以更方便地维护应用程序所用的虚拟机。

1. 若要创建可用性集，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_availability_set(compute_client):
        avset_params = {
            'location': LOCATION,
            'sku': { 'name': 'Aligned' },
            'platform_fault_domain_count': 3
        }
        availability_set_result = compute_client.availability_sets.create_or_update(
            GROUP_NAME,
            'myAVSet',
            avset_params
        )
    ```

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter to continue...')
    ```

与虚拟机通信需要[公共 IP 地址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)。

1. 若要创建虚拟机的公共 IP 地址，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_public_ip_address(network_client):
        public_ip_addess_params = {
            'location': LOCATION,
            'public_ip_allocation_method': 'Dynamic'
        }
        creation_result = network_client.public_ip_addresses.create_or_update(
            GROUP_NAME,
            'myIPAddress',
            public_ip_addess_params
        )

        return creation_result.result()
    ```

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

虚拟机必须在[虚拟网络](../../virtual-network/virtual-networks-overview.md)的子网中。

1. 若要创建虚拟网络，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_vnet(network_client):
        vnet_params = {
            'location': LOCATION,
            'address_space': {
                'address_prefixes': ['10.0.0.0/16']
            }
        }
        creation_result = network_client.virtual_networks.create_or_update(
            GROUP_NAME,
            'myVNet',
            vnet_params
        )
        return creation_result.result()
    ```

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

3. 若要将子网添加到虚拟网络，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_subnet(network_client):
        subnet_params = {
            'address_prefix': '10.0.0.0/24'
        }
        creation_result = network_client.subnets.create_or_update(
            GROUP_NAME,
            'myVNet',
            'mySubnet',
            subnet_params
        )

        return creation_result.result()
    ```

4. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

虚拟机需要使用网络接口在虚拟网络上通信。

1. 若要创建网络接口，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_nic(network_client):
        subnet_info = network_client.subnets.get(
            GROUP_NAME, 
            'myVNet', 
            'mySubnet'
        )
        publicIPAddress = network_client.public_ip_addresses.get(
            GROUP_NAME,
            'myIPAddress'
        )
        nic_params = {
            'location': LOCATION,
            'ip_configurations': [{
                'name': 'myIPConfig',
                'public_ip_address': publicIPAddress,
                'subnet': {
                    'id': subnet_info.id
                }
            }]
        }
        creation_result = network_client.network_interfaces.create_or_update(
            GROUP_NAME,
            'myNic',
            nic_params
        )

        return creation_result.result()
    ```

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

创建所有支持资源后，即可创建虚拟机。

1. 若要创建虚拟机，请在 .py 文件中的变量后添加此函数：

    ```python
    def create_vm(network_client, compute_client):  
        nic = network_client.network_interfaces.get(
            GROUP_NAME, 
            'myNic'
        )
        avset = compute_client.availability_sets.get(
            GROUP_NAME,
            'myAVSet'
        )
        vm_parameters = {
            'location': LOCATION,
            'os_profile': {
                'computer_name': 'myVM',
                'admin_username': 'azureuser',
                'admin_password': 'Azure12345678'
            },
            'hardware_profile': {
                'vm_size': 'Standard_DS1'
            },
            'storage_profile': {
                'image_reference': {
                    'publisher': 'MicrosoftWindowsServer',
                    'offer': 'WindowsServer',
                    'sku': '2012-R2-Datacenter',
                    'version': 'latest'
                }
            },
            'network_profile': {
                'network_interfaces': [{
                    'id': nic.id
                }]
            },
            'availability_set': {
                'id': avset.id
            }
        }
        creation_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            'myVM', 
            vm_parameters
        )

        return creation_result.result()
    ```

    > [!NOTE]
    > 本教程创建运行 Windows Server 操作系统版本的虚拟机。 若要详细了解如何选择其他映像，请参阅[使用 Windows PowerShell 和 Azure CLI 来导航和选择 Azure 虚拟机映像](../linux/cli-ps-findimage.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。
    > 
    > 

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

## 添加用于删除资源的代码
<a id="add-code-to-delete-resources" class="xliff"></a>

由于需要为 Azure 中使用的资源付费，因此，最好删除不再需要的资源。 如果要删除虚拟机和所有支持资源，只需删除资源组。

1. 若要删除资源组和所有资源，请在 .py 文件中的变量后添加此函数：

    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. 若要调用之前添加的函数，请在 .py 文件末尾处的 if 语句下添加此代码：

    ```python
    delete_resources(resource_group_client)
    ```

3. 保存 myPythonProject.py。

## 运行应用程序
<a id="run-the-application" class="xliff"></a>

1. 若要运行控制台应用程序，请在 Visual Studio 中单击“启动”。

2. 所有资源的状态都返回后，请按 Enter。 在状态信息中，应会看到“成功”预配状态。 创建虚拟机后，可以删除所创建的所有资源。 在按 **Enter** 开始删除资源之前，可能需要在 Azure 门户中花几分钟时间来验证资源的创建。 如果已打开 Azure 门户，则可能需要刷新边栏选项卡才能看到新资源。  

    控制台应用程序从头到尾完成运行大约需要五分钟时间。 应用程序完成运行之后，可能需要花费几分钟时间来删除所有资源和资源组。

## 后续步骤
<a id="next-steps" class="xliff"></a>

- 如果部署出现问题，请查看[使用 Azure 门户对资源组部署进行故障排除](../../resource-manager-troubleshoot-deployments-portal.md)
- 查看[使用 Azure Resource Manager 和 PowerShell 管理虚拟机](ps-manage.md)，了解如何管理创建的虚拟机。
- 参考 [使用 Resource Manager 模板创建 Windows 虚拟机](ps-template.md)