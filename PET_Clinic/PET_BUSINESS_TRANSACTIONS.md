# Setup business transactions
The goal in this part of the workshop is to setup up logical applications and correlate the gathered data to business contexts.

The prerequisite for this part is that you have finished the [setup part](PET_SETUP.md) of the workshop.

### Creating a Business Context

In inspectIT a business context represents an application and consists of application definitions and business transactions definitions. We're starting by creating an application context which relates the data to an application.

At first, you have to switch to the ![Configuration perspective](../images/compass.png?raw=true) *Configuration perspective*. As in all Eclipse-based application, perspective bar is available on the top-right of your application window.

Open the ![Business Context Manager](../images/briefcase.png?raw=true) **Business Context Manager** and click on the *Create Application* button. In the appearing dialog, you can specify the name of the application and assign a description, for example create the application Pet_Clinic_API which represents the AngularJS frontend (API Gateway):

Property | Value
--- | ---
Name | Pet_Clinic_API
Description | Pet Clinic Microservice application

### Creating Application Definitions

Once the application has been created, it is opened in the editor. 

The next step is to specify the **application definition**. In the **application definition** tab we can create **application mapping** that represent rules that match monitoring data to that specific application.  

Create a new **application mapping** by clicking on the ![Add](../images/add_obj.gif?raw=true) plus icon in the upper right corner of the view.

We know that each microservice of the Petclinic application is running on different ports. Therefore, a **HTTP Server Port Matching** should be created which matches all requests on a specific port to an application. The Pet_Clinic_API is running on port 8080. Specify this port and save the configuration. Now, all data matching this application definition is correlated to it. You can check this by navigating to the *Data Explorer View* of the api-gateway agent and open the Invocation Sequences. 

### Creating Business Transaction Definitions

In the next step, we can group the data of an application and combine them with a certain business context, for example: the business transaction *Owner* contains all data belonging to the process of editing the details of an owner.

#### 1. Creating Business Transaction for Owner

First, switch to the **Business Transaction Definitions** tab (at the bottom of the view) and click on the **Create** button. As before, a dialog appears where we can name the newly created business transaction definition. Enter a name, e.g. *Owner* and click on the *Finish* button.

Like the application definition, a business transaction definition consists of **business transaction mappings** which are used to correlate data to a business transaction.

As before, create an **URI Matcher** which maps all requests where URI starts with ```/api/customer/owners``` to the *Owner* Business Transaction. Press ```CTRL + S``` to save the changes. And that's it.

#### 2. Creating Business Transaction for Static Resources

Second, we would like to see all requests that are related to static resources. Create a new business tracsaction with the name "Static Resources". As before, create an **URI Matcher** which accepts all data where the URI matches the regex ```.*(js|css|woff|woff2|png)```.  

#### 3. Advanced Rules for Creating Business Transactions

In the Business Transaction Mapping multiple rules can be used to define one business transaction. These rules are linked with OR conditions. In case AND conditions should be used enable the "advanced rule view". In our case we want to split the business transaction *Owner* further into "Edit Owner" and "Get All Owner" business transactions. This business transaction has the same URI but can be further distinguished by the used HTTP Request Method. 

Create a new Business Transactions "Edit Owner". Press ```CTRL + S``` to save the changes. Then click on "enable advanced rule view". 

![Add](../images/enable_advanced_rules.png?raw=true)

Right click on the OR condition and add an AND condition. Right click on the AND condition and add "HTTP URI Matcher" which starts with the value ```/api/customer/owners```. Again right click on AND and add a second condition "HTTP Request Method Matching" and choose HTTP method PUT. The advanced rule definition should look like this:

![Add](../images/advanced_rules.png?raw=true)

Repeat that and create a second Business Transaction with the name "Get All Owner" but this time with "HTTP Request Method Matching" matching GET methods. 

When you go back to the Data Explorer View you might see that there is no Business Transaction with "Edit Owner" or "Get All Owner". This comes from the fact that the order of the Business Transaction Definitions is important. The definition that matches first, defines the name of the Business Transaction. Therefore, move the "Edit Owner" and "Get All Owner" to the top of the Business Transaction Definitions list. Then go back to the Data Explorer View refresh the data and check if the Business Transaction are there. 

#### 4. Dynamic Business Transaction Extraction

We still have requests that are not mapped to a Business Transaction. Thus, we would like to dynamically extract the name of the business transaction depending on the **URI**. Using dynamic extraction allows to easily specify Business Transactions in an automatic way. So you do not have to specify a business transaction rule for each request type. 

As before, create a new Business Trancation named *Dynamic BT* and add a **URI Matcher** which accepts all data where the URI starts with the value ```/```. Enable the "Dynamic Name Extraction" on the right side. Activate the checkbox "Extract name dynamically" and select the source based on which the name should be extracted. Choose **HTTP URI** from the dropdown list. In the **Name Pattern Specification** select **Specify name pattern** and add:
```
Regular Expressions: /([^/]*).*
Name Pattern: BT - (1)
```

This way the first part of the URI will be extracted and used as business transaction name, i.e. /owner/details --> "BT - owner" OR /pet/visit --> "BT - pet".
 
## Results

We have create an application definition including a business transaction definition. Navigating to the **Data Explorer View** of the api-gateway agent and check if all requests have the specified business transaction definition. 

Note: data which exists in the buffer will be updated when a business context is changed.
