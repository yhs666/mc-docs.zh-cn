---
title: 使用 Azure CLI 重启 Azure Database for PostgreSQL - 单一服务器
description: 本文介绍了如何使用 Azure CLI 重启 Azure Database for PostgreSQL - 单一服务器
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 5/6/2019
ms.date: 05/20/2019
ms.openlocfilehash: 081699a93d7a05af2542b3cb2f906dd8752b886e
ms.sourcegitcommit: 11d81f0e4350a72d296e5664c2e5dc7e5f350926
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65732003"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>使用 Azure CLI 重启 Azure Database for PostgreSQL - 单一服务器
本主题介绍如何重启 Azure Database for PostgreSQL 服务器。 出于维护原因，可能需要重启服务器，这会在服务器执行操作时导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可能在处理缩放 vCore 等先前请求的操作。
 
完成重启所需的时间取决于 PostgreSQL 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。

## <a name="prerequisites"></a>先决条件
若要完成本操作指南，需要：
- [Azure Database for PostgreSQL 服务器](quickstart-create-server-up-azure-cli.md)

> [!IMPORTANT]
> 本操作方法指南要求使用 Azure CLI 版本 2.0 或更高版本。 若要确认版本，请在 Azure CLI 命令提示符下输入 `az --version`。 若要安装或升级，请参阅[安装 Azure CLI]( /cli/install-azure-cli)。


## <a name="restart-the-server"></a>重启服务器

使用以下命令重启服务器：

```azurecli
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>后续步骤

了解[如何在 Azure Database for PostgreSQL 中设置参数](howto-configure-server-parameters-using-cli.md)