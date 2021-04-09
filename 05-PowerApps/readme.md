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

Part 2:  Get Material Details

Create Flow “ Get_SAP_Material  “
This flow has 3 steps and uses SAP BAPI named "BAPI_MTERIAL_GET_DETAIL". 

![image](https://user-images.githubusercontent.com/45843990/114221959-d36ce900-9922-11eb-911c-7ee63ff0c940.png)

![image](https://user-images.githubusercontent.com/45843990/114221974-d8319d00-9922-11eb-8ec8-99cd79785f39.png)

![image](https://user-images.githubusercontent.com/45843990/114221994-dd8ee780-9922-11eb-9b32-49201a737f89.png)

Follow similar steps from previous exercise. ![image](https://user-images.githubusercontent.com/45843990/114222005-e1bb0500-9922-11eb-8c9d-72a62bb1d3e0.png)

Exercise 3:  

In this exercise, you are going to create Power Application with Layout feature. 

Create Power Application with tablet layout. 
	• Create a screen with appropriate header. 

![image](https://user-images.githubusercontent.com/45843990/114222068-f8615c00-9922-11eb-9057-36fbb7921826.png)

Select onvisible property in the form and connect to powerautomate flow "Get_SAP_MaterialList" .  For this go to Action -> click on power automate select the flow. 

![image](https://user-images.githubusercontent.com/45843990/114222120-057e4b00-9923-11eb-85e5-b3af73d6049c.png)

Adjust the onvisible property of screen as shown in below screen. Below Screen shot  has different flow name, please ignore it and use correct flow name. Screenshot is given to show the property "OnVisible" and collection object "QueryResults". 

![image](https://user-images.githubusercontent.com/45843990/114222164-1333d080-9923-11eb-9f1f-d0c57a46edd0.png)

	• Insert vertical gallery.  
Connect the vertical gallery to collection object "QueryResults" which was pre-loaded while form is being loaded.  

![image](https://user-images.githubusercontent.com/45843990/114222209-1fb82900-9923-11eb-951e-30dd7cfa2238.png)


Reference documents:
• SAP ERP Connector in power Apps https://powerapps.microsoft.com/en-us/blog/introducing-the-sap-erp-connector/
• Power Apps Canvas Apps: https://docs.microsoft.com/en-us/powerapps/maker/canvas-apps/
• Create collection: https://docs.microsoft.com/en-us/powerapps/maker/canvas-apps/create-update-collection
• Using Flows in PowerApps: https://docs.microsoft.com/en-us/powerapps/maker/canvas-apps/using-logic-flows
![image](https://user-images.githubusercontent.com/45843990/114222233-26df3700-9923-11eb-93c1-98f493f9d6f5.png)











