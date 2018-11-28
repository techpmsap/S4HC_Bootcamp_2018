<table width=100% border=>
<tr><td colspan=2><img src="images/spacer.png"></td></tr>
<tr><td colspan=2><h1>EXERCISE 25 - Create a service and a front-end application using the Application Programming Model</h1></td></tr>
<tr><td><h3>ASUG PreConference 2018</h3></td><td><h1><img src="images/clock.png"> &nbsp;50 min</h1></td></tr>
</table>

## Description
In this exercise, you’ll learn how to 

* create a new CDS service exposing data from S/4HANA Cloud using the Business Application Programming model
* create, with SAP Web IDE Full-Stack, a frontend application which consumes data from the service.


## Target group

* Developers
* People interested in learning about Application Programming Model  


## Goal

The goal of this exercise is to create a frontend application for your service using the Application Programming Model template in SAP Web IDE Full-Stack.  

## Prerequisites
  
Here below are prerequisites for this exercise.

* Previous exercises completed

## Steps

1. [Create a new application with the Programming Model template](#new-application)
1. [Integrate an external service in the project](#external-service)
1. [Create the custom CRUD classes](#crud-classes)
1. [Build and deploy the project](#build-deploy)
1. [Add some annotations to the service](#add-annotations)
1. [Create the front end application, build and deploy again](#frontend-application)




### <a name="new-application"></a>Create a new application with the Programming Model template

In this chapter you are going to see how to use SAP Web IDE to create a new application with the Application Programming Model. 

1. Access the **SAP Web IDE** link you have bookmarked in the previous exercise <https://webidecp-ovb9vknezv.dispatcher.hana.ondemand.com/> using the credentials provided by your instructor  
	![](images/01.png)

1. SAP Web IDE opens up in the browser
	>NOTE: for SAP employees it's better to use Firefox, because it's easy to bypass the SAP SSO, since we will be using different credentials  
	
	![](images/02.png)

1. Click on the small gear on the left-hand sidebar, choose **Cloud Foundry** and click on the **API Endpoint** drop down list to choose your Cloud Foundry API Endpoint  
	![](images/03.png)

1. This is the one you wrote down in the previous exercise  
	![](images/04.png)

1. Enter the Cloud Foundry credentials if requested. They are provided by your instructor  
	![](images/05.png)

1. Select the **Organization** and **Space** reserved for you and click on the button **Install Builder**. This operation will install a special app, the "Cloud Foundry Builder", into your Cloud Foundry space: this app will take care of building the CDS and the .mtar files which will be deployed to the Cloud Foundry environment   
	![](images/06.png)

1. After some time the builder will be installed as a Cloud Foundry application and you will get the message that the builder is up to date. Click on **Save** and go to the **Development** tab in SAP Web IDE  
	![](images/07.png)

1. From the **View** menu click on the **Console** item to enable the console in SAP Web IDE. This will help you to easily debug and check the status of the processes  
	![](images/08.png)

1. The console is opened at the bottom of the screen  
	![](images/09.png)

1. Click on **File -> New -> Project from Template**  
	![](images/10.png)

1. Under the **Featured** category select the **SAP Cloud Platform Business Application** template and click **Next**  
	![](images/11.png)

1. Enter **bpr\_bam\_XXX** (where **XXX** is your workstation ID) as project name and click **Next**  
	![](images/12.png)

1. Select 


	| Parameter | Description |
	| --------- | ----------- |
	| Service   | **Java**        |
	| Java Package | **com.sap.sample.bpr\_bam\_XXX** (where **XXX** is your workstation ID) |
	| Database | **not included** |


	and click **Finish**  
	![](images/13.png)

1. In the Workspace now you can see your new project  
	![](images/14.png)

1. If you access now your Cloud Foundry environment <https://account.hana.ondemand.com/cockpit#/globalaccount/8fd39023-a237-4d71-9b6a-ed9d1719d275/subaccount/1d4a4a5d-2ada-4584-9c6a-631cb5fa80f2/spaces> and click on your space...  
	![](images/15.png)

1. ... you can see that there is a new application running: that's the **Cloud Foundry Builder**  
	![](images/16.png)

1. If you click on **Service Instances** in the left-hand menu, you can see that there is still one service instance (related to the "destination" service), we defined in the previous exercise  
	![](images/17.png)




### <a name="external-service"></a>Integrate an external service in the project
In our project, we want now to use an external service like the one for retrieving Business Partners from S/4HANA Cloud. Let's see how to add this external service and how to redefine it using a CDS file.

1. First of all, go back to your SAP Web IDE and double click on the *mta.yaml* file to open it in the editor  
	![](images/18.png)

1. Replace the entire content with the following:

	```yml
	ID: bpr_bam_XXX
	_schema-version: '3.1'
	version: 0.0.1
	modules:
	  - name: bpcrud_XXX
	    type: java
	    path: srv
	    parameters:
	      memory: 768M
	      disk-quota: 512M
	    requires:
	      - name: bpr_destination
	      - name: uaa_bpr_bam
	    provides:
	      - name: srv_api
	        properties:
	          url: '${default-url}'
	    properties:
	      ALLOW_MOCKED_AUTH_HEADER: true
	resources:
	  - name: bpr_destination
	    type: destination
	  - name: uaa_bpr_bam
	    parameters:
	       service-plan: application
	       service: xsuaa
	    type: org.cloudfoundry.managed-service
	```
  	make also sure you replace the **XXX** text with your workstation ID. Then save the file.
  	Just to give you a little background to this file, here we are defining a new module named "bpcrud\_XXX" which will be your new OData service. This services requires two CF service instances: one (bpr\_destination) for the destination to the back-end data and another (uaa\_bpr\_bam) for the security management. We also increased the memory and disk quotas for this service  
	![](images/19.png)

1.  In order to include and external service in your project, right click on the *srv* folder and choose **New -> Data Model from External Service**  
	![](images/20.png)

1. Select **Service URL**, locate the destination named **ErpQueryEndpoint** (pay attention that this destination is the same we defined in the previous exercise, so it should be already in place), enter the path `/sap/opu/odata/sap/API_BUSINESS_PARTNER` and click on the **Test** button  
	![](images/21.png)

1.  Verify that the service is available and click on **Next**  
	![](images/22.png)

1. Deselect the **Generate Virtual Data Model classes** checkbox and click on **Finish**  
	![](images/23.png)

1. Please re-enter your Cloud Foundry credentials in case they will be requested again  
	![](images/23_2.png)

1. After a while your service model will be created  
	![](images/24.png)

1. You should see something like this in your SAP Web IDE  
	![](images/25.png)

1. Let's now define our Business Partners service. Double click on the *my-service.cds* file to open it in the editor  
	![](images/26.png)

1. Replace its content with the following code and save the file. As you can see here, we are loading the service definitions from the file *./external/csn/API\_BUSINESS\_PARTNER*. Based on this file we are exposing several fields in our service and we are also telling to the service provider that we are going to define **custom handlers** for CRUD requests - we will see this in a bit e.g.: @Read(serviceName = "CrudService", entity = "BusinessPartner") - by specifying the statement **@cds.persistence.skip**  

	```
	using API_BUSINESS_PARTNER as bp from './external/csn/API_BUSINESS_PARTNER';
	
	service CrudService {
		@cds.persistence.skip
	     entity BusinessPartner as projection on bp.A_BusinessPartnerType{
	      BusinessPartner,
	      BusinessPartnerFullName,
	      LastName,
	      FirstName,
	      BusinessPartnerCategory,
	      CreatedByUser,
	      CreationDate,
	      BusinessPartnerUUID,
	      PersonNumber
	     };
	}
	```
	![](images/27.png)

1. After a while you should receive the message **Build of "/bpr_bam" completed.**  
	![](images/27_2.png)




### <a name="crud-classes"></a>Create the custom CRUD classes

1. Locate the *bpr\_bam\_XXX* folder (where XXX is your workstation ID) under the path *bpr\_bam\_XXX/srv/src/main/java/com/sap/sample/bpr\_bam\_XXX* and, right clicking on it, choose **New -> Folder**  
	![](images/28.png)

1. Create a folder named *commands*  
	![](images/29.png)

1. Create another named *crud*  
	![](images/30.png)

1. This is what you should see at the end of this process  
	![](images/31.png)

1. Right click on the *commands* folder and create a new Java class by choosing **New -> Java Class**  
	![](images/32.png)

1. Name this class as *BusinessPartnerReadByKeyCommand* 
	![](images/33.png)

1. Create another class in the same folder named *BusinessPartnerReadCommand*  
	![](images/34.png)

1. Create another Java class under the *crud* folder  
	![](images/35.png)

1. Name this class as *BusinessPartnerRead*  
	![](images/36.png)

1. This is what you should get at the end of this process  
	![](images/37.png)

1. Now, double click on the *BusinessPartnerReadByKeyCommand*, replace just the class declaration with the following content and save the file (do not touch the first line containing the package declaration)

	```java
	import com.netflix.hystrix.exception.HystrixBadRequestException;
	import com.sap.cloud.sdk.odatav2.connectivity.ODataException;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpCommand;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpConfigContext;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartner;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartnerByKeyFluentHelper;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.services.DefaultBusinessPartnerService;
	
	public class BusinessPartnerReadByKeyCommand extends ErpCommand<BusinessPartner> {
	
	    private final ErpConfigContext erpConfigContext;
	    private String businessPartner;
	
	    public BusinessPartnerReadByKeyCommand(ErpConfigContext erpConfigContext, String businessPartner) {
	        super(BusinessPartnerReadByKeyCommand.class, erpConfigContext);
	        this.businessPartner = businessPartner;
	        this.erpConfigContext = erpConfigContext;
	    }
	
	    @Override
	    protected BusinessPartner run() {
	
	        BusinessPartnerByKeyFluentHelper service = new DefaultBusinessPartnerService()
	                .getBusinessPartnerByKey(businessPartner);
	
	        try {
	            return service.execute(erpConfigContext);
	        } catch (final ODataException e) {
	            throw new HystrixBadRequestException(e.getMessage(), e);
	        }
	    }
	}
	```

	![](images/38.png)

1. Double click on the *BusinessPartnerReadCommand*, replace the existing class with the following content and save the file (do not touch the first line)

	```java
	import com.netflix.hystrix.exception.HystrixBadRequestException;
	import com.sap.cloud.sdk.odatav2.connectivity.ODataException;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpCommand;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpConfigContext;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.helper.Order;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartner;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartnerField;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartnerFluentHelper;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.services.DefaultBusinessPartnerService;
	import com.sap.cloud.sdk.service.prov.api.request.OrderByExpression;
	
	import java.util.*;
	
	public class BusinessPartnerReadCommand extends ErpCommand<List<BusinessPartner>> {
	
	   
	    private final int top;
	    private final int skip;
	    private final BusinessPartnerField[] selectedProperties;
	    private final List<OrderByExpression> orderByProperties;
	    private final ErpConfigContext erpConfigContext;
	
	
	    public BusinessPartnerReadCommand(ErpConfigContext erpConfigContext, int top, int skip, List<String> properties, List<OrderByExpression> orderByProperties) {
	        super(BusinessPartnerReadCommand.class, erpConfigContext);
	        this.erpConfigContext = erpConfigContext;
	        this.top = top;
	        this.skip = skip;
	        selectedProperties = properties.stream().
	                map(property -> new BusinessPartnerField(property))
	                .toArray(BusinessPartnerField[]::new);
	
	        this.orderByProperties = orderByProperties;
	    }
	
	    @Override
	    protected List<BusinessPartner> run() {
	
	        BusinessPartnerFluentHelper service = new DefaultBusinessPartnerService()
	                .getAllBusinessPartner();
	
	        orderByProperties.stream().forEach(expression -> service.orderBy(new BusinessPartnerField<>(expression.getOrderByProperty()), expression.isDescending() ? Order.DESC : Order.ASC));
	
	        service.select(selectedProperties);
	
	        if (skip > 0)
	            service.skip(skip);
	
	        if (top > 0)
	            service.top(top);
	
	        try {
	            return service.execute(erpConfigContext);
	
	        } catch (final ODataException e) {
	            throw new HystrixBadRequestException(e.getMessage(), e);
	        }
	    }
	}
	```
	![](images/39.png)


1. Finally, double click on the *BusinessPartnerRead*, replace the class with the following content without touching the first line, replace the **XXX** on lines 12 and 13 with your workstation ID and save the file

	```java
	import com.sap.cloud.sdk.service.prov.api.operations.Read;
	import com.sap.cloud.sdk.service.prov.api.response.ReadResponse;
	
	import org.slf4j.Logger;
	import org.slf4j.LoggerFactory;
	
	import java.util.List;
	import java.util.stream.Collectors;
	
	import com.sap.sample.bpr_bam_XXX.commands.BusinessPartnerReadByKeyCommand;
	import com.sap.sample.bpr_bam_XXX.commands.BusinessPartnerReadCommand;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpConfigContext;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartner;
	import com.sap.cloud.sdk.service.prov.api.operations.Query;
	import com.sap.cloud.sdk.service.prov.api.request.QueryRequest;
	import com.sap.cloud.sdk.service.prov.api.request.ReadRequest;
	import com.sap.cloud.sdk.service.prov.api.response.QueryResponse;
	
	
	public class BusinessPartnerRead {
	    private final Logger logger = LoggerFactory.getLogger(this.getClass());
	
	    @Read(serviceName = "CrudService", entity = "BusinessPartner")
	    public ReadResponse readSingleCustomerByKey(ReadRequest readRequest) {
	        logger.debug("Received the following keys: {} ", readRequest.getKeys().entrySet().stream().map(x -> x.getKey() + ":" + x.getValue())
	                .collect(Collectors.joining(" | ")));
	
	        String id = String.valueOf(readRequest.getKeys().get("BusinessPartner"));
	
	        BusinessPartner partner = new BusinessPartnerReadByKeyCommand(new ErpConfigContext("ErpQueryEndpoint"), id).execute();
	
	        ReadResponse readResponse = ReadResponse.setSuccess().setData(partner).response();
	
	        return readResponse;
	    }
	
	    @Query(serviceName = "CrudService", entity = "BusinessPartner")
	    public QueryResponse queryCustomers(QueryRequest qryRequest) {
	
	        List<BusinessPartner> businessPartners = new BusinessPartnerReadCommand(new ErpConfigContext("ErpQueryEndpoint"),
	                qryRequest.getTopOptionValue(),
	                qryRequest.getSkipOptionValue(),
	                qryRequest.getSelectProperties(),
	                qryRequest.getOrderByProperties())
	                .execute();
	
	        QueryResponse queryResponse = QueryResponse.setSuccess().setData(businessPartners).response();
	        return queryResponse;
	    }
	}
	```

	![](images/40.png)




### <a name="build-deploy"></a>Build and deploy the project
1. We can build the project. Right click on the project name and choose **Build -> Build**  
	![](images/41.png)

1. After a while, you should receive a "BUILD SUCCESS" message. Also, a new folder named *mta_archives* should be available in the project exporer of your SAP Web IDE  
	![](images/42.png)

1. Right click on the *bpr\_bam\_XXX\_0.0.1.mtar* package located under this folder and choose **Deploy -> Deploy to SAP Cloud Platform**  
	![](images/43.png)

1. Enter your **Cloud Foundry API Endpoint** with your credentials and click **Deploy**  
	![](images/44.png)

1. After a few minutes the deployment ends and you should receive a message with a button to go straight to the service. Click on the **Open** button  
	![](images/45.png)

1. A new browser page opens up, showing the content of this service. It reports all the available OData Endpoints. For this service we have just one: click on its link  
	![](images/46.png)

1. The endpoint is reached and its definition is shown  
	![](images/47.png)

1. If you append the "BusinessPartner" entity name to this OData Endpoint you will get the list of all available business partners coming from the service
	> NOTE: This screenshot has been taken on Google Chrome, because it better shows the OData feed in the XML format  
	
	![](images/48.png)

1. Looking at your Cloud Foundry cockpit now, you should find a new **bpcrud\_XXX** application listed 
	![](images/49.png)

1. Switching to the **Service instances** you should find two more instances: one named **bprbamXXX-uaabprbam-P\<user\>-\<random_characters\>** and another named **uaa\_bpr\_bam**. The first one has been automatically generated by the creation of the Data Model from External Service; the second one is the one we defined in the *mta.yaml* file  
	![](images/49_2.png)

1. Please delete the both **bprbamXXX-uaabprbam-P\<user\>-\<random_characters\>** and **uaa\_bpr\_bam** service instances because the first one is not needed and the second one will be recreated with the required parameters for the front-end application in the next chapter  
	![](images/50.png)




### <a name="add-annotations"></a>Add some annotations to the service
1. Before we create our frontend application with the List Report template, we want to create some OData annotations directly on the main service. To do this, right click on the *srv* folder and choose **New -> File**  
	![](images/51.png)

1. Enter *main-annotations.cds* as the name for this new file and click **OK**  
	![](images/52.png)

1. Enter the following content for this file and save it. Saving this file will automatically trigger a build phase where the annotation file will be parsed and applied to the service

	```
	using CrudService as cs from './my-service.cds';
	
	annotate cs.BusinessPartner with @(
		UI: {
			LineItem: [ 
				{$Type: 'UI.DataField', Value: BusinessPartner, "@UI.Importance": #High},
				{$Type: 'UI.DataField', Value: BusinessPartnerFullName, "@UI.Importance": #High},
				{$Type: 'UI.DataField', Value: FirstName, "@UI.Importance": #High},
				{$Type: 'UI.DataField', Value: LastName, "@UI.Importance": #High}
			],
			DataPoint#DPCategory: {
				Value: BusinessPartnerCategory,
				Title: 'Category'
			},
			HeaderInfo: {
				TypeName: 'Business Partner',
				TypeNamePlural: 'Business Partners',
				Title: { Value: BusinessPartner },
				Description: { Value: BusinessPartnerFullName }
			}
		}
	);
	```
	![](images/53.png)

1. At the end of the process you will get a message informing you that the project has been successfully rebuilt  
	![](images/54.png)


### <a name="frontend-application"></a>Create the front end application, build and deploy again

1. Now let's create our frontend application. Right click on the *bpr\_bam* project and choose **New -> HTML5 Module**  
	![](images/55.png)

1. Under the **Featured** category, choose the **List Report Application** template and click **Next**  
	![](images/56.png)

1. Enter **frontend\_XXX** (where **XXX** is your workstation ID) for the **Module Name** and **frontend** for the **Title** and click **Next**  
	![](images/57.png)

1. Select **Current Project** as the data source and check that you are able to see the **CrudService** you defined earlier. Then click **Next**  
	![](images/58.png)

1. Click **Next**  
	![](images/59.png)

1. Choose **BusinessPartner** as OData Collection and click **Finish**  
	![](images/60.png)

1. This is what you should get at the end  
	![](images/61.png)

1. Open the *mta.yaml* file in the editor and replace the entire content with this. Remember to replace **XXX** with your workstation ID and save the file

	```yml
	ID: bpr_bam_XXX
	_schema-version: '3.1'
	version: 0.0.1
	modules:
	  - name: bpcrud_XXX
	    type: java
	    path: srv
	    parameters:
	      memory: 768M
	      disk-quota: 512M
	    requires:
	      - name: bpr_destination
	      - name: uaa_bpr_bam
	    provides:
	      - name: srv_api
	        properties:
	          url: '${default-url}'
	    properties:
	      ALLOW_MOCKED_AUTH_HEADER: true
	
	  - name: frontend_XXX
	    type: html5
	    path: frontend_XXX
	    parameters:
	       disk-quota: 1024M
	       memory: 1024M
	    build-parameters:
	       builder: grunt
	    requires:
	     - name: srv_api
	       group: destinations
	       properties:
	          forwardAuthToken: true
	          strictSSL: false
	          name: srv_api
	          url: ~{url}
	     - name: uaa_bpr_bam
	
	resources:
	  - name: bpr_destination
	    type: destination
	  - name: uaa_bpr_bam
	    parameters:
	       path: ./xs-security.json
	       service-plan: application
	       service: xsuaa
	    type: org.cloudfoundry.managed-service

	```
	![](images/62.png)

1. Let's build the project again. Right click on the project's name and choose **Build -> Build**  
	![](images/63.png)

1. The build phase should end successfully  
	![](images/64.png)

1. Right click on the *mtar* file in the *mta\_archives/bpr\_bam\_XXX* folder and choose **Deploy -> Deploy to SAP Cloud Platform**  
	![](images/65.png)

1. Choose the right Cloud Foundry API Endpoint together with the Organization and the Space and click **Deploy**  
	![](images/66.png)

1. After some minutes the deployment ends and you get the chance to open the just deployed frontend application directly from SAP Web IDE. Click on the **Open** button for the **frontend\_XXX** application  
	![](images/67.png)

1. The frontend application is opened. Enter your SAP Cloud Platform credentials and click **Log On**  
	![](images/68.png)

1. The launch pad is shown. Click on the **frontend\_XXX** (where XXX is your workstation ID) tile  
	![](images/69.png)

1. The List Report application is shown. Notice that you can already see in the list header the three fields we defined in the annotation file earlier. Click on **Go**  
	![](images/70.png)

1. The list of business partners is shown (many are empty because they simply don't have any LastName and FirstName)
	![](images/71.png)

1. If you just click on one of those records you can get the detail fields coming from the annotation file     
	![](images/72.png)

1. Congratulations! You have successfully completed the exercise.



## Summary
This concludes the exercise. You should have learned how to create a new application with the Application Programming Model template in SAP Web IDE Full-Stack and how to create in the same project a frontend application to consume your business partners service. Please proceed with the [next exercise](../Exercise3/Exercise3.md).
