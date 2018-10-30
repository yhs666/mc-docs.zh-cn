---
author: rockboyfor
ms.service: ansible
ms.topic: include
origin.date: 08/09/2018
ms.date: 10/29/2018
ms.author: v-yeche
ms.openlocfilehash: 0fa576ccf289a8b61d227d9ed80d69f9a0d12771
ms.sourcegitcommit: c5529b45bd838791379d8f7fe90088828a1a67a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035087"
---
<!--Verify successfully-->
1. 在 Azure 本地 Shell 中，创建名为 `rg.yml` 的文件。

    ```bash
    vi rg.yml
    ```

1. 按 **I** 键进入插入模式。

1. 在编辑器中粘贴以下代码：

   ```yaml
   ---
   - hosts: localhost
     connection: local
     tasks:
       - name: Create resource group
         azure_rm_resourcegroup:
           name: ansible-rg
           location: chinaeast
         register: rg
       - debug:
           var: rg
   ```

1. 按 **Esc** 键退出插入模式。

1. 保存文件，然后输入以下命令退出 vi 编辑器：

    ```bash
    :wq
    ```

1. 运行 playbook `rg.yml`：

   ```bash
   ansible-playbook rg.yml
   ```

运行 ansible 命令的结果应如以下输出所示：

```output
PLAY [localhost] *********************************************************************************

TASK [Gathering Facts] ***************************************************************************
ok: [localhost]

TASK [Create resource group] *********************************************************************
changed: [localhost]

TASK [debug] *************************************************************************************
ok: [localhost] => {
    "rg": {
        "changed": true,
        "contains_resources": false,
        "failed": false,
        "state": {
            "id": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/ansible-rg",
            "location": "chinaeast",
            "name": "ansible-rg",
            "provisioning_state": "Succeeded",
            "tags": null
        }
    }
}

PLAY RECAP ***************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0
```

<!-- Update_Description: new articles on ansible create resource group -->
<!--ms.date: 10/22/2018-->
