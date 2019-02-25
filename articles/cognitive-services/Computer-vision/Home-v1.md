---
title: 什么是计算机视觉 API？
titlesuffix: Azure Cognitive Services
description: 使用计算机视觉 API，开发人员可以访问用于处理图像并返回信息的高级算法。
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
origin.date: 08/10/2017
ms.date: 02/20/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: d5863d4a6959719a961c1259e825969a82032a78
ms.sourcegitcommit: 3ae99942621d28a8439ca1e7a7905caa5a3a10f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2019
ms.locfileid: "56582776"
---
# <a name="what-is-computer-vision-api-version-10"></a>什么是计算机视觉 API 版本 1.0？

> [!IMPORTANT]
> 现已推出计算机视觉 API 的新版本，请参阅：
>- [概述](/cognitive-services/computer-vision/home)

开发人员可以使用基于云的计算机视觉 API 来访问高级算法，以便处理图像并返回信息。 上传图像或指定图像 URL 后，Microsoft 计算机视觉算法可以根据输入和用户选择以不同的方式分析视觉内容。 使用计算机视觉 API，用户可以根据以下目的来分析图像：
- 基于内容标记图像。
- 对图像进行分类。
- 标识图像的类型和质量。
- [检测人脸并返回其坐标。](#Faces)
- 识别特定于域的内容。
- 生成内容说明。
- 使用光学字符识别标识图像中找到的打印文本。
- 识别手写的文本。
- 区分配色方案。
- 标志成人内容。
- 裁剪要用作缩略图的照片。

## <a name="requirements"></a>要求
- 支持的输入方法：原始图像二进制，采用应用程序/业务流程流或图像 URL 的形式。
- 支持的图像格式：JPEG、PNG、GIF 和 BMP。
- 图像文件大小：小于 4 MB。
- 图像维度：大于 50 x 50 像素。

## <a name="tagging-images"></a>标记图像
计算机视觉 API 在上千个可识别对象、生物、风景和操作的基础上返回标记。 当标记内容不明确或者不属常识时，API 响应会提供“提示”来澄清标记在已知场景中的含义。 标记不按分类来组织，且不存在继承层次结构。 内容标记集合在一起，形成图像“说明”的基础。该“说明”以人类可读语言显示，采用完整句子的格式。 请注意，图像说明目前只能使用英语。

上传图像或指定图像 URL 以后，计算机视觉 API 的算法会根据图像中标识的对象、生物和动作输出标记。 标记不限于主体（例如前景中的人），还包括场景（户内或户外）、家具、工具、植物、动物、配件、小器具等。

### <a name="example"></a>示例
![House_Yard](./Images/house_yard.png) '

```json
Returned Json
{
   'tags':[
      {
         "name":"grass",
         "confidence":0.999999761581421
      },
      {
         "name":"outdoor",
         "confidence":0.999970674514771
      },
      {
         "name":"sky",
         "confidence":999289751052856
      },
      {
         "name":"building",
         "confidence":0.996463239192963
      },
      {
         "name":"house",
         "confidence":0.992798030376434
      },
      {
         "name":"lawn",
         "confidence":0.822680294513702
      },
      {
         "name":"green",
         "confidence":0.641222536563873
      },
      {
         "name":"residential",
         "confidence":0.314032256603241
      },
   ],
}
```
## <a name="categorizing-images"></a>对图像进行分类
除了标记和说明以外，计算机视觉 API 还会返回早期版本中定义的基于分类的类别。 这些类别按分类组织，存在可继承的父/子层次结构。 所有类别都是英文， 既可单独使用，也可与新模型配合使用。

### <a name="the-86-category-concept"></a>“86 类”概念
根据一个包含 86 个概念的列表（见下图），可以将图像中找到的视觉特征分类，从广泛到具体，不一而足。 有关文本格式的完整分类，请参阅[类别分类](/cognitive-services/computer-vision/category-taxonomy)。

![分析类别](./Images/analyze_categories.png)

映像                                                  | 响应
------------------------------------------------------ | ----------------
![屋顶的女人](./Images/woman_roof.png)                 | people
![家庭照片](./Images/family_photo.png)             | people_crowd
![可爱的小狗](./Images/cute_dog.png)                     | animal_dog
![户外山脉](./Images/mountain_vista.png)       | outdoor_mountain
![视觉分析食品面包](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>标识图像类型
有多种方法可对图像进行分类。 计算机视觉 API 可以设置一个布尔标记，指示图像是黑白的还是彩色的。 它还可以设置一个标志，指示图像是否为线绘。 此外，它还可以指示图像是否为剪贴画，以及图像的质量如何（使用 0-3 作为度量）。

### <a name="clip-art-type"></a>剪贴画类型
检测图像是否为剪贴画。  

值 | 含义
----- | --------------
0     | Non-clip-art
1     | ambiguous
2     | normal-clip-art
3     | good-clip-art

映像|响应
----|----
![视觉分析奶酪剪贴画](./Images/cheese_clipart.png)|3 good-clip-art
![视觉分析住宅庭院](./Images/house_yard.png)|0 Non-clip-art

### <a name="line-drawing-type"></a>线绘类型
检测图像是否为线绘。

映像|响应
----|----
![视觉分析狮子绘制](./Images/lion_drawing.png)|True
![视觉分析花](./Images/flower.png)|False

### <a name="faces"></a>人脸
检测图片内的人脸并生成人脸坐标、人脸边框、性别和年龄。 这些视觉特征是为人脸生成的元数据的一部分。 若要为人脸生成更广泛的元数据（人脸识别、姿势检测等），请使用人脸 API。  

映像|响应
----|----
![视觉分析屋顶的女人人脸](./Images/woman_roof_face.png) | [ { "age":23, "gender":"Female", "faceRectangle": { "left":1379, "top":320, "width":310, "height":310 } } ]
![视觉分析妈妈女儿人脸](./Images/mom_daughter_face.png) | [ { "age":28, "gender":"Female", "faceRectangle": { "left":447, "top":195, "width":162, "height":162 } }, { "age":10, "gender":"Male", "faceRectangle": { "left":355, "top":87, "width":143, "height":143 } } ]
![视觉分析家庭照片人脸](./Images/family_photo_face.png) | [ { "age":11, "gender":"Male", "faceRectangle": { "left":113, "top":314, "width":222, "height":222 } }, { "age":11, "gender":"Female", "faceRectangle": { "left":1200, "top":632, "width":215, "height":215 } }, { "age":41, "gender":"Male", "faceRectangle": { "left":514, "top":223, "width":205, "height":205 } }, { "age":37, "gender":"Female", "faceRectangle": { "left":1008, "top":277, "width":201, "height":201 } } ]


## <a name="domain-specific-content"></a>特定于域的内容

除了标记和顶级分类以外，计算机视觉 API 还支持专用（或特定于域的）信息。 可将专用信息作为独立方法来实现，也可将其与高级分类配合使用。 可以利用该信息，通过添加特定领域的模块来进一步优化“86 类”分类。

目前，只能使用特定领域信息进行名人识别和地标识别。 这些特定领域的信息针对人和人群类别以及世界各地的地标进行了优化。

可以通过两个选项来使用特定领域的模型：

### <a name="option-one---scoped-analysis"></a>选项一 - 范围内分析
仅分析所选模型，方法是触发 HTTP POST 调用。 对于此选项，如果知道要使用的具体模型，则可指定模型的名称，仅获取与该模型相关的信息。 例如，如果只进行名人识别，则可使用此选项。 响应包含一系列可能与之匹配的名人，以及置信度。

### <a name="option-two---enhanced-analysis"></a>选项二 - 强化分析
通过分析，提供与“86 类”分类中的类别相关的更多详细信息。 如果应用程序用户除了获取一个或多个特定领域模型中的详细信息，还需要获取泛型图像分析信息，则可使用此选项。 调用此方法时，先调用“86 类”分类的分类器。 如果某个类别与已知/匹配模型的类别相符，则会进行第二轮分类器调用。 例如，如果 'details=all'，或者 "details" 包括 'celebrities'，此方法会在“86 类”分类器被调用以后调用名人分类器。 结果包含以“people_”开头的标记。

## <a name="generating-descriptions"></a>生成说明 
计算机视觉 API 算法可分析图像中的内容。 此分析形成“说明”的基础。该“说明”以人类可读语言显示，采用完整句子。 说明汇总了图像中找到的内容。 计算机视觉 API 的算法根据图像中标识的对象生成各种说明。 会对说明一一进行评估，并生成置信度。 然后返回一个列表，将置信度从高到低进行排列。   

### <a name="example-description-generation"></a>说明生成示例
![黑白建筑](./Images/bw_buildings.png) '
```json
 Returned Json

'description':{
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a large city',
         'confidence':0.607638706850331
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a photo of a large city',
         'confidence':0.577256764264197
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a city',
         'confidence':0.538493271791207
      }
   ]   
   'description':[
      "tags":{
         "outdoor",
         "city",
         "building",
         "photo",
         "large",
      }
   ]
}
```

## <a name="perceiving-color-schemes"></a>认知配色方案
计算机视觉算法从图像中提取颜色。 颜色在三种不同的环境（前景、背景和整体）中分析。 颜色分成十二 (12) 个主要的主题色。 这些主题色为黑、蓝、褐、灰、绿、橙、粉、紫、红、青、白、黄。 可能会在十六进制颜色代码中返回简单的黑白色或主题色，具体取决于图像中的颜色。

映像                                                       | 前景 |背景| 颜色
----------------------------------------------------------- | --------- | ------- | ------
![户外山地](./Images/mountain_vista.png)            | 黑色     | 黑色   | 白色
![视觉分析花](./Images/flower.png)               | 黑色     | 白色   | 白色、黑色、绿色
![视觉分析火车站](./Images/train_station.png) | 黑色     | 黑色   | 黑色

### <a name="accent-color"></a>主题色
从图像中提取的颜色。该图像旨在通过混合使用主流色和饱和度，向用户呈现最抢眼的颜色。

映像                                                       | 响应
----------------------------------------------------------- | ----
![户外山地](./Images/mountain_vista.png)            | #BC6F0F
![视觉分析花](./Images/flower.png)               | #CAA501
![视觉分析火车站](./Images/train_station.png) | #484B83


### <a name="black--white"></a>黑白
布尔标记，指示图像是否为黑白色。

映像                                                      | 响应
---------------------------------------------------------- | ----
![视觉分析建筑](./Images/bw_buildings.png)      | True
![视觉分析庭院](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>标志成人内容
各种视觉类别中包含成人和不雅内容，可以检测成人材料检测并限制显示包含性内容的图像。 可以在滑尺上设置成人和不雅内容检测的筛选器以满足用户偏好。

## <a name="optical-character-recognition-ocr"></a>光学字符识别 (OCR)
OCR 技术可检测图像中的文本内容，并将所标识的文本提取到计算机可读的字符流中。 可以将结果用于搜索和各种其他目的，例如医疗记录、安全性、银行操作。 它会自动检测语言。 OCR 可以节省用户的时间，为用户带来方便，因为用户只需对文本拍照即可，不需进行文本转录。

OCR 支持 25 种语言。 这些语言包括：阿拉伯语、简体中文、繁体中文、捷克语、丹麦语、荷兰语、英语、芬兰语、法语、德语、希腊语、匈牙利语、意大利语、日语、韩语、挪威语、波兰语、葡萄牙语、罗马尼亚语、俄语、塞尔维亚语(西里尔文和拉丁语)、斯洛伐克语、西班牙语、瑞典语和土耳其语。

OCR 可以根据需要，围绕图像的水平轴将识别的文本旋转相应的度数，对倾斜的文本进行纠正。 OCR 提供每个单词的帧坐标，如下图所示。

![OCR 概述](./Images/vision-overview-ocr.png) OCR 要求：
- 输入图像的大小必须介于 40 x 40 像素到 3200 x 3200 像素之间。
- 图像不能大于 10 兆像素。

对输入图像进行旋转时，旋转角度可以是 90 度的任意倍数加一个小角度（最多 40 度）。

文本识别的准确性取决于图像质量。 以下情况可能导致读取不准确：
- 图像模糊。
- 文本为手书或草书。
- 字体为艺术体。
- 文本大小过小。
- 背景复杂、阴影、文本反光或透视扭曲。
- 文本过长或单词开头缺少大写字母
- 文本包含下标、上标或删除线。

的限制：在以文本为主的照片上，误报可能来自部分识别的字词。 某些照片（尤其是没有任何文本的照片），精度可能因图像类型的不同而差异很大。

## <a name="recognize-handwritten-text"></a>识别手写的文本
此技术使你可以检测和提取笔记、信件、文章、白板、表格等对象中的手写文本。它适用于不同的表面和背景，例如白纸、黄色便笺、白板。

手写文本识别技术既省时又省力，可以提高工作效率，因为你只需获取文本图像，不需进行转录。 它可以将说明数字化。 可以通过此数字化实现便捷的搜索。 它还可以减少纸质工作。

输入要求：
- 支持的图像格式：JPEG、PNG 和 BMP。
- 图像文件大小必须小于 4 MB。
- 图像维度必须至少为 40 x 40，至多为 3200 x 3200。

注意：此技术当前处于预览状态，且仅适用于英语文本。

## <a name="generating-thumbnails"></a>生成缩略图
缩略图是完全尺寸的图像的小型表示形式。 设备不同（例如手机、平板电脑和电脑），需要的用户体验 (UX) 布局和缩略图大小也不同。 此计算机视觉 API 功能使用智能裁剪来解决问题。

上传某个图像以后，会生成高质量的缩略图，计算机视觉 API 算法会分析该图像中的对象， 然后会裁剪图像以满足感兴趣区域的要求。 输出显示在一个特殊的框架中，如下图所示。 生成的缩略图在呈现时，使用的纵横比可能不同于原始图像的纵横比，具体取决于用户的需要。

缩略图算法的原理如下所示：

1. 从图像中删除让人分散注意力的元素并识别主要对象，即感兴趣区域。
2. 基于所标识的感兴趣区域裁剪图像。
3. 更改纵横比，使之适合目标缩略图维度。

![缩略图](./Images/thumbnail-demo.png)

<!-- Update_Description: link update -->