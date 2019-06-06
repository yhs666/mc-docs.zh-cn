---
title: 如何在 Azure Stack 中使用 SSH 公钥 | Microsoft Docs
description: 如何使用 SSH 公钥
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/25/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 4248d969923cec0b5d510c438e7384543f40b331
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249131"
---
# <a name="how-to-use-an-ssh-public-key"></a>如何使用 SSH 公钥

可能需要创建 SSH 公钥和私钥对，才能在托管 Web 应用的 Azure Stack 中从开发计算机和服务器 VM 建立 SSH 连接。 本文逐步讲解如何获取密钥并使用它们来连接服务器。 可以使用 SSH 客户端在 Linux 服务器上获取 bash 提示符，或使用 SFTP 客户端将文件移入和移出服务器。

## <a name="create-an-ssh-public-key-on-windows"></a>在 Windows 上创建 SSH 公钥

在本部分，你将使用 PuTTY 的密钥生成器创建 SSH 公钥和私钥对，以便在 Azure Stack 中与 Linux 计算机建立安全连接时使用。 PuTTY 是适用于 Windows 和 Unix 平台的 SSH 与 Telnet 的免费实现，它附带了一个 `xterm` 终端仿真器。

1. [下载并安装适用于你的计算机的 PuTTY。](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

1. 打开 PuTTY 密钥生成器。

1. 将“参数”设置为“RSA”。  

1. 将生成的密钥中的位数设置为 `2048`。  

    ![使用 PuTTY 生成 SSH 公钥](media/azure-stack-dev-start-howto-ssh-public-key/001-putty-key-gen-start.png)

1. 然后选择“生成”  。 在“密钥”区域中，将光标移到空白区域上以生成一些随机字符。 

1. 添加**密钥通行短语**，并在“确认”框中确认。  请记下该通行短语。
    ![使用 PuTTY 生成 SSH 公钥](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-key-gen-result.png)

1. 选择“保存公钥”，并将公钥保存到可访问的位置。 

1. 选择“保存私钥”，将私钥保存到可访问的位置，并记得它属于公钥。 

公钥已存储在保存的文本文件中。 打开该文件后，其中会显示如下所示的文本：

```text  
---- BEGIN SSH2 PUBLIC KEY ----
Comment: "rsa-key-20190330"
THISISANEXAMPLEDONOTUSE AAAAB3NzaC1yc2EAAAABJQAAAQEAthW2CinpqhXq
9uSa8/lSH7tLelMXnFljSrJIcpxp3MlHlYVbjHHoKfpvQek8DwKdOUcFIEzuStfT
Z8eUI1s5ZXkACudML68qQT8R0cmcFBGNY20K9ZMz/kZkCEbN80DJ+UnWgjdXKLvD
Dwl9aQwNc7W/WCuZtWPazee95PzAShPefGZ87Jp0OCxKaGYZ7UXMrCethwfVumvU
aj+aPsSThXncgVQUhSf/1IoRtnGOiZoktVvt0TIlhxDrHKHU/aZueaFXYqpxDLIs
BvpmONCSR3YnyUtgWV27N6zC7U1OBdmv7TN6M7g01uOYQKI/GQ==
---- END SSH2 PUBLIC KEY ----
```

使用公钥时，需复制并粘贴文本框中的整个内容作为应用程序请求的密钥值。

<!-- 
## Create an SSH public key on Linux

ToDo: I need to write this section.

-->
## <a name="connect-with-ssh-using-putty"></a>使用 PuTTY 通过 SSH 进行连接

如果已安装 PuTTY，则你已获得密钥生成器和一个 SSH 客户端。 打开 SSH 客户端 PuTTY 并配置连接值和 SSH 密钥。如果你在 Azure Stack 所在的同一网络中操作，请连接到 VM。

连接之前，需要：
- PuTTY
- Azure Stack 中使用 SSH 公钥作为身份验证类型的 Linux 计算机的 IP 地址和用户名。
- 需要为计算机打开端口 22。
- 创建该计算机时使用的 SSH 公钥。
- 与 Azure Stack 位于同一网络中的、运行 PuTTY 的客户端计算机。

### <a name="connect-via-ssh-with-putty"></a>使用 PuTTy 通过 SSH 进行连接

1. 打开 PuTTY。

    ![使用 PuTTY 进行连接](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-connect.png)

2. 添加计算机的用户名和公共 IP 地址。 例如，为“主机名”输入 `username@192.XXX.XXX.XX`。  
3. 验证是否已设置**端口** `22`，并且“连接类型”设置为 `SSH`。 
4. 在“类别”树中展开“SSH” > “身份验证”。   

    ![SSH 私钥](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-set-private-key.png)

5. 选择“浏览”，并找到公钥和私钥对的私钥文件 (filename.ppk)。 
6. 在“类别”树中选择“会话”。

    ![包含私钥的 SSH 公钥](media/azure-stack-dev-start-howto-ssh-public-key/003-puTTY-save-session.png)

7. 在“保存的会话”下面键入会话的名称，然后选择“保存”。  
8. 选择会话的名称，然后选择“加载”。 
9. 选择“打开”  。 此时会打开 SSH 会话。

## <a name="connect-with-sftp-with-filezilla"></a>使用 FileZilla 通过 SFTP 进行连接

可以使用 Filezilla 作为支持 SFTP 的 FTP 客户端，将文件移入和移出 Linux 计算机。 FileZilla 可在 Windows 10、Linux 和 macOS 上运行。 FileZilla 客户端支持 FTP，但也支持基于 TLS 的 FTP (FTPS) 和 SFTP。 它是根据 GNU 常规公共许可条款免费分发的开源软件。

### <a name="set-your-connection"></a>设置连接

1. [下载并安装 FileZilla](https://filezilla-project.org/download.php)。
1. 打开 FileZilla。
1. 选择“文件” > “站点管理器”。  

    ![包含私钥的 SSH 公钥](media/azure-stack-dev-start-howto-ssh-public-key/005-filezilla-file-manager.png)

1. 为“协议”选择“SFTP - SSH 文件传输协议”。  
1. 在“主机”框中，添加计算机的公共 IP 地址。 
1. 为“登录类型”选择“正常”。  
1. 添加你的用户名和密码。
1. 选择“确定”  。
1. 选择“编辑” > “设置”。  

    ![包含私钥的 SSH 公钥](media/azure-stack-dev-start-howto-ssh-public-key/006-filezilla-add-private-key.png)

1. 在“选择页面”树中展开“连接”。   选择“SFTP”。 
1. 选择“添加密钥文件”并添加私钥文件，例如 filename.ppk。 
1. 选择“确定”  。

### <a name="open-your-connection"></a>打开连接

1. 打开 FileZilla。
1. 选择“文件” > “站点管理器”。  
1. 选择站点名称，然后选择“连接”。 

## <a name="next-steps"></a>后续步骤

详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)