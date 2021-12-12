[//] <> (AZ-104 | Michael Teske | Pluralsight)

# <ins>***Automate Deployment and Configuration of Virtual Machines***</ins>
---


## <ins>*Manage Deployments with ARM Templates*</ins>

### *Arm Templates:*

- JSON Format
- Contains the resources to create in Azure
- Submit template to the Azure Resource Manager (ARM)
- Tracked Deployments

**Basic Arm Template Format:**

```JSON
{
  
    "$schema": "https://chema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#", 
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "functions": [],
    "variables":{},
    "resources": [],
    "outputs": {}

}
```

- Required Sections:
  - Schema, contentVersion, and resources


### *Modify ARM Templates:*

- Can modify existing template in portal
  - Choose 'Export' template under 'Automation'
  - Select 'Deploy' to edit template
  - Select 'Purchase'


### *Steps to deploy from template:*

1. Generate template in the portal
2. Download the template (zip file)
3. Edit and deploy the modified template
4. Find 'Deploy a Custom Template' blade
5. Choose 'build your own template' and upload the file to deploy


### *Save a Deployment as an ARM Template:*

- Locate resource group in the portal
- Choose 'Export' template
- Download the template

*Using PowerShell:*

```PowerShell

Export-AzResourceGroup -ResourceGroupName "ps-course-rg" -Path "C:\downloads"

-Resource /subscriptions/123/resourceGroups/ps-course-rg `
/providers/Microsoft.network/networksecuritygroups/web-nsg

```
---

## <ins>*Automate Configuration and Deployment of VMs*</ins>

### *Custom Script Extensions*

- Scripts can be located anywhere
- Scripts can be deployed with ARM templates
- Scripts will only run once
- Removing the extension does not undo what the script did
- Write scripts that are idempotent (If it runs multiple times it will always produce the same outcome)


### *Configure VHD Templates:*

- SysPrep managed image with support up to 20 simultaneous deployments
- Capture image, provide image name
- Choose to have VM deleted after capture
- Provide VM name to confirm the process
  

  