---
title: "Azure PowerShell 脚本示例 - 计算 blob 容器大小 | Microsoft Docs"
description: "通过计算容器各 blob 的总大小来计算 Azure Blob 存储中容器的大小。"
services: storage
documentationcenter: na
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
origin.date: 06/13/2017
ms.date: 10/23/2017
ms.author: v-johch
ms.openlocfilehash: 5005c33fb02ce3647bf9fcfb8c32cfd339c42db7
ms.sourcegitcommit: fea4940a09cecbae36256410227e701e5f0aab6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>计算 Blob 存储容器的大小

此脚本通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="understand-the-size-of-blob-storage-container"></a>了解 Blob 存储容器的大小

Blob 存储容器的总大小包括容器本身的大小，以及容器中所有 blob 的大小。

下面介绍如何为 Blob 容器和 Blob 计算存储容量。 下面的 Len(X) 是指字符串中的字符数。

### <a name="blob-containers"></a>Blob 容器

下面介绍如何估算每个 blob 容器使用的存储空间量：

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

以下是细分：
* 每个容器 48 字节的开销，包括上次修改时间、权限、公共设置，以及其他系统元数据。
* 容器名称作为 Unicode 存储，因此需将字符数乘以 2。
* 对于每个存储的 blob 容器元数据，我们会存储名称的长度（存储为 ASCII），以及字符串值的长度。
* 每个已签名标识符 512 字节，包括已签名标识符名称、开始时间、到期时间和权限。

### <a name="blobs"></a>Blob

下面介绍如何估算每个 blob 使用的存储空间量：

* 块 Blob（基 blob 或快照）

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
SizeInBytes(data in unique committed data blocks stored) +
SizeInBytes(data in uncommitted data blocks)
`

* 页 Blob（基 blob 或快照）

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
number of nonconsecutive page ranges with data * 12 bytes +
SizeInBytes(data in unique pages stored)
`

以下是细分：

* 124 字节的 blob 开销，包括上次修改时间、大小、Cache-Control、Content-Type、Content-Language、Content-Encoding、Content-MD5、权限、快照信息、租约，以及某些系统元数据。
* blob 名称作为 Unicode 存储，因此需将字符数乘以 2。
* 然后，对于每个存储的元数据，我们会存储名称的长度（存储为 ASCII），以及字符串值的长度。
* 然后，对于块 Blob
    * 8 字节用于阻止列表
    * 块数乘以块 ID 大小（单位：字节）
    * 此外还包括所有已提交块和未提交块中的数据的大小。 请注意，使用快照时，此大小仅包括此基 blob 或快照 blob 的唯一数据。 如果一周过后仍未使用未提交的块，则会对其进行垃圾回收。自那以后，将不再对其收费。
* 然后，对于页 Blob
    * 带数据的非连续页范围数乘以 12 字节。 这是在调用 GetPageRanges API 时看到的唯一页范围的数目。
    * 此外还包括所有已存储页的数据的大小（单位：字节）。 请注意，使用快照时，此大小仅包括要计入的基 blob 或快照 blob 的唯一页面。

## <a name="sample-script"></a>示例脚本

```powershell
# this script will show how to get the total size of the blobs in a container
# before running this, you need to create a storage account, create a container,
#    and upload some blobs into the container
# note: this retrieves all of the blobs in the container in one command.
#       connect Azure with Login-AzureRmAccount before you run the script.
# command line usage: script.ps1 -ResourceGroup {YourResourceGroupName} -StorageAccountName {YourAccountName} -ContainerName {YourContainerName}
#

param(
    [Parameter(Mandatory=$true)]
    [string]$ResourceGroup,

    [Parameter(Mandatory=$true)]
    [string]$StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]$ContainerName
)

# The script has been tested on Powershell 5.0
Set-StrictMode -Version 5

$VerbosePreference = "Continue"

if((Get-Module -ListAvailable Azure) -eq $null)
{
    throw "Azure Powershell not found! Please install from http://www.windowsazure.com/en-us/downloads/#cmd-line-tools"
}

# function Get-BlobBytes

function Get-BlobBytes
{
    param(
        [Parameter(Mandatory=$true)]
        $Blob)

    # Base + blobname
    $blobSizeInBytes = 124 + $Blob.Name.Length * 2

    # Get size of metadata
    $metadataEnumerator=$Blob.ICloudBlob.Metadata.GetEnumerator()
    while($metadataEnumerator.MoveNext())
    {
        $blobSizeInBytes += 3 + $metadataEnumerator.Current.Key.Length + $metadataEnumerator.Current.Value.Length
    }

    if($Blob.BlobType -eq [Microsoft.WindowsAzure.Storage.Blob.BlobType]::BlockBlob)
    {
        $blobSizeInBytes += 8
        # Default is Microsoft.WindowsAzure.Storage.Blob.BlockListingFilter.Committed. Need All
        $Blob.ICloudBlob.DownloadBlockList([Microsoft.WindowsAzure.Storage.Blob.BlockListingFilter]::All) |
            ForEach-Object { $blobSizeInBytes += $_.Length + $_.Name.Length }
    }
    else
    {
        $Blob.ICloudBlob.GetPageRanges() |
            ForEach-Object { $blobSizeInBytes += 12 + $_.EndOffset - $_.StartOffset }
    }

    return $blobSizeInBytes
}

# function Get-ContainerBytes

function Get-ContainerBytes
{
    param(
        [Parameter(Mandatory=$true)]
        [Microsoft.WindowsAzure.Storage.Blob.CloudBlobContainer]$Container)

    # Base + name of container
    $containerSizeInBytes = 48 + $Container.Name.Length*2

    # Get size of metadata
    $metadataEnumerator = $Container.Metadata.GetEnumerator()
    while($metadataEnumerator.MoveNext())
    {
        $containerSizeInBytes += 3 + $metadataEnumerator.Current.Key.Length + $metadataEnumerator.Current.Value.Length
    }

    # Get size for SharedAccessPolicies
    $containerSizeInBytes += $Container.GetPermissions().SharedAccessPolicies.Count * 512

    # Calculate size of all blobs.
    $blobCount = 0
    $Token = $Null
    $MaxReturn = 5000

    do {
        $Blobs = Get-AzureStorageBlob -Context $storageContext -Container $Container.Name -MaxCount $MaxReturn -ContinuationToken $Token
        if($Blobs -eq $Null) { break }

        $Token = $Null
        if (Get-Member -InputObject $Blobs -Name Count -MemberType Properties)
        {
            $Token = $Blobs[$Blobs.Count - 1].ContinuationToken;
        }

        $Blobs | ForEach-Object {
                $blobSize = Get-BlobBytes $_
                $containerSizeInBytes += $blobSize
                $blobCount++

                if(($blobCount % 1000) -eq 0)
                {
                    Write-Verbose("Counting {0} Sizing {1} " -f $blobCount, $containerSizeInBytes)
                }
            }
    }
    While ($Token -ne $Null)

    return @{ "containerSize" = $containerSizeInBytes; "blobCount" = $blobCount }
}

#Login-AzureRmAccount

$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroup -Name $StorageAccountName -ErrorAction SilentlyContinue
if($storageAccount -eq $null)
{
    throw "The storage account specified does not exist in this subscription."
}

$storageContext = $storageAccount.Context

$containers = New-Object System.Collections.ArrayList
if($ContainerName.Length -ne 0)
{
    $container = Get-AzureStorageContainer -Context $storageContext -Name $ContainerName -ErrorAction SilentlyContinue |
        ForEach-Object { $containers.Add($_) } | Out-Null
}
else
{
    Get-AzureStorageContainer -Context $storageContext | ForEach-Object { $containers.Add($_) } | Out-Null
}

$sizeInBytes = 0

if($containers.Count -gt 0)
{
    $containers | ForEach-Object {
        Write-Output("Calculating container {0} ..." -f $_.CloudBlobContainer.Name)
        $result = Get-ContainerBytes $_.CloudBlobContainer
        $sizeInBytes += $result.containerSize

        Write-Output("Container '{0}' with {1} blobs has a sizeof {2:F2} MB." -f $_.CloudBlobContainer.Name,$result.blobCount,($result.containerSize/1MB))
    }
}
else
{
    Write-Warning "No containers found to process in storage account '$StorageAccountName'."
}
```

## <a name="next-steps"></a>后续步骤

有关 Azure 存储计费的详细信息，请参阅 [Understanding Windows Azure Storage Billing](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/)（了解 Windows Azure 存储计费）。

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure 存储的 PowerShell 示例](../blobs/storage-samples-blobs-powershell.md)中找到其他存储 PowerShell 脚本示例。
