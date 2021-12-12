[//] <> (AZ-104 Notes | Michael Bender | Pluralsight)

# <ins>***Manage Data in Azure Storage***</ins>
---

## <ins>**Working with Azure Import/Export Service**</ins>

#### <ins>*Migrating Data into Azure*</ins>

- Can be sent over the wire for smaller amounts of data
- Shipping physical media is more ideal for larger data sets (TBs or larger)

#### <ins>*Data Management Tools*</ins>

1. <ins>**Azure Import/Export Service**</ins>
   - Allows data to be copied to physical drives for import or export into/out of Azure Storage
   - Want to use when you have a large amount of data to me in/out of blobs
2. <ins>**AzCopy**</ins>
    - CLI tool for over-the-wire transfer
    - Allows transfer of data from:
      -  computer to storage account 
      -  storage account to storage account
3. <ins>**Azure Storage Explorer**</ins>
    - Desktop based GUI
    - Administrate Azure storage
    - Can upload/download data
    - Container creation, SAS Key creation, etc.

#### <ins>**Azure Import/Export Service**</ins>

> - Securely import/export large amounts of data with physical drives
> - Create jobs in Azure Portal or Azure Resource Manager REST API
> - Import to Azure BlobStorage and Azure Files
> - Export to Azure BlobStorage
> - Supports General Purpose v1, v2 and BlobStorage storage accounts
> 
> <ins>**Use Cases:**</ins>
>
> - Backups
> - Content Distribution
> - Data Recovery
>
> <ins>**Service Components:**</ins>
> 
> - Azure Import/Export Service
> - WAImportExportTool
> - Physical Drives (You Provide) 
>   - SSD or HDD
>   - Size and amount of files depends on amount of data
> - Azure DataBox
>   - Microsoft provides piece of hardware relevant to your situation for transfer.

#### <ins>*Importing Blobs and Files to Azure*</ins>

> - Drives prepped with WAImportExportTool
> - Import Job Created (Azure)
> - Drives shipped to Azure DC
> - Processed drive data copied to storage account and ready for verification
>   - Verify data is accurate before deleting on-prem backups
> - Drives returned to customer

#### <ins>*Exporting Blobs from Azure*</ins>

> - Create export job in Azure Portal
> - Ship drives to DC
> - Export data copied to encrypted drive
> - Drives shipped to customer
> - Unlock and verify drive with WAImportExportTool 'Unlock' command

#### *WAImportExportTool*

> - CLI tool run on Windows 64-bit *ONLY*
> - Encryption, decryption and data copy
> - Creation of journal files
> - Determine number of drives needed for export job
> 2 version for Azure Blobs (v1) and Azure Files (v2)


#### <ins>**Copy and Prepare Data Drive for Azure File Import**</ins>

```PowerShell

 .\WAImportExport.exe PrepImport
    /j:<JournalFile>
    /id:<SessionId>
    [/logdir:<LogDirectory>]
    [/sk:<StorageAccountKey>] {/silentmode}
    [/IntialDriveSet:<driveset.csv>]
    /DataSet:<dataset.csv>

```

---
## <ins>**Managing Data with AzCopy and Azure Data Storage Explorer**</ins>

### <ins>*AzCopy*</ins>

#### *AzCopy Syntax:*

```PowerShell

azcopy [command] [arguements]
--[flag-name]=[flag-value]

azcopy copy 'H:\data'
'https://sabloblstore001.blob.core.windows.net/blobdata' --recursive

```

#### <ins>*Supported Authentication Credentials for AzCopy*</ins>

<ins>**BlobStorage:**</ins>
- SAS
- Azure AD
  - *Ensure proper roles are assigned*

<ins>**FileStorage:**</ins>
- SAS

---

## <ins>**Azure Storage Explorer**</ins>

> - Manage Azure Storage from your desktop
> - Authentication based on storage account
> - Runs on Windows, MacOS, and Linux
> - Supports Multiple Subscriptions

