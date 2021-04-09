# Introduction

how you doing?
NEw changes made

# Challenge #1:  
# Challenge #2: 
# Challenge #3: 
# Challenge #4: ANF

[ANF documentation](https://github.com/Microsoft-SAPonAzure-OpenHack/Learning-the-OpenHack-Way/tree/main/04-ANF)

# Challenge #5: 
[PowerApps documentation](https://github.com/Microsoft-SAPonAzure-OpenHack/Learning-the-OpenHack-Way/blob/main/05-PowerApps)


# Challenge #6: Application aware maintenance (Start/Stop/Scale)

The goal of this solution is to facilitate a controlled shutdown & startup of SAP systems, which is a common mechanism to save costs for non Reserved Instances (RI) VMs in Azure.

Note: VMs must be deallocated for Azure charges to stop and that's facilitated by scripts

This is ready, flexible, end-to-end solution (including Azure automation runtime environment, scripts, and runbooks, tagging process etc.) that enables:

- Start & Stop of your SAP application servers, central services, database, and corrosponding VMs
- Optionally convert Premium Managed Disks to Standard during the stop procedure, and back to Premium during the start procedure, thus saving cost storage as well  

SAP systems start and stop is done gracefully (using SAP native commands), allowing SAP users and batch jobs to finish (with timeout) minimizing downtime impact.  

Go to the following link to get started with participant guide: [Start-Stop Automation documentation](https://github.com/Microsoft-SAPonAzure-OpenHack/SAPOH/blob/main/06-Start-Stop-Automation)

# Challenge #7: 
