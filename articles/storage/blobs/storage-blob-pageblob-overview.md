---
title: Unique feature of Azure page blobs | Microsoft Docs
description: Overview about the Azure page blobs, benefits and use cases with sample scripts. 
services: storage
documentationcenter: ''
author: anasouma
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: wielriac
---


# Unique Features of Azure Page Blobs

Azure Storage offers three types of blob storage: Block Blobs, Append Blobs and Page Blobs. Block blobs are composed of blocks and are ideal for storing text or binary files, and for uploading large files efficiently. Append blobs are also made up of blocks, but they are optimized for append operations, making them ideal for logging scenarios. Page blobs are made up of 512-byte pages up to 8 TB in total size and are more efficient for frequent random read/write operations. Page blobs are the foundation of Azure IaaS Disks. This article focuses on explaining the features and benefits of Page Blobs.

## Overview
Page Blobs are a collection of 512-byte pages, which provide the ability to read/write arbitrary ranges of bytes. Hence, Page Blobs are ideal for storing index-based and sparse data structures like OS and data disks for Virtual Machines and Databases. For example, Azure SQL DB uses page blobs as the underlying persistent storage for its databases. Moreover, Page Blobs are also often used for files with Range-Based updates.  

Key features of Azure Page Blobs are its REST interface, the durability of the underlying storage, and the seamless migration capabilities to Azure. We will discuss these features in more details in the next section. In addition, Azure Page Blobs are currently supported on two types of storage: Premium Storage and Standard Storage. Premium Storage is designed specifically for workloads requiring consistent high performance and low latency making premium page blobs ideal for high performant data storage databases.  Standard Storage is more cost effective for running latency-insensitive workloads.

## Sample Use Cases

Let’s discuss a couple of use cases for Page Blobs starting with Azure IaaS Disks. Azure Page Blobs are the backbone of the virtual disks platform for Azure IaaS. Both Azure OS and data disks are implemented as virtual disks where data is durably persisted in the Azure Storage platform and then delivered to the virtual machines for maximum performance. Azure Disks are persisted in Hyper-V [VHD format](https://technet.microsoft.com/library/dd979539.aspx) and stored as a [Page Blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) in Azure Storage. In addition to using virtual disks for Azure IaaS VMs, Page Blobs also enable PaaS and DBaaS scenarios such as Azure SQL DB service, which currently uses Page Blobs for storing SQL data, enabling fast random read-write operations for the database. Another example would be if you have a PaaS service for shared media access for collaborative video editing applications, page blobs enable fast access to random locations in the media. It also enables fast and efficient editing and merging of the same media by multiple users. 

First party Microsoft services like Azure Site Recovery, Azure Backup, as well as many third-party developers have implemented industry-leading innovations using page blob’s REST interface. Following are some of the unique scenarios implemented on Azure: 
* Application-directed incremental snapshot management: Applications can leverage Page Blob Snapshots and REST APIs for saving the application checkpoints without incurring costly duplication of data. This is possible because we support local snapshots for page blobs, which don’t require copying the whole data. These public snapshot APIs also enable accessing and copying of deltas between snapshots.
* Live migration of application and data from on-prem to cloud: Copy the on-prem data and use REST APIs to write directly to Azure Page Blob while the on-prem VM continues to run. Once the target has caught up, you can quickly failover to Azure VM using that data. This enables migration of your VMs and virtual disks from on-prem to cloud with minimal downtime since the data migration occurs in the background while you continue to use the VM and the downtime needed for failover will be short (in minutes).
* [SAS-based](../common/storage-dotnet-shared-access-signature-part-1.md) shared access, which enables scenarios like multiple-readers and single-writer with support for concurrency control.

## Page Blob Features

### REST API
Refer to the following document to get started with [developing using Page Blobs](storage-dotnet-how-to-use-blobs.md). As an example, let us look at how to access Page Blobs using Storage Client Library for .NET. 

The following diagram describes the overall relationships between account, containers, and page blobs.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### Creating an empty Page Blob of a certain size
To create a Page Blob, we first create a CloudBlobClient object, with the base Uri for accessing the blob storage for your storage account (pbaccount in figure 1) along with the StorageCredentialsAccountAndKey object as shown below.  The example then shows creating a reference to a CloudBlobContainer object, and then creating the container (testvhds) if it doesn’t already exist.  Then using the CloudBlobContainer object, we can create a reference to a CloudPageBlob object by specifying the page blob name (os4.vhd) we want to access. Then to create the page blob we call [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) passing in the max size for the blob we want to create.  The blobSize has to be a multiple of 512 bytes.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### Resizing a Page Blob
To resize a Page Blob after creation, use the [Resize](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. The requested size should be a multiple of 512 bytes.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### Writing Pages to a Page Blob
To write pages,  use the [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) method.  This allows you to write a sequential set of pages up to 4MBs. The offset being written to must start on a 512-byte boundary (startingOffset % 512 == 0), and end on a 512 boundary - 1.  The code below shows an example of calling WritePages for a blob object we are accessing:
```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

As soon as a write request for a sequential set of pages succeeds in the blob service and is replicated for durability and resiliency, the write has committed, and success is returned back to the client.  

The below diagram shows 2 separate write operations:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.	A Write operation starting at offset 0 of length 1024 bytes 
2.	A Write operation starting at offset 4096 of length 1024 

#### Reading Pages from a Page Blob
To read pages, use the [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) method to read a range of bytes from the page blob. This allows you to download the full blob or range of bytes starting from any offset in the blob. When reading, the offset does not have to start on a multiple of 512. When reading bytes from a NUL page, the service returns zero bytes.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
The following figure shows a Read operation with BlobOffSet of 256 and rangeSize of 4352. Data returned is highlighted in Orange. Zeros are returned for NUL pages

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

If you have a sparsely populated blob you may want to just download the valid page regions to avoid paying for egressing of zero bytes and to reduce download latency.  To determine which pages are backed by data, use [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). You can then enumerate the returned ranges and download the data in each range. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### Leasing a Page Blob
The Lease Blob operation establishes and manages a lock on a blob for write and delete operations. This operation is quite useful in scenarios where a page blob is being accessed from multiple clients to ensure only one client can write to the blob at a time. Azure Disks, for example,  leverages this leasing mechanism to ensure the disk is only managed by a single VM. The lock duration can be 15 to 60 seconds, or can be infinite. See the documentation [here](/rest/api/storageservices/lease-blob) for more details.

> Use the following link to get [code samples](/resources/samples/?service=storage&term=blob&sort=0) for many other application scenarios. 

In addition to rich REST APIs, Page blobs also provide shared access, durability, and enhanced security. We will cover those benefits in more detail in the next paragraphs. 

### Concurrent Access
The Page Blobs REST API and it’s leasing mechanism allows applications to access the page blob from multiple clients. For example, let’s say you need to build a distributed cloud service that shares storage objects with multiple users. It could be a web application serving a large collection of images to several users. One option for implementing this is to use a VM with attached disks. Downsides of this include, (i) the constraint that a disk can only be attached to a single VM thus limiting the scalability, flexibility, and increasing risks. If there is a problem with the VM or the service running on the VM, then due to the lease, the image will be inaccessible until the lease expires or is broken; and (ii) Additional cost of having an IaaS VM. 

An alternative option is to use the Page Blobs directly via Azure Storage REST APIs. This option eliminates the need for costly IaaS VMs, offers full flexibility of direct access from multiple clients, simplifies the service management by eliminating the need to attach/detach disks, and eliminates the risk of issues on the VM. And, it provides the same level of performance for random read/write operations as a disk

### Durability and High-Availability
Both Standard and premium storage are durable storage where the page blob data is always replicated to ensure durability and high availability. For more information about Azure Storage Redundancy, see this [documentation](../common/storage-redundancy.md). Azure has consistently delivered enterprise-grade durability for IaaS disks and page blobs, with an industry-leading ZERO % [Annualized Failure Rate](https://en.wikipedia.org/wiki/Annualized_failure_rate). That is, Azure has never lost a customer’s page blob data. 

### Seamless Migration to Azure
For the customers and developers who are interested in implementing their own customized backup solution, Azure also offers incremental snapshots that only hold the deltas. This feature avoids the cost of the initial full copy, which greatly lowers the backup cost. Along with the ability to efficiently read and copy differential data, this is another powerful capability that enables even more innovations from developers, leading to a best-in-class backup and disaster recovery (DR) experience on Azure. You can setup your own backup or DR solution for your VMs on Azure using [Blob Snapshot](/rest/api/storageservices/snapshot-blob) along with the [Get Page Ranges](/rest/api/storageservices/get-page-ranges) API and the [Incremental Copy Blob](/rest/api/storageservices/incremental-copy-blob) API, which you can use for easily copying the incremental data for DR. 

Moreover, many enterprises have critical workloads already running in on-premises datacenters. For migrating the workload to the cloud, one of the main concerns would be the amount of downtime needed for copying the data, and the risk of unforeseen issues after the switchover. In many cases, the downtime can be a showstopper for migration to the cloud. Using the Page Blobs REST API, Azure addresses this problem by enabling cloud migration with minimal disruption to critical workloads. 

For examples on how to take a snapshot and how to restore a page blob from a snapshot, please refer to the [setup a backup process using incremental snapshots](../../virtual-machines/windows/incremental-snapshots.md) article.


## Summary
Azure strives to deliver the best-in-class experience for our customers. We built the storage platform with enterprise-grade durability of data, so our customers can trust Azure with their critical data. Azure offers the unique API support and developer experience for Page Blobs, which no other public cloud platform can provide. This has led to many 1st-party and 3rd-party innovations like seamless cloud migrations, superior backup/DR experience, PaaS and DBaaS support, distributed storage solutions, and other innovations for IaaS VMs/disks. Overall, Azure stands out among all the public cloud platforms, with these unique capabilities, and Azure customers benefits from these value-adds that no other cloud platforms can provide.
