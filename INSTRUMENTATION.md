# Instrumentation configuration
The goal in this part of the workshop is to create a basic instrumentation for our sample application _DVD Store_.

The prerequisite for this part is that you have finished the [setup part](SETUP.md) of the workshop. We also assume that you have your _DVD Store_ up and running with the inspectIT agent.

## Configuration perspective
For all the instrumentation configurations you need to perform you will need to switch to the ![Configuration perspective](images/compass.png?raw=true) *Configuration perspective*. As in all Eclipse-based application, perspective bar is available on the top-right of your application window.

## Environment creation
The first thing to do is to create a new environment that will be used by the inspectIT agent that runs within the sample application. From the ![Configuration Manager](images/compass.png?raw=true) **Configuration Manager** view click on the ![Add](images/add_obj.gif?raw=true) *Add* menu item and select the *Create Environment* option. Define the environment name (for example *DVD Store [dev]* or any that you like) and click on *Finish*.

You will notice that created Environment comes with some default settings.  For example several common profiles are already selected:
 - **[Common] Exclude classes** - defines the default exclude classes patterns. This profile should always be included to insure the correct functioning of the inspectIT agent.
 - **[Common] HTTP** - defines configuration for instrumenting the HTTP calls. As our application is web based, we will keep this profile included.
 - **[Commons] SQL** - defines configuration for instrumenting database calls. As out application uses H2 database in the back-end, we will also keep this profile included.

For the beginning we will include additionally one more profile. As the application is using the Hibernate framework, please select the **[Common] Hibernate** profile as well.

Since this is the workshop we will change some of the default settings. In the *Sensor Options* part set the string length limitation for all the sensor types to unlimited and activate the enhanced exception sensor. Save the changes.

## Profile definition
### Create new profile
We need a profile where all our instrumentation points for the monitored application will be defined.  From the ![Instrumentation Manager](images/compass.png?raw=true) **Instrumentation Manager** view click on the ![Add](images/add_obj.gif?raw=true) *Add* menu item and select the *Create Profile* option. Define the profile name (for example *DVD Store [base profile]* or any that you like). For the profile type make sure that you select *Sensor assignment* and click on *Finish*.

### Define instrumentation points
After creating the new profile does not contain any sensor assignment. We will add several instrumentations points for the Timer sensor and Exception sensor:

##### 1. Public methods of ApplicationDispatcher
For the warm up we would like to instrument all the public methods of the `org.apache.catalina.core.ApplicationDispatcher`.  In the **Sensor Definitions** page click on the ![Add](images/add_obj.gif?raw=true) *Add* option and select *Timer sensor*. In the assignment details perform following definition:
```
Fully Qualified Name: org.apache.catalina.core.ApplicationDispatcher
Method / Method Name: *
Method visibility: public
```

##### 2. All EJBs in the package *com.jboss.dvd*
As our *DVD Store* application is using the Enterprise JavaBeans (EJB) in version 3.0, we would like to add timer sensor to all methods of all the EJBs that are defined in the package `com.jboss.dvd`. We know that EJB classes are annotated with the `javax.ejb.Stateless` or `javax.ejb.Stateful` annotations, thus we can use the *Annotation* option of the sensor assignment definition. Note that we will need separate definition for each annotation (*hint: You can use ![Duplicate](images/copy_edit.gif?raw=true) Duplicate button to copy assignments*):
```
Fully Qualified Name: com.jboss.dvd*
Annotation: javax.ejb.Stateless
Method / Method Name: *
```
```
Fully Qualified Name: com.jboss.dvd*
Annotation: javax.ejb.Stateful
Method / Method Name: *
```

##### 3. Execute method of the JavaFaces phases
As the JavaFaces are used on the presentation layer, we would also like to measure duration of the  *execute* method of all JavaFaces phase classes. All the phase classes have the same abstract super-class: `com.sun.faces.lifecycle.Phase`, so we can use the inspectIT *Superclass* option to instrument all the sub-classes of the `com.sun.faces.lifecycle.Phase` without actually knowing their fully qualified names:
```
✓ Superclass
Fully Qualified Name: com.sun.faces.lifecycle.Phase
Method / Method Name: execute
```
##### 4. All exceptions in the package *com.jboss.dvd* 
inspectIT is capable of capturing information about any exception being thrown in the JVM. We would like to use this option and catch all the application exceptions being thrown. These exceptions are all defined in the package `com.jboss.dvd`  and its sub-packages. Add a new sensor assignment definition and choose *Exception sensor*. Then perform following definition:
```
Fully Qualified Name: com.jboss.dvd*
```
###### 4a. What about Java exceptions?
inspectIT can instrument the Java native classes as well. Thus, select your favorite Java exception that you would like to instrument and add this definition as well.

##### 5. [Advanced] Context capturing: search criteria
Beside measuring the execution time of the method, the *inspectIT Timer sensor* is capable of capturing context information like parameters, fields or return value at the method execution time. For this example, we would like to capture the parameter value passed to the method: `com.jboss.dvd.seam.FullTextSearchAction.searchTitleAndDescriptionWithOneWord(java.lang.String)`. Create new *Timer sensor* assignment and define the following options:
```
Fully Qualified Name: com.jboss.dvd.seam.FullTextSearchAction
Method / Method Name: searchTitleAndDescriptionWithOneWord
Parameters: ✓ Only method/constructor with selected parameters
            java.lang.String
Capture context: ✓  Yes 
                 type: Parameter (index 0), name: searchCriteria, accessor path:
```

### Include to the environment
After defining all the instrumentation point please save the profile and include it to our *DVD Storage [dev]* environment. Open the environment from the **Instrumentation Manager** view and make sure that the new profile is selected in the *Profile* list. Save the environment. 


## Alter agent mapping settings
You are almost finished. We have defined environment that our agent should use and included in this environment one custom profile that contains all of our instrumentation points. The last step is to map the created environment to the inspectIT agent that will run in the *DVD Store* application.

In the **Instrumentation Manager** tool-bar select the ![Agent Mapping Settings](images/agent.gif?raw=true) *Agent Mapping Settings* options. You should see that the current mappings define that all the agents should use the *Default Environment*:

Active | Agent Name | IP Address | Environment
--- | --- | --- | ---
✓ | * | * | Default Environment

Perform the following changes to create a new mapping that maps your DVD_Store agent to your DVD Store [dev] environment:

1. Deactivate existing mapping for the Default Environment
2. Add new mapping:
``` 
Active: ✓
Agent name: DVD_Store (same name you gave in the -Dinspectit.agent.name= option)
IP address: *
Environment: DVD Store [dev]
```

After performing the changes, you should see the new mapping:

Active | Agent Name | IP Address | Environment
--- | --- | --- | ---
✗ | * | * | Default Environment
✓ | DVD_Store | * | DVD Store [dev]

After saving the mappings settings you need to restart the *DVD Store* application so that the new instrumentation configuration is applied. After restart make sure that the new configuration working. Check the ![Instrumentation Browser](images/blue-document-tree.png?raw=true) *Instrumentation Browser* and see if new classes/methods are instrumented.

## Playground

If you were faster than the workshop pace we don't want you to be bored.  Play around with the environment settings or the profile definition. For example, you can do one of the following:
- Active the **[Commons] SQL Parameters** profile in the environment. When this profile is active all the SQLs reported bu inspectIT will have the actual parameter values instead of the '?'.
- Explore the accessor path option when capturing context: catch return of the `com.jboss.dvd.seam.CheckoutAction.submitOrder()`, but catch the field *trackingNumber* of the returned *Order*
