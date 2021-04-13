# Table of Contents 

- [Finalize the description for challenge](#Finalize-the-description-for-challenge)
- [Understand required Architecture and Azure components](#Understand-required-Architecture-and-Azure-components) 
- [Get equipped with necessary skills and Knowledge](#Get-equipped-with-necessary-skills-and-Knowledge)
- [Final Architecture](#Final-Architecture)


**Description:**

 

Generate self-service sales report from SAP system. Option 1: Extract sales data from SAP system to ADLS and generate analytics  report using power BI /Synapse. Option 2:  Generate self-service sales report using direct query to SAP  HANA system. 

 

# Finalize the description for challenge

 

   Generate self-service SAP system  report about batch jobs status and  name from SAP system. 

 

   * Download Power BI client and connect SAP HANA DB using Power BI configuration option. 

   * Extract  table TBTCO ( SAP job status ) using SQL and   Generate self-service Job status report  using direct custom query to SAP system. 

 

 

# Understand required Architecture, Azure components / Services

 

   * Download Power  BI  desktop from windows power app platfrom  

   * Execute the install and make sure that you have network connectivity from Jump box to SAP HANA DB and required HANA DB ports are open. ( no firewall issues etc..) 

   * For the HANA DB create the SCHEMA user with admin rights on SAP SCHEMA and assign appropriate roles and privileges to extract the data. 

   * Open the BI desktop client and execute the connection configure for SAP HANA database. 

   * Enter the Server IP address , select ports  (custom ) and port  for the  HANA system ID  ( for example SAP HANA system ID NN ports will be 3NN40-3NN99) 

   * Select Appropriate user name,  password and SSL encryption - none 

   * Select direct query for data connectivity Mode  

   * Extract the data and see the  tabular from of the DB 

 

 # Get equipped with necessary skills / Knowledge 

 

   * Target Roles: DBA, BASIS and Technical Architect 

   * SAP HANA database administrator  

   * Report creation and selection for data analytics 

   * HANA DB skills – how to create Schema user and assign roles and privileges 

   * Other tools - perf. benchmark tools, snapshot validation tools, Portal, CLI, bash scripting, Linux Administration 

 

 

 

# Final Architecture 

 
![](media/GetImage.png)
 

   
5. **COMPLETE - Due Jan 30th - Build prototype and testing**

 

   * Building the prototype and respective testing: 

   * Base Infra to support the challenge - VMs, ANF volumes, scale-out HANA 

   * Download power BI desktop and configure 

   * Create HANA DB schema and user , assign appropriate roles and privileges  to user to extract the data 

   * Extract the data and start working filtering and value selection 

   * Generate the Chart  

   * Publish the Chart 

6. **IN PROGRESS BUT AT RISK OF DELAY - Document the challenge and the step by step process**

 

   * Document the Challenge - Participant Guide (by 2/12) 

   * Participant Documentation ( in progress)  

   * Document the Challenge -  ( in progress )  

7. **PLANNED -Test documented process**

 
8. **PLANNED -Internal dry run with in Team and identify any gaps**

9. **PLANNED -Update documentation based on leanings**
10. **PLANNED -Dry run with CSA community**

11. **PLANNED -If there are any lessons, update documentation**

12. **PLANNED -Ready to conduct OpenHack at the customer**

13. **Other**

