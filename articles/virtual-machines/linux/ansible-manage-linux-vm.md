---
title: 快速入门 - 使用 Ansible 管理 Azure 中的 Linux 虚拟机 | Azure
description: 在本快速入门中，你将了解如何使用 Ansible 管理 Azure 中的 Linux 虚拟机
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: rockboyfor
manager: digimobile
ms.author: v-yeche
origin.date: 04/30/2019
ms.date: 07/01/2019
ms.openlocfilehash: e91b2d96c8ce9df20968b296a140ea49a165c3aa
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67570330"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>快速入门：使用 Ansible 管理 Azure 中的 Linux 虚拟机

使用 Ansible 可以在环境中自动部署和配置资源。 在本文中，你将使用 Ansible playbook 来启动和停止 Linux 虚拟机。 

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>停止虚拟机

在本部分中，你将使用 Ansible 解除分配（停止）Azure 虚拟机。

<!--MOONCAKE: CUSTOMIZE-->

1. 登录到 [Azure 门户](https://portal.azure.cn)。

    <!--Not Available on [Cloud Shell](/cloud-shell/overview)-->

1.  连接已成功安装 ansible 的 linux 虚拟机。

    ```
    ssh <your-linux-account>@<your-linux-public-ip-address>
    ```

1. 创建一个名为 `azure-vm-stop.yml` 的文件，并在编辑器中将其打开：

    ```azurecli
    vi azure-vm-stop.yml
    ```

1.  按 **I** 键进入插入模式。

1.  将以下示例代码粘贴到编辑器中：

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Stop virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
            allocated: no
    ```
    
1. 将占位符 `{{ resource_group_name }}` 和 `{{ vm_name }}` 替换成自己的值。

1.  按 **Esc** 键退出插入模式。

1.  保存文件，然后输入以下命令退出 vi 编辑器：

    ```bash
    :wq
    ```

1. 使用 `ansible-playbook` 命令运行 playbook：

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. 运行 playbook 后，将看到类似于以下结果的输出：

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>启动虚拟机

在本部分中，你将使用 Ansible 启动已解除分配（停止）的 Azure 虚拟机。

1. 登录到 [Azure 门户](https://portal.azure.cn)。

1. 连接已成功安装 ansible 的 linux 虚拟机。

    ```
    ssh <your-linux-account>@<your-linux-public-ip-address>
    ```

1.  创建一个名为 `azure-vm-start.yml` 的文件，并在编辑器中将其打开：

    ```azurecli
    vi azure-vm-start.yml
    ```

1.  按 **I** 键进入插入模式。

1.  将以下示例代码粘贴到编辑器中：

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Start virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
    ```

1. 将占位符 `{{ resource_group_name }}` 和 `{{ vm_name }}` 替换成自己的值。

1.  按 **Esc** 键退出插入模式。

1.  保存文件，然后输入以下命令退出 vi 编辑器：

    ```bash
    :wq
    ```

1. 使用 `ansible-playbook` 命令运行 playbook：

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. 运行 playbook 后，可看到类似于以下结果的输出：

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

<!-- Not Available on ## Next steps-->

<!-- Update_Description: update meta properties, wording update -->