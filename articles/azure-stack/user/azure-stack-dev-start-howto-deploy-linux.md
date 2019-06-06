---
title: 将 Linux VM 部署到 Azure Stack | Microsoft Docs
description: 将应用部署到 Azure Stack。
services: azure-stack
author: WenJason
ms.service: azure-stack
ms.topic: overview
origin.date: 04/24/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 5e4b2e59721b5fafd9988fd1ea11d52caf966f1b
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249151"
---
# <a name="deploy-a-linux-vm-to-host-a-web-app-in-azure-stack"></a>在 Azure Stack 中部署用于托管 Web 应用的 Linux VM

可以使用市场中的 Ubunutu 映像创建和部署基本的 Linux 虚拟机 (VM)，以托管使用 Web 框架创建的 Web 应用。 此 VM 可以托管使用以下语言编写的 Web 应用：

- **Python**。 常见的 Python Web 框架包括 Flask、Bottle 和 Django。
- **Go**。 常见的 Go 框架包括 Revel、Martini、Gocraft/Web 和 Gorilla。 
- **Ruby**。 可将 Ruby on Rails 设置为交付 Ruby Web 应用的框架。 
- **Java**。 可以使用 Java 来开发要发布到 Apache Tomcat 服务器的 Web 应用。 可将 Tomcat 安装在 Linux 上，然后将 Java WAR 文件直接部署到服务器。 

可根据本文中的说明，通过任何 Web 应用、框架和使用 Linux OS 的后端技术来启动并运行。 然后可以使用 Azure Stack 来管理基础结构，并使用技术内部的管理工具来处理应用的维护任务。

## <a name="deploy-a-linux-vm-for-a-web-app"></a>部署适用于 Web 应用的 Linux VM

创建机密密钥，使用 Linux VM 的基础映像，指定 VM 的特定属性，然后创建 VM。 创建 VM 后，打开使用 VM 所需的端口，并让 VM 托管你的应用。 此外，将会创建 DNS 名称。 最后，连接到 VM 并使用 apt-get 更新计算机。 完成本操作指南中的步骤后，即已在 Azure Stack 中创建了一个可以托管 Web 应用的 VM。

可以直接跳转到说明，否则需要确保一切已准备就绪。

## <a name="prerequisites-and-considerations"></a>先决条件和注意事项

- 需要一个 Azure Stack 订阅。
- 该订阅需有权访问 **Ubuntu Server 16.04 LTS** 映像。 可以使用更高版本的映像，但请注意，本文中的说明是根据 16.04 LTS 编写的。 如果没有此映像，请联系云操作员，以将映像放入 Azure Stack 市场。

<!-- Azure Stack specific considerations

### Authentication

Azure Stack works with two identity management services, Azure Active Directory (Azure AD) and Active Directory Federated Services (AD FS).  This section addresses how this procedure will work with either version.

### Connectivity

Azure Stack can be run in connected to completely disconnected scenarios. This section addresses considerations about the use case in relation to connectivity.

### Azure Stack Development Kit and Integrated Systems

While the two version of the product are the same product both version behave differently. Call out considerations about either version. 

### Azure Stack version

Place any version specific calls outs. The procedure will contain steps for the latest version. This section will contain call outs for previous version that are still supported. -->

## <a name="deploy-vm-using-the-portal"></a>使用门户部署 VM

使用 Putty 之类的应用创建服务器的 SSH 公钥。 访问 Azure Stack 门户并添加 Ubuntu 服务器。 设置网络和 DNS 条目。 连接到服务器以将其更新。

### <a name="create-your-vm"></a>创建 VM

1. 创建服务器的 SSH 公钥。 有关详细信息，请参阅[如何使用 SSH 公钥](azure-stack-dev-start-howto-ssh-public-key.md)。
2. 打开 Azure Stack 门户。
3. 选择“创建资源” > “计算” > “Ubuntu Server 16.04 LTS”。   

    ![将 Web 应用部署到 Azure Stack VM](media/azure-stack-dev-start-howto-deploy-linux/001-portal-compute.png)

4. 在“创建虚拟机”边栏选项卡中，对于“1.   配置基本设置”：
    1. 键入 **VM 的名称**。
    1. 选择 **VM 磁盘类型**：“高级 SSD”或“标准 HDD”。  
    1. 键入你的**用户名**。
    1. 选择“SSH 公钥”作为“身份验证类型”。  
    1. 检索创建的 SSH 公钥。 在文本编辑器中打开此公钥，复制密钥并将其贴到“SSH 公钥”框中。  包含从 `---- BEGIN SSH2 PUBLIC KEY ----` 到 `---- END SSH2 PUBLIC KEY ----` 的文本。 将整个文本块贴到密钥框中：
       ```text  
            ---- BEGIN SSH2 PUBLIC KEY ----
            Comment: "rsa-key-20190207"
            <Your key block>
            ---- END SSH2 PUBLIC KEY ----```
    1. Select your Azure Stack subscription.
    1. Create a new **Resource group** or use an existing depending on how you want to organize the resources for your app.
    1. Select your location. The ASDK is usually in a **local** region. The location will depend on your Azure Stack.
1. For **2. Size** type:
    - Select the size of data and RAM for your VM that is available in your Azure Stack.
    - You can either browse the list or filter for the size of your VM by **Compute type**, **CPUs**, and storage space.
    - Prices presented are estimates in your local currency that include only Azure infrastructure costs and any discounts for the subscription and location. The prices don't include any applicable software costs. Recommended sizes are determined by the publisher of the selected image based on hardware and software requirements.
    - Using a standard disk rather than a premium disk could impact operating system performance.

1. in **3. Configure optional** features type:
    1. For **High availability,** you can select an availability set. To provide redundancy to your application, you can group two or more virtual machines in an availability set. This configuration ensures that during a planned or unplanned maintenance event, at least one virtual machine will be available and meet the 99.95% Azure SLA. The availability set of a virtual machine can't be changed after it is created.
    1. For **Storage** select **Premium disks (SSD)** or **Standard disks (HDD)**. Premium disks (SSD) are backed by solid-state drives and offer consistent, low-latency performance. They provide the best balance between price and performance, and are ideal for I/O-intensive applications and production workloads. Standard disks (HDD) are backed by magnetic drives and are preferable for applications where data is accessed infrequently.
    1. You can select **Use managed disks**. Enable this feature to have Azure automatically manage the availability of disks to provide data redundancy and fault tolerance, without creating and managing storage accounts on your own. Managed disks may not be available in all regions. For more information, see [Introduction to Azure managed disks](/virtual-machines/windows/managed-disks-overview).
    1. Select **virtual network** to configure your network. Virtual networks are logically isolated from each other in Azure. You can configure their IP address ranges, subnets, route tables, gateways, and security settings, much like a traditional network in your data center. Virtual machines in the same virtual network can access each other by default. 
    1. Select **subnet** to configure your subnet. A subnet is a range of IP addresses in your virtual network, which can be used to isolate virtual machines from each other or from the Internet. 
    1. Select **Public IP address** to configure access to your VM or services running on your VM. Use a public IP address if you want to communicate with the virtual machine from outside the virtual network. 
    1. Select **Network Security Group**, **Basic, or **Advanced**. Set rules that allow or deny network traffic to the VM 
    1. Select **public inbound ports** to set access for common or custom protocols to your VM. The service specifies the destination protocol and port range for this rule. You can choose a predefined service, like RDP or SSH, or provide a custom port range.  
        For the  web server, you are going to want to HTTP (80), HTTPS (443), and SSH (22) open. If you plan on managing the machine with an RDP connection, open port 3389.
    1. Select **Extensions** if you would like to add Extension to your VM. Extensions add new features, like configuration management or antivirus protection, to your virtual machine using extensions. 
    1. Disable or enable **Monitoring**. Capture serial console output and screenshots of the virtual machine running on a host to help diagnose startup issues. 
    1. Select **diagnostics storage account** to specify the storage account holding your metrics. Metrics are written to a storage account so you can analyze them with your own tools. . 
    1. Select **OK**.
1. Review **4. Summary**:
    - The portal validates your settings.
    - You can download the Azure Resource Manager template for your VM if you would like to reuse your settings with an Azure Resource Manager workflow.
    - Press **OK** when the validation has passed. The deployment of the VM takes several minutes.

### Specify the open ports and DNS name

You will want to make your web app accessible to users on your network by opening the ports used to connect to the machine and adding a friendly DNS name such as `mywebapp.local.cloudapp.azurestack.external` that users can use in their web browsers.

#### Open inbound ports

You can modify the destination protocol and port range for predefined service, like RDP or SSH or provide a custom port range. For example, you may want to work with the port range of your web framework. GO, for instance, communicates on port 3000.

1. Open the Azure Stack portal for your tenant.
1. Find your VM. You may have pinned the VM to your dashboard, or you can search for the VM in the **Search resources** box.
1. Select **Networking** in your VM blade.
1. Select **Add inbound port** rule to open a port.
1. For Source, leave the default to **Any**.
1. For Source port range, leave the wildcard (*).
1. For Destination port range, add the port you would like to open, such as `3000`.
1. For **Protocol** leave **Any**.
1. For **Action** set to **Allow**.
1. For **Priority** leave for the default.
1. Type a **Name** and **Description** to help you remember why the port is open.
1. Select **Add**.

#### Add a DNS name for your server

In addition, you can create a DNS name for your server, and then users can connect to your web site using a URL.

1. Open the Azure Stack portal for your tenant.
1. Find your VM. You may have pinned the VM to your dashboard, or you can search for the VM in the **Search resources** box.
1. Select **Overview**.
1. Select **Configure** under VM.
1. Select **Dynamic** for **Assignment**.
1. Type the DNS name label such as `mywebapp` so that your full URL will be: `mywebapp.local.cloudapp.azurestack.external` (for an ASDK app).

### Connect via SSH to update your VM

1. On the same network as your Azure Stack, open your SSH client. For more information, see [How to use an SSH public key](azure-stack-dev-start-howto-ssh-public-key.md).
1. Type:
    ```bash  
        sudo apt-get update
        sudo apt-get -y upgrade
    ```

<!--

## Deploy VM using the PowerShell

Include a sentence or two to explain only what is needed to complete the procedure.

1. Step one of the procedures.

    | Parameter | Example | Description |
    | --- | --- | --- |
    | item      | "dog"   | Describe what it is and where to find the information. |

2. Step two of the procedure

    ```PowerShell  
    verb-command -item "dog"
    ```

3. Step three of the procedures.

    ```PowerShell  
    verb-command -item "dog"
    ```

4. Step four of the procedures.

    ```PowerShell  
    verb-command -item "dog"
    ```

## Deploy VM using the CLI

Include a sentence or two to explain only what is needed to complete the procedure.

1. Step one of the procedures.

    | Parameter | Example | Description |
    | --- | --- | --- |
    | item      | "dog"   | Describe what it is and where to find the information. |

2. Step two of the procedure

    ```CLI  
    verb-command -item "dog"
    ```

3. Step three of the procedures.

    ```CLI  
    verb-command -item "dog"
    ```

4. Step four of the procedures.

    ```CLI  
    verb-command -item "dog"
    ```

-->

## <a name="next-steps"></a>后续步骤

详细了解如何[针对 Azure Stack 进行开发](azure-stack-dev-start.md)