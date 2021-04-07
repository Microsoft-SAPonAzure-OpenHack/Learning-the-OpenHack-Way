- [Architecture of the Start/Stop solution](#architecture-of-the-start-stop-solution)
- [Deployed SAP Landscape](#deployed-sap-landscape)
- [Tasks for participents](#tasks-for-participents)
- [Implementation details](#implementation-details)
  * [**Create Azure Automation Account**](#--create-azure-automation-account--)
  * [**Import Az.Modules**](#--import-azmodules--)
  * [**Import SAP PowerShell Module**](#--import-sap-powershell-module--)
  * [**Import SAP Runbooks**](#--import-sap-runbooks--)
  * [Tagging and executing Runbooks](#tagging-and-executing-runbooks)
  * [Runbook Description](#runbook-description)
- [Access consideration for Azure Automation Runbooks and Jobs](#access-consideration-for-azure-automation-runbooks-and-jobs)
  * [Limiting access to individual runbooks/jobs](#limiting-access-to-individual-runbooks-jobs)

# Architecture of the Start/Stop solution 

The solution is using Azure automation account PaaS solution to execute the SAP shutdown/startup jobs (as shown in the below diagram).
Runbooks are written in PowerShell. There is also a PowerShell module SAPAzurePowerShellModules that is used by all runbooks. These runbooks and module are stored in PowerShell Gallery, and are easy to import. 

![](Pictures/media/image5.png)

Information about SAP landscape and instances are to be stored in VM Tags.

Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables. These assets are encrypted and stored in Azure Automation using a unique key that is generated for each Automation account. [Azure Automation stores the key in the system-managed Key Vault](https://docs.microsoft.com/en-us/azure/automation/shared-resources/credentials?tabs=azure-powershell). Before storing a secure asset, Automation loads the key from Key Vault and then uses it to encrypt the asset.

SAP system start / stop / monitoring and SAP Application server Start / stop is implemented using scripts (calling SAP sapcontrol executable) via the Azure VM agent. 

SAP HANA start / stop / monitoring is implemented using scripts (calling SAP sapcontrol executable) via the Azure VM agent. 

SQL Server start / stop / monitoring is implemented using scripts (calling SAP Host Agent executable) via the Azure VM agent. 

Azure runbooks can either be executed manually or scheduled. 

# Deployed SAP Landscape

In the deployed SAP landscape we have: (Deployment scenario #2 in below diagram)

* SAP ABAP ASCS instance
*	ONE DBMS instance (HA for DBMS is currently not implemented)
* One or more SAP application servers 

![image](https://user-images.githubusercontent.com/26795040/113913301-c83e7f80-97a1-11eb-891c-8ba22219ae26.png)

# Tasks for participents

- Create Azure Automation account

- Tag all the VMs for the **SID** in place

- List the SID to ensure all the systems were tagged correctly

- Execute runbook manually to stop systems

- Schedule the runbook to start the systems

- Schedule scale out & scale in out the Application servers


# Implementation details

This section details the implementation flow of the solution

## **Create Azure Automation Account**

 [How to create Azure automation account](https://docs.microsoft.com/en-us/azure/automation/automation-quickstart-create-account).



## **Import Az.Modules**

SAP start / stop PowerShell (PS) runbooks use new Az PS module, which must be [imported](https://docs.microsoft.com/en-us/azure/automation/az-modules#import-the-az-modules) .

Import Az modules:

  - **Az.Account**

  - **Az.Compute**

  - **Az.Automation**

  - **Az.Resources**


## **Import SAP PowerShell Module**

Import **SAPAzurePowerShellModules** PowerShell module that will be used by SAP Runbooks.

> [!NOTE]
> PowerShell module **SAPAzurePowerShellModules** is stored in PowerShell Gallery and is easy to import into Azure automation account.


## **Import SAP Runbooks**

Navigate to **Runbook** and click **Import a runbook**.

Import these runbooks:

  - Stop-SAPSystem

  - Start-SAPSystem

  - List-SAPSystemInstances

  - Stop-SAPHANA

  - Start-SAPHANA

  - List-SAPHANAInstance

  - Start-SAPApplicationServer

  - Stop-SAPApplicationServer

  - Tag-SAPSystemASCSInstanceLinux

  - Tag-SAPSystemDialogInstanceLinux

  - Tag-SAPSystemStandaloneHANA

> [!NOTE] 
> All SAP runbooks are stored in PowerShell Gallery and are easy to import into Azure automation account.


## Tagging and executing Runbooks

**Example Distributed SAP ABAP System with HANA – ALL Linux**

Here is an example of a distributed SAP ABAP System **TS1** with HANA
DB. ALL VMs are Linux VMs. SAP HANA SID **TS2** is different than SAP
SID **TS1**.

![](Pictures/media/image53.png)

**HANA DB VM**

DB has following properties.

<table>
<thead>
<tr class="header">
<th><strong>Properties</strong></th>
<th><strong>Value</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>VM Name</td>
<td>ts2-db</td>
</tr>
<tr class="even">
<td>Azure Resource Group Name</td>
<td>gor-linux-eastus2</td>
</tr>
<tr class="odd">
<td>SAP System SID</td>
<td>TS1</td>
</tr>
<tr class="even">
<td><p>SAP HANA SID *</p>
<p>* in central SAP system HANA SID is can be different than SAP System SID</p></td>
<td>TS2</td>
</tr>
<tr class="odd">
<td>SAP HANA Instance Nr</td>
<td>0</td>
</tr>
</tbody>
</table>

VM \[ts2-db\] has to be tagged with following Tags:

| Tag                   | Value |
| --------------------- | ----- |
| SAPSystemSID          | TS1   |
| SAPHANASID            | TS2   |
| SAPDBMSType           | HANA  |
| SAPHANAINstanceNumber | 0     |

You can create these Tags manually. However, to simplify and automate
Tagging process run this Azure runbook: **Tag-SAPSystemStandaloneHANA**

| Parameter             | Mandatory | Value             |
| --------------------- | --------- | ----------------- |
| ResourceGroupName     | YES       | gor-linux-eastus2 |
| VMName                | YES       | ts2-db            |
| SAPSID                | YES       | TS1               |
| SAPHANASID            | YES       | TS2               |
| SAPHANAINstanceNumber | YES       | 0                 |

**ASCS VM**

ASCS has following properties.

| **Properties**            | **Value**         |
| ------------------------- | ----------------- |
| Azure Resource Group Name | gor-linux-eastus2 |
| VM Name                   | ts2-ascs          |
| SAP System SID            | TS1               |
| SAP ASCS Instance Nr      | 0                 |

VM \[ts2-ascs\] has to be tagged with following Tags:

| Tag                          | Value     |
| ---------------------------- | --------- |
| SAPSystemSID                 | TS1       |
| SAPApplicationInstanceType   | SAP\_ASCS |
| SAPApplicationInstanceNumber | 1         |

You can create these Tags manually. However, to simplify and automate
Tagging process run this Azure runbook:
**Tag-SAPSystemASCSInstanceLinux**

| Parameter             | Mandatory | Value             |
| --------------------- | --------- | ----------------- |
| ResourceGroupName     | YES       | gor-linux-eastus2 |
| VMName                | YES       | ts2-ascs          |
| SAPSID                | YES       | TS1               |
| SAPASCSInstanceNumber | YES       | 1                 |

**SAP Application Server VM #1 **

SAP application server 1 has following properties.

| **Properties**            | **Value**         |
| ------------------------- | ----------------- |
| Azure Resource Group Name | gor-linux-eastus2 |
| VM Name                   | ts2-di0           |
| SAP System SID            | TS1               |
| SAP Dialog Instance Nr    | 1                 |

VM \[ts2-di0\] has to be tagged with following Tags:

| Tag                          | Value  |
| ---------------------------- | ------ |
| SAPSystemSID                 | TS1    |
| SAPApplicationInstanceType   | SAP\_D |
| SAPApplicationInstanceNumber | 1      |

You can create these Tags manually. However, to simplify and automate
Tagging process run this Azure runbook: Tag-SAPSystemDialogInstanceLinux

| Parameter               | Mandatory | Value             |
| ----------------------- | --------- | ----------------- |
| ResourceGroupName       | YES       | gor-linux-eastus2 |
| VMName                  | YES       | ts2-ascs          |
| SAPSID                  | YES       | TS1               |
| SAPDialogInstanceNumber | YES       | 1                 |

**SAP Application Server VM #2**

SAP application server 2 has following properties.

| **Properties**            | **Value**           |
| ------------------------- | ------------------- |
| Azure Resource Group Name | gor-linux-eastus2-2 |
| VM Name                   | ts2-di1             |
| SAP System SID            | TS1                 |
| SAP Dialog Instance Nr    | 2                   |

VM \[ts2-di0\] has to be tagged with following Tags:

| Tag                          | Value  |
| ---------------------------- | ------ |
| SAPSystemSID                 | TS1    |
| SAPApplicationInstanceType   | SAP\_D |
| SAPApplicationInstanceNumber | 2      |

**You can create these Tags manually. However, to simplify, avoid typos, and automate the tagging process use Azure runbook**: Tag-SAPSystemDialogInstanceLinux

| Parameter               | Mandatory | Value               |
| ----------------------- | --------- | ------------------- |
| ResourceGroupName       | YES       | gor-linux-eastus2-2 |
| VMName                  | YES       | ts2-di1             |
| SAPSID                  | YES       | TS1                 |
| SAPDialogInstanceNumber | YES       | 2                   |


##  Runbook Description

**Listing SAP System \<SID\> VMs: List-SAPSystemInstance**s

| Parameter | Mandatory | Default value | Comment |
| --------- | --------- | ------------- | ------- |
| SAPSID    | YES       |               |         |

**Listing SAP HANA VM: List-SAPHANAInstance**

| Parameter | Mandatory | Default value | Comment |
| --------- | --------- | ------------- | ------- |
| SAPSID    | YES       |               |         |

**Start SAP System: Start-SAPSystem**

| Parameter                 | Mandatory | Default value | Comment                                                |
| ------------------------- | --------- | ------------- | ------------------------------------------------------ |
| SAPSID                    | YES       |               |                                                        |
| WaitForStartTimeInSeconds | No        | 600           | Wait time to start SAP system                          |
| ConvertDisksToPremium     | No        | $False        | If set to $True, all disk will be converted to Premium |
| PrintExecutionCommand     | No        | $False        | If set to $True, all OS commands will be printed       |

**Runtime steps:**

  - Convert disk to Premium, if desired  
    ConvertDisksToPremium = $True

  - Start VMs in this order:
    
      - SAP ASCS or DVEBMS VM
    
      - SAP DBMS VM
    
      - SAP Dialog instances VMs

  - Show SAP DBMS Status

  - Start SAP DBMS

  - Show SAP DBMS Status

  - List SAP ABAP instances and show status.

<!-- end list -->

  - Start SAP ABAP system and wait for WaitForStartTimeInSeconds
    seconds.

  - List SAP ABAP instances and show status.

  - Show summary.

**Stop SAP System: Stop-SAPSystem**

| Parameter                 | Mandatory | Default value | Comment                                                 |
| ------------------------- | --------- | ------------- | ------------------------------------------------------- |
| SAPSID                    | YES       |               |                                                         |
| SoftShutdownTimeInSeconds | No        | 600           | Soft Shutdown time for SAP system                       |
| ConvertDisksToStandard    | No        | $False        | If set to $True, all disk will be converted to standard |
| PrintExecutionCommand     | No        | $False        | If set to $True, all OS commands will be printed        |

Runtime steps:

  - List SAP ABAP instances and show status.

  - Stop SAP ABAP System with soft shutdown SoftShutdownTimeInSeconds

  - List SAP ABAP instances and show status.

  - Get SAP DBMS status

  - Stop SAP DBMS

  - Get DBMS status

  - Stop VMs in this order:
    
      - SAP Dialog instances VMs
    
      - SAP DBMS VM
    
      - SAP ASCS or DVEBMS VM

  - Convert disk to Standard, if desired  
    ConvertDisksToStandard = $True

<!-- end list -->

  - Show summary.

**Start-SAPHANADB**

This runbook will only start VM and standalone HANA DB.

| **Parameter**         | **Mandatory** | **Default value** |
| --------------------- | ------------- | ----------------- |
| SAPHANASID            | YES           |                   |
| ConvertDisksToPremium | No            | $False            |
| PrintExecutionCommand | No            | $False            |

Runtime steps are:

  - Convert disk to Premium, if desired  
    ConvertDisksToPremium = $True

  - Start VM.

  - Show SAP HANA Status

  - Start SAP HANA

  - Show SAP HANA Status

  - Show summary.

**Stop-SAPHANADB**

This runbook will stop standalone HANA DB, and VM.

| **Parameter**          | **Mandatory** | **Default value** |
| ---------------------- | ------------- | ----------------- |
| SAPHANASID             | YES           |                   |
| ConvertDisksToStandard | No            | $False            |
| PrintExecutionCommand  | No            | $False            |

Runtime steps:

  - Show SAP HANA Instance status

  - Stop SAP HANA

  - Show SAP HANA status

  - Stop VM.

  - Convert disk to Standard, if desired  
    ConvertDisksToStandard = $True

  - Show summary.


**Stop- SAPApplicationServer**

This runbook will stop standalone SAP Application Server and VM.

| **Parameter**                | **Mandatory** | **Default value** |
| ---------------------------- | ------------- | ----------------- |
| ResourceGroupName            | YES           |                   |
| VMName                       | YES           |                   |
| SAPApplicationServerWaitTime | No            | 300               |
| ConvertDisksToStandard       | No            | $False            |
| PrintExecutionCommand        | No            | $False            |

Runtime steps:

  - Stop SAP application server.

  - Stop VM.

  - Convert disk to Standard, if desired  
    ConvertDisksToStandard = $True

  - Show summary.

**Start- SAPApplicationServer**

This runbook will start VM and standalone SAP Application Server.

| **Parameter**                | **Mandatory** | **Default value** |
| ---------------------------- | ------------- | ----------------- |
| ResourceGroupName            | YES           |                   |
| VMName                       | YES           |                   |
| SAPApplicationServerWaitTime | No            | 300               |
| ConvertDisksToPremium        | No            | $False            |
| PrintExecutionCommand        | No            | $False            |

Runtime steps:

  - Convert disk to Premium, if desired  
    ConvertDisksToPremium = $True

  - Start VM.

  - Start SAP application server.

  - Show summary.


# Access consideration for Azure Automation Runbooks and Jobs

Utilizing the SAP start/stop automation you can also limit the rights on
Azure resources such as VMs while at same time do a finer grained
segregation of access to individual Automation runbooks and jobs.

The deployed Azure Automation account inherits role assignments from the
resource group and/or subscription/management group. Utilizing the
principle of least privileges, you might or might not already have
rights to start/stop Automation runbooks and view runbook jobs outputs.

The following is a list of prebuild roles for Automation accounts, as
applicable in the context of this document. Full list of roles for
Automation can be seen here in documentation.

| **Role**                    | **Description**                                                                                                                                                                                                                                                                                                                                                               |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Owner                       | The Owner role allows access to all resources and actions within an Automation account including providing access to other users, groups, and applications to manage the Automation account.                                                                                                                                                                                  |
| Contributor                 | The Contributor role allows you to manage everything except modifying other user’s access permissions to an Automation account.                                                                                                                                                                                                                                               |
| Reader                      | The Reader role allows you to view all the resources in an Automation account but cannot make any changes.                                                                                                                                                                                                                                                                    |
| User Access Administrator   | The User Access Administrator role allows you to manage user access to Azure Automation accounts.                                                                                                                                                                                                                                                                             |
| Automation Operator         | The Automation Operator role allows you to view runbook name and properties and to create and manage jobs for all runbooks in an Automation account. This role is helpful if you want to protect your Automation account resources like credentials assets and runbooks from being viewed or modified but still allow members of your organization to execute these runbooks. |
| Automation Job Operator     | The Automation Job Operator role allows you to create and manage jobs for all runbooks in an Automation account.                                                                                                                                                                                                                                                              |
| Automation Runbook Operator | The Automation Runbook Operator role allows you to view a runbook’s name and properties.                                                                                                                                                                                                                                                                                      |

In other words, to grant access to a group to manage/create and operate
Automation runbooks and jobs, on the entire SAP start/stop automation
account, use the Automation Operator role.

Open the Automation Account in Azure Portal and navigate to Access
control (IAM).  
On tab Role assignments you can see currently assigned groups to this
resource.

## Limiting access to individual runbooks/jobs

The above section provides access to the entire Azure Automation
account.  
If however, you want to provide access to individual runbooks or jobs,
this can NOT be accomplished using Azure Portal currently.

As a solution however you can utilize below PowerShell to grant access
to individual sub-resources inside the Automation Account. This is
shorted from the public documentation here.

$rgName = "\<Resource Group Name\>" \# Resource Group name for the
Automation Account  
$automationAccountName ="\<Automation account name\>" \# Name of the
Automation Account  
$rbName = "\<Name of Runbook\>" \# Name of the runbook  
$userId = Get-AzADUser -ObjectId "\<your email\>" \# Azure Active
Directory (AAD) user's ObjectId from the directory

\# Get the Automation account resource ID  
$aa = Get-AzResource -ResourceGroupName $rgName -ResourceType
"Microsoft.Automation/automationAccounts" -ResourceName
$automationAccountName

\# Get the Runbook resource ID  
$rb = Get-AzResource -ResourceGroupName $rgName -ResourceType
"Microsoft.Automation/automationAccounts/runbooks" -ResourceName
"$rbName"

\# The Automation Job Operator role only needs to be run once per user  
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation
Job Operator" -Scope $aa.ResourceId

\# Adds the user to the Automation Runbook Operator role to the Runbook
scope  
New-AzRoleAssignment -ObjectId $userId.Id -RoleDefinitionName
"Automation Runbook Operator" -Scope $rb.ResourceId

A user with just individual runbooks granted runbook operator role, will
see only these runbook(s) in the resource group and can start jobs as
otherwise documented earlier.  
Granting access to ONLY the runbooks and jobs shows under ‘all
resources’ in Azure Portal, it will NOT show up in Azure Automation
Accounts because access it granted ONLY to the runbooks and jobs within
themselves.

Below an example of exactly this – only runbooks granted access to a
user and the actual Azure Automation Account

![](Pictures/media/image75.png)
