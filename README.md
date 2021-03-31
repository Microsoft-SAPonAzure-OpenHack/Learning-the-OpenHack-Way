# Introduction

how you doing?


# Challenge #1:  
# Challenge #2: 
# Challenge #3: 
# Challenge #4: ANF

[ANF documentation](https://github.com/Microsoft-SAPonAzure-OpenHack/Learning-the-OpenHack-Way/tree/main/04-ANF)

# Challenge #5: 

# Challenge #6: SAP & VM Start / Stop Automation

The goal of this solution is to facilitate a controlled shutdown & startup of SAP systems, which is a common mechanism to save costs for non Reserved Instances (RI) VMs in Azure.

Note: VMs must be deallocated for Azure charges to stop and that's facilitated by scripts

This is ready, flexible, end-to-end solution (including PaaS Azure automation runtime environment, scripts, and runbooks, tagging process etc.) that enables you automatically.

- Start / Stop of your SAP systems DBMS, and VMs:
- SAP application servers
- If you use managed disks (Premium and Standard), you can decide to convert them to Standard during the stop procedure, and to Premium during the start procedure.
In this way, cost saving is achieved both on the compute as well as on the storage side!

SAP systems stop and SAP application servers stop is specially cared for in a graceful way, allowing SAP users and batch jobs to finish. In this way you can minimize SAP system or SAP application server’s downtime impact. Similar is done on DBMS side.

To further enhance user experience, you can consume this functionality using a SAP Azure Power App. For more information you can check a great blog - Hey, SAP Systems! My PowerApp says Snooze! But only if you’re done yet.

This solution is product of joint work of SAP on Azure CAT Team (Cloud Advisory Team) and SAP on Azure FastTrack Team.

[Start-Stop Automation documentation](https://github.com/Microsoft-SAPonAzure-OpenHack/SAPOH/blob/main/06-Start-Stop-Automation)

# Challenge #7: 
