---
title: 实时传送视频流故障排除指南 | Azure
description: 本主题提供有关如何排查实时流式处理问题的建议。
services: media-services
documentationcenter: ''
author: hayley244
manager: digimobile
editor: ''
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/20/2017
ms.date: 09/04/2017
ms.author: v-haiqya
ms.openlocfilehash: cda8144c589e90c81e705a08748865bd9f39608d
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475388"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>实时流式处理故障排除指南
本主题提供有关如何排查某些实时流式处理问题的建议。

## <a name="issues-related-to-on-premises-encoders"></a>与本地编码器相关的问题
本部分提供有关如何排查本地编码器相关问题的建议，这些编码器配置为向启用了实时编码的 AMS 通道发送单比特率流。

### <a name="problem-would-like-to-see-logs"></a>问题：想要查看日志
* **潜在问题**：找不到可帮助调试问题的编码器日志。
  
  * Telestream Wirecast：通常可以在 C:\Users\{username}\AppData\Roaming\Wirecast\下找到日志 
  * Elemental Live：可以在管理门户上找到日志登录的链接。 单击“开始” 然后单击“日志” 在“日志文件”页上，可以看到所有 LiveEvent 项的日志列表；选择与当前会话匹配的日志。 
  * **Flash Media Live Encoder**：可以通过导航到“编码日志”选项卡找到“日志目录...”。

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>问题：没有输出渐进式流的选项
* **潜在问题**：使用的编码器不自动取消隔行扫描。 
  
    **故障排除步骤**：在编码器界面中查找取消隔行扫描选项。 启用取消隔行扫描后，再次检查渐进式输出设置。 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>问题：已尝试多种编码器输出设置，但仍然无法连接。
* **潜在问题**：未正确重置 Azure 编码通道。 
  
    **故障排除步骤**：确保编码器不再推送到 AMS，停止并重置该通道。 再次运行后，尝试使用新的设置连接到编码器。 如果这样仍无法更正问题，请尝试创建全新的通道，因为有时通道在经过几次失败的尝试后可能会损坏。  
* **潜在问题**：GOP 大小或关键帧设置不佳。 
  
    **故障排除步骤**：建议的 GOP 大小或关键帧间隔为 2 秒。 有些编码器以帧数计算此设置，而有些则以秒计算。 例如：输出 30fps 时，GOP 大小是 60 帧，相当于 2 秒。  
* **潜在问题**：关闭的端口阻止流式处理。 
  
    **故障排除步骤**：通过 RTMP 流式处理时，请检查防火墙和/或代理设置，确认出站端口 1935 和 1936 已打开。 使用 RTP 流式处理时，请确认出站端口 2010 已打开。 

### <a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>问题：在配置编码器以使用 RTP 协议流式处理时，没有可用于输入主机名的位置。
* **潜在问题**：许多 RTP 编码器不允许使用主机名，需要获取 IP 地址。  
  
    **故障排除步骤**：若要查找 IP 地址，请在任一计算机上打开命令提示符。 若要在 Windows 中执行此操作，请打开“运行”启动器 (WIN + R) 并键入“cmd”。  

    命令提示符打开后，键入“Ping [AMS 主机名]”。 

    通过在 Azure 引入 URL 中省略端口号来派生主机名，如以下示例中突出显示的部分所示： 

    rtp://test2-amstest009.rtp.channel.mediaservices.chinacloudapi.cn:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> 如果按照故障排除步骤执行操作后，仍然无法成功进行流式处理，可使用 Azure 门户提交支持票证。
> 
> 
