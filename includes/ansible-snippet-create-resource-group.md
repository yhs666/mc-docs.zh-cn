---
author: rockboyfor
ms.service: ansible
ms.topic: include
origin.date: 04/22/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: f52418e696db8d66a836f6b5bffff7a6f361e332
ms.sourcegitcommit: 878a2d65e042b466c083d3ede1ab0988916eaa3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835756"
---
<!--Verify successfully-->
1. 在 Azure 本地 Shell 中，创建名为 `rg.yml` 的文件。

    ```bash
    code rg.yml
    ```

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

1. 保存文件并退出编辑器。

1. 使用 `ansible-playbook` 命令运行 playbook：

   ```bash
   ansible-playbook rg.yml
   ```

运行 playbook 后，可看到类似于以下结果的输出：

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

<!-- Update_Description: wording update -->

