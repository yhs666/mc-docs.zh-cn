---
title: 存储帐户名称错误
description: 说明指定存储帐户名称时可能遇到的错误。
ms.topic: troubleshooting
origin.date: 03/09/2018
ms.date: 11/25/2019
ms.openlocfilehash: 64c4f8a46d29dfe94fbd58582df6488f45b950b6
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389568"
---
# <a name="resolve-errors-for-storage-account-names"></a>解决存储帐户名称错误

本文介绍了部署存储帐户时可能遇到的命名错误。

## <a name="symptom"></a>症状

如果存储帐户名称包含禁用的字符，则会收到如下错误：

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

对于存储帐户，必须提供在 Azure 中唯一的资源名称。 如果不提供唯一名称，会出现类似于下面的错误：

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

如果部署的存储帐户与订阅中某个现有存储帐户的名称相同，但提供的位置不同，会出现一条错误消息，指出不同的位置已存在该存储帐户。 请删除现有存储帐户，或者提供与现有存储帐户相同的位置。

## <a name="cause"></a>原因

存储帐户名称必须为 3 到 24 个字符，只能使用数字和小写字母。 此名称必须唯一。

## <a name="solution"></a>解决方案

请确保存储帐户名称是唯一的。 可将命名约定与 [uniqueString](resource-group-template-functions-string.md#uniquestring) 函数的结果连接起来创建一个唯一名称。

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

请确保存储帐户名称不超过 24 个字符。 [uniqueString](resource-group-template-functions-string.md#uniquestring) 函数返回 13 个字符。 如果将前缀或后缀连接到 **uniqueString** 结果，请提供 11 个字符（或更少字符）的值。

```json
"parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
}
```

请确保存储帐户名称不包含任何大写字母或特殊字符。

<!--Update_Description: update meta properties, wording update -->