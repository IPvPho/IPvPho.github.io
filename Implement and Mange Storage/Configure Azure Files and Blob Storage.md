[//] <> (AZ-104 Notes | Michael Bender | Pluralsight)

# <ins>***Configure Azure Files and Blob Storage***</ins>
---

### <ins>*Azure File Share*</ins>

> - Cloud-based SMB or NFS file share
> - Accessible from Windows, MacOS, and Linux
> - Clients use Port 445 (check prior that it's open)
>   - 445 is a known attack vector, not recommended to use file share for public protection resources, use Site-to-Site VPN or ExpressRoute
> - Supported in GPv1, GPv2, and FileStorage accounts
> - Replication available depending on storage account


### <ins>*Azure File Share Requirements:*</ins>

> 1. Performance
> 2. Sizing
> 3. Redundancy

#### <ins>*Azure File Share Options*</ins>


|            | Max Size of File Share                   | Performance Tiers | Access Tiers                     | Replication Options                |
|:----------:|------------------------------------------|-------------------|----------------------------------|------------------------------------|
| GPv2       | 5 TB default Up to 100 TiB upon request | Standard          | Hot, Cool, Archive,  Transaction Optimized | LRS, GRS, ZRS, GZRS                |
| GPv1       | 5 TB default Up to 100 TiB upon request | Standard          | N/A                              | LRS, GRS                           |
| FieStorage | 100 TB default                          | Premium           | N/A                              | LRS, ZRS (small subset of regions) |


#### <ins>*Creating Azure File Share with Azure CLI*</ins>

```Bash
> az storage share create \
> --account-name <storageAccountName> \
> --account-key <storageAccountKey> \ or --sas-key <SharedAccessSignatureKey>
> --name <shareName> \
> --quota <SizeInGB>
```
#### <ins>*Creating a Directory within the File Share in Azure CLI*</ins>

```Bash
> az storage directory create \
> --account-name <storageAccountName> \
> --account-key <storageAccountKey> \ or --sas-key <SharedAccessSignatureKey>
> --share-name <FileShareName> \
> --name "<DirectoryName>"
```

#### <ins>*Uploading a file to the File Share with Azure CLI*</ins>

```Bash
> az storage file upload \
> --account-name <storageAccountName> \
> --account-key <storageAccountKey> \ or --sas-key <SharedAccessSignatureKey>
> --share-name <FileShareName> \
> --source "<SourceFile>" \
> --path "<DirectoryName>/<FileName>"
```


### <ins>*Azure Files Access Tiers*</ins>
---

> 1. **Hot:**
>       - Storage optimized for general file share purposes (organizational and Azure file sync)
>       - Storage cost is higher
>       - Access cost is lower
> 2. **Cool:**
>       - Cost efficient storage optimized for online archive storage scenarios
>       - Lower cost of storage
>       - Access cost is higher as it is intended not to be accessed as often as Hot tier
> 3. **Transaction Optimized:**
>       - Transaction heavy workloads that do not need lower-latency
>       - Highest storage-at-rest cost
>       - Lowest access cost per transaction compared to above
>       - Great for applications requiring file shares or backend storage

### <ins>*Authentication*</ins>

> - AAD Domain Services and AD Domain Services
>   - Provides identity-based access for hybrid environments
>   - Allows granular share-level and file-level permissions
> - Storage Account Keys or Shared Access Signatures
>   - Less granular control
>  - **Secure the keys** (best for limited access)

#### <ins>*Azure File Sync*</ins>

> - Provides an on-premises caching solution on Windows Server
> - Replicates data between on-premises DCs and Azure
> - Integrates with Azure File Shares
> - Seamless integration into file share infrastructure


#### <ins>*Azure File Sync Management Components*</ins>

> - **Cloud Endpoint:** 
>  - Azure File Share being synced
> - **Server Endpoint:**
>  - Local server with windows file system path being synced with Azure File Sync and Azure File Share
>  - Define local system path within your sync group 
> - **Sync Group:**
>  - Defines relationship between Cloud endpoint and Server endpoint
>  - Only one cloud endpoint allowed
>  - Multiple server endpoints allowed

![File Sync in Action](https://user-images.githubusercontent.com/3911650/49400286-0a9eb000-f701-11e8-95f9-4ff38170319c.png)

#### <ins>*Deploying Azure FIle Sync*</ins>

> 1. Deploy the Storage Sync Service
> 2. Create a sync group and cloud endpoint
> 3. Install the azure File Sync agent on Windows Server(s)
> 4. Register Windows Server with the Storage Sync Service
> 5. Create a server endpoint and wait for sync

---
## <ins>**Configure Azure Blob Storage**</ins>
---

### <ins>*Azure Binary Large Objects (BLOB)*</ins>

> - Unstructured data objects of various types
>   - Text, binary data, files, docs, streaming video/audio, log files, disaster recovery
> - Supported in BlobStorage, GPv1, GPv2
> - Supported Tiers:
>   -  Hot
>   -  Cool
>   -  Archive
> - Supports various replication options depending on Storage Account used

#### <ins>*Blob Types in Azure*</ins>

> - **Block:**
>   - Text and Binary data
>   - Made up of blobs of data that cane be managed individually
>   - Files and Videos commonly stored
> - **Append:**
>   - Made of Block Blobs optimized for Append operations
>   - Scenarios like logging data from VMs
> - **Page:**
>   - Random access files up to 8 TB
>   - Store VHD files 
>   - Serve as disk for Azure VMs


#### <ins>*Blob Storage Resources*</ins>

> 1. **Storage Account:**
>       - Holds and manages all of Azure data objects (blobs)
> 2. **Container:**
>       - Organizes a set of blobs like a directory in a file system
>       - Storage Accounts can have an unlimited number of containers
>       - Containers can store an unlimited number of blobs
> 3. **Blob:**
>       - Unstructured data objects in Azure Storage
>
> > <ins>**Azure Storage Endpoint for Azure Blob Object:**</ins>
> > https://sablobblob.core.windows.net/media/img1.png
> > - '*sablobl001*' == Storage Account Name
> > - '*blob.core.windows.net*' == Storage Endpoint Service
> > - '*media*' == Container Name
> > - *'img1.png'* == Blob Name

### <ins>*Azure Blob Access Tiers*</ins>

> 1. **Hot:**
>   - Frequently used data, anay data in an active state
>   - Higher Storage Cost
>   - Lower Access Cost
> 2. **Cool:**
>   - Short term backups and recovery sets
>   - Not used often but needs to be accessible when quickly when needed
>   - Lower SLA
>   - Higher Access Cost
>   - Lower Storage Cost
> 3. **Archive:**
>   - Storage for data rarely accessed
>   - 180-day minimum storage time, otherwise fees will be imposed
>   - Will need to "Rehydrate" the data to access it, can take hours if large amounts of data are stored here 
>   - Rehydration comes at a cost
>   - Lowest storage cost
>   - Highest access cost (Hydration)

#### <ins>*Lifecycle Management*</ins>

> - Automate tiering of Blob objects (JSON, PowerShell, Azure CLI)
> - Can take up to 24 hours to deploy

### <ins>*Blob Object Replication*</ins>

> - Asynchronous copies of block blob between storage accounts
> - Supported Scenarios:
>   - Minimizing Latency
>   - Increase efficiency for compute workloads
>   - Optimizing data distribution
>   - Optimizing Costs

> - Caveats for Blob Object Replications:
>   - No Snapshot Support
>   - Hot and Cool tiers only
>   - No immutable blob support


**When configuring Blob Replication you should enable the following options:**

> - *'Turn on versioning for blobs.'*
> - *'Turn on blob change feed.'*


### <ins>*Verify Replication with Azure CLI*</ins>

```PowerShell

az storage blob show \
--account-name sablobeast \
--container-name blobdata \ 
--name file001.txt \
--query 'objectReplicationSourceProperties[].rules[].status' \
--output tsv \
--account-key '98ywhtug0w8viyebvwn08hvpfnwf0jp9fwnunpfunwfoiw/fkhboudcbyc8wn0whk47beiw'

```


