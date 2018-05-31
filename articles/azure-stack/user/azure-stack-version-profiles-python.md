---
title: 在 Azure Stack 中将 API 版本配置文件与 Python 配合使用 | Microsoft Docs
description: 了解如何在 Azure Stack 中将 API 版本配置文件与 Python 配合使用。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/30/2018
ms.date: 05/23/2018
ms.author: v-junlch
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: ef3df889efc68a3440edecfeb7d470de95e846cf
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475122"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>在 Azure Stack 中将 API 版本配置文件与 Python 配合使用

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

## <a name="python-samples-for-azure-stack"></a>适用于 Azure Stack 的 Python 示例 

可以使用以下代码示例对 Azure Stack 中的虚拟机执行常见管理任务。

代码示例展示了如何执行以下任务：

- 创建虚拟机：
    - 创建 Linux 虚拟机
    - 创建 Windows 虚拟机
- 更新虚拟机：
    - 扩展驱动器
    - 标记虚拟机
    - 附加数据磁盘
    - 分离数据磁盘
- 操作虚拟机：
    - 启动虚拟机
    - 停止虚拟机
    - 重新启动虚拟机
- 列出虚拟机
- 删除虚拟机

若要查看执行这些操作的代码，请检查 GitHub 存储库 [virtual-machines-python-manage](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88) 中的 Python 脚本 **Hybrid/unmanaged-disks/example.py** 中的 **run_example()** 函数。

每个操作都清晰地带有注释和一个 print 函数。
这些示例不一定按以上列表中显示的顺序执行。


## <a name="run-the-python-sample"></a>运行 Python 示例

1.  如果还没有 Python，请[安装 Python](https://www.python.org/downloads/)。

    此示例（和 SDK）与 Python 2.7、3.4、3.5 和 3.6 兼容。

2.  对于 Python 开发，通常建议使用虚拟环境。 
    有关详细信息，请参阅 https://docs.python.org/3/tutorial/venv.html
    
    在 Python 3 上，请使用“venv”模块安装并初始化虚拟环境（对于 Python 2.7，必须安装 [virtualenv](https://pypi.python.org/pypi/virtualenv)）：

    ````bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ````

3.  克隆存储库。

    ````bash
    git clone https://github.com/Azure-Samples/virtual-machines-python-manage.git
    ````

4.  使用 pip 安装依赖项。

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  创建要用于 Azure Stack 的[服务主体](/azure-stack/azure-stack-create-service-principals)。 确保该服务主体在你的订阅上具有[参与者/所有者角色](/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal)。

6.  设置以下变量并将这些环境变量导出到当前 shell 中。 

`Note: provide an explanation of where these variables come from?`

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  注意，若要运行此示例，Ubuntu 16.04-LTS 和 WindowsServer 2012-R2-Datacenter 映像必须存在于 Azure Stack Marketplace 中。 可以[从 Azure 下载](/azure-stack/azure-stack-download-azure-marketplace-item)这些映像或者将其[添加到平台映像存储库](/azure-stack/azure-stack-add-vm-image)。


8. 运行示例。

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>说明

你可能会尝试使用 `virtual_machine.storage_profile.os_disk` 检索 VM 的 OS 磁盘。
在某些情况下，这可能能够实现你的目的，但请注意，它提供的是 `OSDisk` 对象。
若要像 `example.py` 那样更新 OS 磁盘的大小，需要的是 `OSDisk` 对象而不是 `Disk` 对象。
`example.py` 使用以下信息获取 `Disk` 对象：

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>后续步骤

- [Azure Python 开发中心](/develop/python/)
- [Azure 虚拟机文档](https://azure.microsoft.com/services/virtual-machines/)
- [虚拟机的学习路径](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- 如果没有 Azure 订阅，可从[此处](https://www.azure.cn/pricing/1rmb-trial)获取试用帐户。

