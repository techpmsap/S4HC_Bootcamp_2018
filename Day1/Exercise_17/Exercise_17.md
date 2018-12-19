<table width=100% border=>
<tr><td colspan=2><h1>EXERCISE 1_7 - CUSTOM BUSINESS OBJECT EXPOSURE AS EXTERNAL WEB SERVICE</h1></td></tr>
<tr><td><h3>SAP S/4HANA Cloud Bootcamp</h3></td><td><h1><img src="images/clock.png"> &nbsp;50 min</h1></td></tr>
</table>

## Work in Process for update

## Description
In this exercise, you’ll learn how 

* to expose the custom business object as web service for integration of your solution with other systems.


For further reading on S/4HANA cloud in-app extension, click link below.
<https://jam4.sapjam.com/groups/m8lprEZwfU3zPoX0myj1Xu/overview_page/RfBJ6ix9q00bbSseaxm4zW>


## Target group

* Developers
* People interested in learning about S/4HANA Cloud extension and SDK  


## Goal

The goal of this exercise is to expose the custom business object as web service for integration of your solution with other systems.


## Prerequisites
  
Below are the prerequisites for this exercise.

* Complete previous exercises.
* Google Chrome: Please complete this exercise using the Google Chrome browser
* **Authorizations:** Your user needs a business role with business catalog **Extensibility** (ID: `SAP_CORE_BC_EXT`)
* To be able to use the Communication Management Applications, your user needs Business Catalog (ID: `SAP_CORE_BC_COM`)
* Having installed standalone **POSTMAN** application from <https://www.getpostman.com> and disabled “SSL certificate verification”
* (Optional) Having Microsoft Excel installed.

## Steps

1. [Creating a Custom Communication Scenario](#creating-a-custom-communication-scenario)
1. [Creating a Communication System and User](#creating-a-communication-system-and-user)
1. [Creating a Communication Arrangement](#creating-a-communication-arrangement)
1. [Testing and Calling the Service](#testing-and-calling-the-service)
1. [Consumption from SAP Cloud Platform](#consumption-from-SAP-Cloud-Platform)


### <a name="creating-a-custom-communication-scenario"></a> Creating a Custom Communication Scenario

A communication scenario is the basis definition for a communication between systems. It defines a solution to be made available for external systems.

1. Open Custom Communication Scenario Application

	![](images/01.png) 
1. Start creating a new scenario by executing the “New” action.

	![](images/02.png) 
1. Give following data to the new custom communication scenario ...


	| Field Caption | Field Value |
	|------------|-------------|
	| ID| BonusplanXX |
	| Description | Bonus PlanXX |
	Note: XX is the number assigned to you for the exercise.

	![](images/03.png)  
	
	... and create the scenario by executing the “New” action.
	
1. Start adding an inbound communication service by executing the “+” action

	![](images/04.png) 
1. Search for `BONUSPLANXX`. Choose service `YY1_BONUSPLANXX_CDS` and execute the “OK” action. 

	![](images/05.png)
	
	**Background**: That service had been created during Custom Business Object “Bonus Plan” publishing as in its definition the OData Service Generation flag was set for UI creation already.
	![](images/06.png)
	
1. Publish the Custom Communication Scenario.

	![](images/07.png)
	![](images/08.png)

### <a name="creating-a-communication-system-and-user"></a> Creating a Communication System and User

To enable secure communication between different systems you have to register these systems and define the user which is authorized to use the connection.

The communication system represents the communication partner within a communication. For inbound communication, this is the external system that calls our Bonus Plan service.

We’ll create one Communication System for all systems that want to use our service as well as the user that they’ll have to use.

1. Open “Communication Systems” Application.

	![](images/09.png)
1. Start creating a new system by executing the “New” action.

	![4](images/10.png)
1. Give following data to the new custom communication scenario …

	| Field Caption | Field Value |
	|------------|-------------|
	| System ID | EXTERNAL_SYSTEMXX |
	| System Name | EXTERNAL_SYSTEMXX |
	Note: XX is the number assigned to you for the exercise. 

	![](images/11.png) 
	… and create the system by executing the “Create” action.
	
1. In the Opening Details view, fill the "Host Name" and "Business System" with “EXTERNAL_SYSTEMXX” as well. 

	![](images/12.png) 
1. Scroll down to “User for Inbound Communication” and start adding one by executing the “Add” action.

	![](images/13.png)
1. In the opening Pop Up, start browsing for the user   

	![](images/14.png)
1. Choose the user you already created in the first exercise
	![](images/15.png) 

1. Click on **OK**  
	![](images/16.png) 

1. Your user for Inbound Communication is set up. Click on **Save**
	![](images/18.png)

1. This will switch you back to the Pop Up, where the User Name is filled now and you can confirm to add the Inbound Communication User with action “OK”.
The just created user will be needed by callers to make use of the services. 

	
### <a name="creating-a-communication-arrangement"></a> Creating a Communication Arrangement

Finally, a communication arrangement links the solution’s scenario with the Communication system and its user and exposes the Service to be used.

1. Open “Communication Arrangements” Application

	![](images/19.png)
1. Start creation by executing the “New” action. 

	![](images/20.png)
1. A pop up opens in that you use the value help for Scenario first.

	![](images/21.png)
1. Select the YY1_BONUSPLANXX Scenario.

	![](images/22.png)
1. This will set the Scenario and default the Arrangement Name, so that you can continue the creation via “Create” action. 

	![](images/23.png) 

1. In the opening Arrangement details click to browse the Communication System  
	![](images/24.png) 

1. Choose the **EXTERNAL_SYSTEMXX** you created before (where XX is your workstation ID)  
	![](images/25.png)

1. Choosing the system will automatically set the related User as well. Click **Save** to finish creation  
	![](images/26.png)

1. Copy the **Service URL** for the next step
	![](images/27.png)




### <a name="testing-and-calling-the-service"></a> Testing and Calling the Service

POSTMAN application is there to test Web services by sending requests and receiving responses.
All included postman screenshots are reprinted with permission © Postdot Technologies Inc. All rights reserved.

1. Start the POSTMAN application

1. Open a new tab and do the following steps:
	1. select **GET** as request method
	2. enter the Service URL you copied in the previous chapter
	3. click on the **Authorization** tab
	4. choose **Basic Auth**
	5. enter the user you created in the first exercise
	6. click on **Send**
	7. look at the response from the server: it should be "200 OK"
	8. you get the response content from the server in XML format

	![](images/28.png)


#### Create a Bonus Plan instance ####

**Get X-CSRF-Token**

Whenever you want to do a change to the Custom Business Object’s persistence, for security reasons you need to send a **X-CSRF-Token** with that changes request. To get such a token, you have to send a get request first to fetch it.

1. Here is the list of steps you need to perform
	1. On the same request, click on the **Headers** tab
	2. Add a new key named **X-CSRF-Token** with the value **fetch**
	3. Click **Send**
	4. Copy the x-csrf-token value

	![](images/29.png) 
	
	The fetched token will be valid for **30** minutes and fetching a token will return the same as long as validity has not ended yet.

**Create Instance**

1. To create a new bonus plan via the service, do the following
	1. Open a new request tab
	2. Choose **POST** as request method
	3. Enter the same URL you used for the **GET** request and append the Entity Set name **/YY1_BONUSPLANXX** (where XX is your workstation ID)
	4. Click on **Headers** 
	5. Paste the previous fetched X-CSRF-Token as value to the corresponding header key, add new header key **Accept** with value **application/json** (this will ensure that the response will be in JSON format, which is easier to work with than XML), add new header key **Content-Type** with value `application/json`

	![](images/30.png)
	
1. Now we need to fill the body of the request
	1. Switch to the Body tab
	2. Set the body type to **raw**
	3. Set the editor type **JSON (application/json)**, which will enable correct syntax highlighting and header key for Content-Type
	4. Enter the initial data for the to be created bonus plan in JSON format to the editor
	
		```
		{
	        "ValidityStartDate": "/Date(1514764800000)/",
	        "ValidityEndDate": "/Date(1546214400000)/",
	        "LowBonusPercentage_V": "10.000",
	        "HighBonusPercentage_V": "20.000",
	        "EmployeeID": "<any>",
	        "TargetAmount_V": "1000.00",
	        "TargetAmount_C": "EUR",
	        "LowBonusAssignmentFactor": "1.00",
	        "HighBonusAssignmentFactor": "3.00"
		}
		```
	
		**ValidityStartDate** "/Date(1514764800000)/" is the json format for 01/01/2018. 
		
		**ValidityEndDate** "/Date(1546214400000)/" is the json format for 12/31/2018. 
		
		**EmployeeID** "<any>" shall be the one of a sales person that created sales orders with a Net Amount of more than 3000.00 EUR in 2018 and that are completed. In this exercise, you can use "CB9980000008" or your own ID.
	
	5. **Send** the request
	6. You will get **201 Created** status
	7. A new instance has been created. Copy the **SAP_UUID** you can find in the response

	![](images/31.png) 

	
#### Update a Bonus Plan instance ####

1. Right click on the **POST** request tab and choose **Duplicate Tab**
	![](images/32.png) 

1. To update the Bonus Plan instance do the following
	1. Switch the request method to **PATCH**
	2. Enter the request URL for a bonus plan entity. And add the previous **guid** to the end following this syntax `(guid'<guid_value>’)`, for example `/YY1_BONUSPLANXX(guid’8cdcd4a8-05c0-1ed7-8ec1-84ed870dedb5′)`
	3. Click on **Body**	
	4. Enter following JSON to the body

		```
		{
	    "ValidityStartDate": "/Date(1514764800000)/",
	    "ValidityEndDate": "/Date(1546214400000)/"
		}
		```
	5. **Send** the request
	6. You will see that the update worked as you get the response from the server "204 No Content"

	![](images/33.png) 

#### Using the service (Optional) ####
Note 1 : You can skip this exercise if you have MacBook. 

Note 2: This exercise only works for Microsoft Office 365.  If you have older version of the Microsoft Office, it does not work.

1. Open Microsoft Excel with a new “Book” (= excel file)
1. Go to “Data” tab > “New Query” > “From Other Sources” > “From OData Feed”

	![](images/33a.png)
1. A pop up opens in that you enter the service URL and execute “OK”.

	![](images/33b.png)
1. In the next Pop Up for Access to the Service (= OData feed) switch on the left side from “Anonymous” to “Basic”, then enter Username “dummy” and its Password on the right side, before executing the “Connect” action.

	![](images/33c.png)
1. In the next screen select the item YY1_BONUSPLANXX and execute “Load”. 

	![](images/33d.png)
1. As result the excel workbook will be filled with the current Bonus Plan entities’ data. 

	![](images/33e.png)
	
### <a name="consumption-from-SAP-Cloud-Platform"></a> Consumption from SAP Cloud Platform

**Use SAP Cloud Platform trial account**

You can use a service in SAP Cloud Platform in many different way. You can use it to build a new UI, you can call it in your Java or JavaScript coding, you can use it in Cloud Integration iFlows. Here I want to show only one simple use case on how to consume it: SAP Web IDE.

1.	Access SAP Cloud Platform trial account from <https://account.hanatrial.ondemand.com/> by clicking on the **Log On** button. After you have logged in, click on **Neo Trial**. 
	![](images/34.png)

1. This will take you to the **Overview** page of the SAP Cloud Platform Cockpit for the Neo stack. Click on Services, search for "web" and click on the SAP Web IDE Full-Stack service
	![](images/35.png)

1.	Go to SAP Web IDE Service.
	![](images/36.png)
	
1.	From the **Welcome** page select **New Project from Template**.
	![](images/37.png)
	
1.	From the **Featured** category, select **SAP Fiori Worklist Application** and click **Next**
	![](images/38.png)
	
1.	Enter the basic information for the project.
	
	| Field | Field Value | 
	|------------|-------------|
	| Project Name | bonusplanXX | 
	| Title | Bonus Plan XX | 
	| Namespace | com.sap.bonus | 
	
	Click **Next**.

	![](images/39.png)
	
1.	On the step Data Connection, select **Service URL**. Select the destination you created before (ErpQueryEndpoint) and the relative path of the OData service, as given in the Communication Arrangement screen (`/sap/opu/odata/sap/YY1_BONUSPLANXX_CDS`). You can now discover the details of the service and you can even discover the live data. Click on **Test**. Click on **Next**.
	![](images/40.png) 
	
1. Continue with the creation wizard to create a Fiori app out of the OData service. See below screen as an example. Click on **Finish**.
	![](images/41.png) 
		
1. The project of "bonusplanXX" is created. Expand the project folder bonusplanXX > webapp. Preview the application by click on **Index.html** file and click on preview button.
	![](images/42.png)

1. Preview application will display on another tab. 

	![](images/43.png)
	

**Use Configured SAP Web IDE with S/4HANA Cloud**

The following are the steps for using the SAP Web IDE tile to develop the apps and deployment back to SAP S/4HANA Cloud. 

1. Click on **SAP Web IDE** tile
	![](images/44.png)
	
1. It will launch a new tab from the browser to access the "SAP Web IDE Full-Stack" tool from the SAP Cloud Platform whcih has configured for the extension of this S/4HANA Cloud system. Click on **Welcome** icon on the left panel to show the Welcome page.  Click on **New Project from Template** to create a new project.
	![](images/45.png) 

1. Select **List Report Application** template and click on **Next**.
	![](images/46.png)

1. Put in `BonusplanXXII`, where **XX** is the number assigned to you and **II** is your initial, for the Project Name and Title. And click on **Next**.
	![](images/47.png) 

1. Select **S4HC - S4HC - OAuth** destination.  Search service for **plan50**.  Select `YY1_BONUSPLANXX_CDS`.  Click on **Next**.

	Note:  In this hands-on S4/HANA Cloud system, we have created the communication arrangement named `YY1_BONUSPLAN50IDE` to use the OAuth authentication for the service.  And add the `YY1_BONUSPLAN50_CDS_0001` scope into the destination "S4HC - S4HC - OAuth".  Please use `YY1_BONUSPLAN50_CDS` for practice.
	![](images/48.png) 

1. Select all annotation. Click on **Next**.
	![](images/49.png)

1. Select **YY1_BONUSPLAN50** for OData Collection.  Click on **Finish**. The project of will be created.

	![](images/50.png) 

1. Expand the project folder of Bonusplanxxii. Expand **webapp** folder. Double click on **manifest.json** file. Click on the **Navigation** tab. Click on **+** sign to add semantic object.

	![](images/51.png) 

1. Type in the following for the Sematic Object and Action.

	| Field | Value |
	|------------|-------------|
	| Semantic Object | `CustomUI` |
	| Action | `Bonusplan_UI` |

	Enter the following for Inbond Tile information:
	
	| Field | Value |
	|------------|-------------|
	| Tile | `Bonusplanxxii` |
	| Information | `Bonusplanxxii (info)` |
	| Subtitle | `Bonusplanxxii` |
	| Icon | `sap-icon://activate` |

	Save the file at the end  
	
	![](images/52.png) 
	
1. You can preview the outcome by right click on the project folder and select **SAPUI5 Visual Editor**  
	![](images/53.png) 

1. The preview will show. Click on **Go** to view the items
	![](images/54.png) 

1. Close the SAPUI5 Visual Editor and the manifest file

1. Now we can deploy the application back to S/4HANA Cloud system. Right click on the project folder,  select **Deploy > Deploy to SAPUI5 ABAP Repository**
	![](images/55.png)
	
1. Select **S4HC_OAuth - S4HC - OAuth** system and the option of **Deploy a new application**.  Click on **Next**
	![](images/56.png)

1. Enter the the Name and Description and click on **Next**.

	| Field | Value | Description | 
	|------------|-------------|-------------|
	| Name | `zPlanXXII` |XX is the number assigned to you. II is your initial. | 
	| Description | `zBonusplanXXII` |   | 

	![](images/57.png) 
	
1. Click on **Fihish**. It takes some minutes for the deployment.
	![](images/58.png) 

1. Deployment completed successfully.
	![](images/59.png)

1. Go back to the Home page of the S/4HANA Cloud system. Click on the **Custom Catalog Extensions** tile
	![](images/60.png)

1. Search for the plan that you deployed from SAP Web IDE, search term **zBonus**. The Type for the app is "Custom UI App". Click on the app to go to the detail view
	![](images/61.png) 

1. Click on **Add**
	![](images/62.png) 
	
1. Search for **Extensibility**.  Select "Extensibility - Transport Management - Export" and "Extensibility - Transport Management - Import". Click on **OK**
	![](images/63.png)

1. Select both Business Catalogs and click on **Publish**
	![](images/64.png)
	
1. Confirm with **OK**
	![](images/65.png)

1. It takes sometime to complet the publication  
	![](images/66.png)

1. Go back to the Launchpad Home page. Refresh the browser. Check the result under the catagory of **Transport Management**. 
	![](images/67.png)

1. Click on the tile to view the detail.
	![](images/68.png)

	![](images/69.png)

## Summary
This concludes the exercise. 

You should have learned how to expose the custom business object as web service for integration of your solution with other systems. 


	
