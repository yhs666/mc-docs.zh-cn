---
title: "使用 C# 部署 Azure 虚拟机 | Azure"
description: "使用 C# 和 Azure Resource Manager 部署虚拟机及其所有支持资源。"
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 06/05/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: 531905fd1b1d06e409db16fea0b72585332554ae
ms.sourcegitcommit: 51a25dbbf5f32fe524860b1bb107108122b47bf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
<a id="create-a-windows-vm-in-azure-using-c" class="xliff"></a>

# 在 Azure 中使用 C# 创建 Windows VM #

[Azure 虚拟机](overview.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json) (VM) 需要多个支持性的 Azure 资源。 本文介绍如何使用 C# 创建和删除 VM 资源。 你将学习如何执行以下操作：

> [!div class="checklist"]
> * 创建 Visual Studio 项目
> * 安装库
> * 添加用于创建凭据的代码
> * 添加用于创建资源的代码
> * 添加用于删除资源的代码
> * 运行应用程序

完成这些步骤大约需要 20 分钟。

<a id="create-a-visual-studio-project" class="xliff"></a>

## 创建 Visual Studio 项目

1. 如果尚未安装，请安装 [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio)。 在“工作负荷”页上选择“.NET 桌面开发”，然后单击“安装”。 在摘要中，可以看到系统自动选择了“.NET Framework 4 - 4.6 开发工具”。 如果已安装 Visual Studio，则可以使用 Visual Studio 启动器添加 .NET 工作负荷。
2. 在 Visual Studio 中，单击“文件” > “新建” > “项目”。
3. 在“模板” > “Visual C#”中，选择“控制台应用(.NET Framework)”，输入 *myDotnetProject* 作为项目名称，选择项目的位置，然后单击“确定”。

<a id="install-libraries" class="xliff"></a>

## 安装库

使用 NuGet 包可以最轻松地安装完成这些步骤所需的库。 若要在 Visual Studio 中获取所需的库，请执行以下步骤：

1. 在解决方案资源管理器中右键单击“myDotnetProject”，然后依次单击“管理解决方案的 NuGet 包”和“浏览”。
2. 在搜索框中键入 *Microsoft.IdentityModel.Clients.ActiveDirectory*，单击“安装”，然后按照说明安装该包。
3. 在搜索框中键入 *Microsoft.Azure.Management.Compute*，单击“安装”，然后按照说明安装该包。
4. 在搜索框中键入 *Microsoft.Azure.Management.Network*，单击“安装”，然后按照说明安装该包。
5. 在搜索框中键入 *Microsoft.Azure.Management.ResourceManager*，单击“安装”，然后按照说明安装该包。

现在，你可以开始使用这些库来创建应用程序了。

<a id="add-code-to-create-credentials" class="xliff"></a>

## 添加用于创建凭据的代码

在开始此步骤之前，请确保能够访问 [Active Directory 服务主体](../../azure-resource-manager/resource-group-create-service-principal-portal.md)。 此外，应记下应用程序 ID、身份验证密钥和租户 ID，以便在后面的步骤中使用。

1. 打开创建的 Program.cs 文件，然后将这些 using 语句添加到文件顶部的现有语句：

    ```
    using Microsoft.Azure;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Network;
    using Microsoft.Azure.Management.Network.Models;
    using Microsoft.Azure.Management.Compute;
    using Microsoft.Azure.Management.Compute.Models;
    using Microsoft.Rest;
    ```

2. 接下来，在 Program.cs 文件的 Main 方法中添加变量，用于指定在代码中使用的通用值：

    ```   
    var groupName = "myResourceGroup";
    var subscriptionId = "subsciption-id";
    var location = "chinanorth";
    ```

    将 **subscription-id** 替换为你的订阅标识符。

3. 若要创建发出请求时所需的 Active Directory 凭据，请将此方法添加到 Program 类：

    ```    
    private static async Task<AuthenticationResult> GetAccessTokenAsync()
    {
      var cc = new ClientCredential("application-id", "authentication-key");
      var context = new AuthenticationContext("https://login.chinacloudapi.cn/tenant-id");
      var token = await context.AcquireTokenAsync("https://management.chinacloudapi.cn/", cc);
      if (token == null)
      {
        throw new InvalidOperationException("Could not get the token");
      }
      return token;
    }
    ```

    将 **application-id**、**authentication-key** 和 **tenant-id** 替换为前面在创建 Azure Active Directory 服务主体时收集的值。

4. 若要调用前面添加的方法，请将以下代码添加到 Program.cs 文件中的 Main 方法：

    ```
    var token = GetAccessTokenAsync();
    var credential = new TokenCredentials(token.Result.AccessToken);
    ```

<a id="add-code-to-create-resources" class="xliff"></a>

## 添加用于创建资源的代码

<a id="initialize-management-clients" class="xliff"></a>

### 初始化管理客户端

在 Azure 中使用 .NET SDK 创建和管理资源时需要用到管理客户端。 若要创建管理客户端，请将此代码添加到 Program.cs 文件中的 Main 方法：

```
var resourceManagementClient = new ResourceManagementClient(new Uri("https://management.chinacloudapi.cn/"), credential)
    { SubscriptionId = subscriptionId };
var networkManagementClient = new NetworkManagementClient(credential)
    { SubscriptionId = subscriptionId };
var computeManagementClient = new ComputeManagementClient(credential)
    { SubscriptionId = subscriptionId };
```

<a id="create-the-vm-and-supporting-resources" class="xliff"></a>

### 创建 VM 和支持资源

所有资源必须包含在[资源组](../../azure-resource-manager/resource-group-overview.md)中。

1. 将以下方法添加到 Program 类，以创建资源组：

    ```
    public static async Task<ResourceGroup> CreateResourceGroupAsync(
      ResourceManagementClient resourceManagementClient,
      string groupName,
      string location)
    {   
      var resourceGroup = new ResourceGroup { Location = location };
      return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(
        groupName, 
        resourceGroup);
    }
    ```

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```   
    var rgResult = CreateResourceGroupAsync(
      resourceManagementClient,
      groupName,
      location);
    Console.WriteLine("Resource group creation: " + 
      rgResult.Result.Properties.ProvisioningState + 
      ". Press enter to continue...");
    Console.ReadLine();
    ```

使用[可用性集](tutorial-availability-sets.md)可以更方便地维护应用程序所用的虚拟机。

1. 若要创建可用性集，请将以下方法添加到 Program 类：

    ```   
    public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
      ComputeManagementClient computeManagementClient,
      string groupName,
      string location)
    {
      return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
        groupName,
        "myAVSet",
        new AvailabilitySet()
          { 
            Sku = new Microsoft.Azure.Management.Compute.Models.Sku("Aligned"),
            PlatformFaultDomainCount = 3,
            Location = location 
          }
      );
    }
    ```

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```
    var avResult = CreateAvailabilitySetAsync(
      computeManagementClient,
      groupName,
      location);
    Console.WriteLine("Availability set created. Press enter to continue...");
    Console.ReadLine();
    ```

与虚拟机通信需要[公共 IP 地址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)。

1. 若要创建虚拟机的公共 IP 地址，请将以下方法添加到 Program 类：

    ```
    public static async Task<PublicIPAddress> CreatePublicIPAddressAsync( 
      NetworkManagementClient networkManagementClient,
      string groupName,
      string location)
    {
      return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
        groupName,
        "myIpAddress",
        new PublicIPAddress
          {
            Location = location,
            PublicIPAllocationMethod = "Dynamic"
          }
      );
    }
    ```

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```   
    var ipResult = CreatePublicIPAddressAsync(
      networkManagementClient,
      groupName,
      location);
    Console.WriteLine("IP address creation: " + 
      ipResult.Result.ProvisioningState + 
      ". Press enter to continue...");  
    Console.ReadLine();
    ```

虚拟机必须在[虚拟网络](../../virtual-network/virtual-networks-overview.md)的子网中。

1. 若要创建子网和虚拟网络，请将以下方法添加到 Program 类：

    ```   
    public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
      NetworkManagementClient networkManagementClient,
      string groupName,
      string location)
    {
      var subnet = new Subnet
        {
          Name = "mySubnet",
          AddressPrefix = "10.0.0.0/24"
        };
      var address = new AddressSpace 
        {
          AddressPrefixes = new List<string> { "10.0.0.0/16" }
        };
      return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
        groupName,
        "myVNet",
        new VirtualNetwork
          {
            Location = location,
            AddressSpace = address,
            Subnets = new List<Subnet> { subnet }
          }
      );
    }
    ```

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```   
    var vnResult = CreateVirtualNetworkAsync(
      networkManagementClient,
      groupName,
      location);
    Console.WriteLine("Virtual network creation: " + 
      vnResult.Result.ProvisioningState + 
      ". Press enter to continue...");  
    Console.ReadLine();
    ```

虚拟机需要使用网络接口在虚拟网络上通信。

1. 若要创建网络接口，请将以下方法添加到 Program 类：

    ```   
    public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
      NetworkManagementClient networkManagementClient,
      string groupName,
      string location)
    {
      var subnet = await networkManagementClient.Subnets.GetAsync(
        groupName,
        "myVNet",
        "mySubnet"
      );
      var publicIP = await networkManagementClient.PublicIPAddresses.GetAsync(
        groupName, 
        "myIPaddress"
      );
      return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
        groupName,
        "myNic",
        new NetworkInterface
          {
            Location = location,
            IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                  {
                    Name = "myNic",
                    PublicIPAddress = publicIP,
                    Subnet = subnet
                  }
              }
          }
      );
    }
    ```

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```
    var ncResult = CreateNetworkInterfaceAsync(
      networkManagementClient,
      groupName,
      location);
    Console.WriteLine("Network interface creation: " + 
      ncResult.Result.ProvisioningState + 
      ". Press enter to continue...");  
    Console.ReadLine();
    ```

创建所有支持资源后，即可创建虚拟机。

1. 若要创建虚拟机，请将以下方法添加到 Program 类：

    ```   
    public static async Task<VirtualMachine> CreateVirtualMachineAsync(
      NetworkManagementClient networkManagementClient,
      ComputeManagementClient computeManagementClient,
      string groupName,
      string location)
    {

      var nic = await networkManagementClient.NetworkInterfaces.GetAsync(
        groupName, 
        "myNic");
      var avSet = await computeManagementClient.AvailabilitySets.GetAsync(
        groupName, 
        "myAVSet");
      return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
        groupName,
        "myVM",
        new VirtualMachine
          {
            Location = location,
            AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
            HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_DS1"
              },
            OsProfile = new OSProfile
              {
                AdminUsername = "azureuser",
                AdminPassword = "Azure12345678",
                ComputerName = "myVM",
                WindowsConfiguration = new WindowsConfiguration
                  {
                    ProvisionVMAgent = true
                  }
              },
            NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                  {
                    new NetworkInterfaceReference { Id = nic.Id }
                  }
              },
            StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                  {
                    Publisher = "MicrosoftWindowsServer",
                    Offer = "WindowsServer",
                    Sku = "2012-R2-Datacenter",
                    Version = "latest"
                  }
              }
          }
      );
    }
    ```

    > [!NOTE]
    > 本教程创建运行 Windows Server 操作系统版本的虚拟机。 若要详细了解如何选择其他映像，请参阅[使用 Windows PowerShell 和 Azure CLI 来导航和选择 Azure 虚拟机映像](../linux/cli-ps-findimage.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。
    > 
    >

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```
    var vmResult = CreateVirtualMachineAsync(
      networkManagementClient,
      computeManagementClient,
      groupName,
      location);
    Console.WriteLine("Virtual machine creation: " + 
      vmResult.Result.ProvisioningState + 
      ". Press enter to continue...");
    Console.ReadLine();
    ```

<a id="add-code-to-delete-resources" class="xliff"></a>

## 添加用于删除资源的代码

由于需要为 Azure 中使用的资源付费，因此，删除不再需要的资源总是一种良好的做法。 如果要删除虚拟机和所有支持资源，只需删除资源组。

1. 若要删除资源组，请将此方法添加到 Program 类：

    ```
    public static async void DeleteResourceGroupAsync(
      ResourceManagementClient resourceManagementClient,
      string groupName)
    {
      Console.WriteLine("Deleting resource group...");
      await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
    }
    ```

2. 若要调用前面添加的方法，请将以下代码添加到 Main 方法：

    ```   
    DeleteResourceGroupAsync(
      resourceManagementClient,
      groupName);
    Console.ReadLine();
    ```

3. 保存项目。

<a id="run-the-application" class="xliff"></a>

## 运行应用程序

1. 若要运行控制台应用程序，请在 Visual Studio 中单击“启动”，然后使用用于订阅的相同用户名和密码登录到 Azure AD。

2. 返回每个资源的状态后，请按 **Enter**。 在状态信息中，应会看到“成功”预配状态。 创建虚拟机后，可以删除所创建的所有资源。 在按 **Enter** 开始删除资源之前，可能需要在 Azure 门户中花几分钟时间来验证资源的创建。 如果已打开 Azure 门户，则可能需要刷新边栏选项卡才能看到新资源。  

    控制台应用程序从头到尾完成运行大约需要五分钟时间。 应用程序完成运行之后，可能需要花费几分钟时间来删除所有资源和资源组。

<a id="next-steps" class="xliff"></a>

## 后续步骤
* 参考[使用 C# 和 Resource Manager 模板部署 Azure 虚拟机](csharp-template.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中的信息，利用模板创建虚拟机。
* 查看[使用 Azure Resource Manager 和 C# 管理 Azure 虚拟机](csharp-manage.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)，了解如何管理创建的虚拟机。
