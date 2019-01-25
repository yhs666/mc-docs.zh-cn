---
title: 常见问题解答 - 内容审查器
titlesuffix: Azure Cognitive Services
description: 获取内容审查器常见问题解答。
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: faq
origin.date: 11/21/2016
ms.date: 01/22/2019
ms.author: v-junlch
ms.openlocfilehash: cc1253d67dc933a276f3df28dc7f4e16af832078
ms.sourcegitcommit: f248afb1039011d34579baed2980f0632061f5b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54858051"
---
# <a name="frequently-asked-questions-faq"></a>常见问题 (FAQ)

#### <a name="what-does-my-content-moderator-subscription-include"></a>我的内容审查器订阅包括哪些内容？
内容审查器订阅包括对 API 的访问权限。 可以决定是要使用其中一种，还是两种都要使用，具体视业务需求而定。

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>可使用 API 审查的内容有什么限制？
使用 API 时，图像至少必须为 128 像素，且文件大小不得超过 4MB。 文本长度不得超过 1024 个字符。 视频时长没有限制。

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>如果传递到文本 API 或图像 API 的内容超过大小限制，会发生什么？
文本 API 会返回错误代码，指明文本超过长度上限。 图像 API 也会返回错误代码，指明图像不符合大小要求。

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Microsoft 是否保留已提交审查的图像、文本或视频？
内容是你自己的。未经你同意，Microsoft 不得保留任何内容。 Microsoft 使用业界领先的安全措施来帮助保护你的内容。

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>我能否使用内容审查器筛查非法儿童剥削图像？
否。 不过，合格组织可以使用 [PhotoDNA 云服务](https://www.microsoft.com/photodna "Microsoft PhotoDNA 云服务")筛查此类内容。

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>用户最多可以加入多少个审阅团队？ 用户能否切换团队？
用户一次可以加入一个团队。

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>我的审阅团队可以有多少名团队成员？
团队中最多可以有 5 名成员（包括管理员在内）。

