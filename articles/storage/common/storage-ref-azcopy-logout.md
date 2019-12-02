---
title: azcopy logout | Microsoft Docs
description: 本文提供有关 azcopy logout 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 10/16/2019
ms.date: 11/25/2019
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: e7b516735c8b257323d7ba98c913e460f2790dde
ms.sourcegitcommit: 99c408fd0f1fc264acaed41f1a77fea4ebbc0e0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354355"
---
# <a name="azcopy-logout"></a>azcopy logout

注销用户并终止对 Azure 存储资源的访问。

## <a name="synopsis"></a>概要

此命令将删除当前用户的所有缓存登录信息。

```azcopy
azcopy logout [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](storage-use-azcopy-blobs.md)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="options"></a>选项

|选项|说明|
|--|--|
|-h、--help|显示 logout 命令的帮助内容。|

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

|选项|说明|
|---|---|
|--cap-mbps uint32|以兆位/秒为单位限制传输速率。 瞬间的吞吐量可能会与上限有所不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。|
|--output-type 字符串|命令输出的格式。 选项包括：text、json。 默认值为“text”。|

## <a name="see-also"></a>另请参阅

- [azcopy](storage-ref-azcopy.md)
