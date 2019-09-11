---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 08/05/2019
ms.date: 09/05/2019
ms.author: v-junlch
ms.openlocfilehash: 5409434929c1a91a4ede733961f156cb27abe799
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805758"
---
## <a name="create-the-local-function-app-project"></a>创建本地函数应用项目

从命令行运行以下命令，在当前本地目录的 `MyFunctionProj` 文件夹中创建函数应用项目。 也会在 `MyFunctionProj` 中创建一个 GitHub 存储库。

```command
func init MyFunctionProj
```

当系统提示时，请从下面的语言选项中选择一个辅助角色运行时：

+ `dotnet`：创建 .NET 类库项目 (.csproj)。
+ `node`：创建一个基于 Node.js 的项目。 选择 `javascript` 或 `typescript`。 

创建项目后，使用以下命令导航到新的 `MyFunctionProj` 项目文件夹。

```command
cd MyFunctionProj
```

