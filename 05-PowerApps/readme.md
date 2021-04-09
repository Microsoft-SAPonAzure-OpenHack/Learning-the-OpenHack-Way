Mango Inc warehouse employees should able to identify available materials along with base unit of measurement in real time for available materials in their base plant from their existing mobile device. Design a power application that can be accessible from tablet to search materials in employees own sales organization and plant. This application design consists of building required infrastructure and power application. High level architecture looks like:

![image](https://user-images.githubusercontent.com/45843990/114218203-f9dc5580-991d-11eb-8a75-653fbf7ecc15.png)

Documentation URL: https://powerapps.microsoft.com/en-us/blog/introducing-the-sap-erp-connector/

Exercise  1:  
Verify access to SAP S/4 appliance and Deploy on-prem data gateway. 
Deploy On-premises data gateway VM and SAP .Net Connector closer to SAP Application servers for low latency data traffic. Ensure that NSG allows flow such as SAP Gateway, Dispatcher and Message server ports
 
Exercise  2:  
In this exercise,  you are going to bring SAP Material Information and material details to the PowerApps by using Flow. 
Part 1: Get Material List
Verify access to PowerApps application layer ( https://make.preview.powerapps.com/ ). You do need  right set of licenses such as “Microsoft 365 E5 Developer, Power Apps Plan 2 Trial and Power Automate Free”.

Create SAP Material List flow by calling SAP function module BAPI_MATERIAL_GETLIST by leveraging "Call SAP Function”. 

Use following parameter values such as

![image](https://user-images.githubusercontent.com/45843990/114218407-390aa680-991e-11eb-880e-72987e5b7ef3.png)

![image](https://user-images.githubusercontent.com/45843990/114218429-3d36c400-991e-11eb-9b1b-9f6419877543.png)

![image](https://user-images.githubusercontent.com/45843990/114218442-41fb7800-991e-11eb-8771-1d6cbc7d201b.png)

Next Step is to build the “Response”.

In the response, select MATNRLIST" Using the connector in an App" at https://powerapps.microsoft.com/en-us/blog/introducing-the-sap-erp-connector/.  

Go to outputs section in step 2, go to MATNRLIST section and copy all the data in that section. Now click on advanced options in step 3(Response) -> Click on use sample payload to generate schema -> Paste the copied data.  


