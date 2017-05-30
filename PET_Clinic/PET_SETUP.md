# Setup
The goal in this part of the workshop is to install all inspectIT components on your machine and to integrate the inspectIT agents in the sample application _Spring Petclinic Microservices_. This application will be used as the demonstration application during this workshop.

# Start/Stop Spring Petclinic Application with inspectIT
To start the _Spring Petclinic Microservices_ application please clone the code from https://github.com/inspectit-labs/spring-petclinic-microservices and use branch "inspectIT". Afterwards, you can start the PetClinic Microservices application including the inspectIT agents either with or without docker. Choose one of both options and check afterwards if you can get first monitoring data with inspectIT. 

## With Docker (Docker-compose)
First of all, inspectIT should be installed on your computer. The easiest and fastest way to install inspectIT is by running the installer. The installer(s) can be found on the [official GitHub repository](https://github.com/inspectIT/inspectIT/releases). Download and install the latest stable version of inspectIT. The installers are available for Windows, Linux and Mac platform, so be careful which one you choose. 

The installer is a *Executable Jar File (.jar)* so make sure that you open it with the Java Runtime Platform. The installer will guide you through the installation of all inspectIT components. Warning: Avoid whitespaces in installation path. After the successful installation following inspectIT components are installed:
* Agent
* Central Management Repository (CMR, server)
* inspectIT RCP App (rich user interface)

Afterwards, navigate to the code of the PetClinic Microservices application and run the following command to build the docker image: 
```
mvn clean install -PbuildDocker
```

When the image is build you can start all applications: 
```
docker-compose -f docker-compose-inspectIT.yml up -d
```

During startup (this can take a few minutes) you can track services availability using Eureka dashboard available by default at http://localhost:8761. The inspectIT CMR is started automatically but the inspectIT UI has to be opened from the installed version. Navigate to the ```[INSTALLATION_DIR]/inspectit``` directory and run the *inspectIT* or *inspectIT.exe*. In the inspectIT UI five agents of the PetClinic Microservices application should be listed and connected. When the application is running it can be accessed here: http://localhost:8080. You can stop the application with: 
```
docker-compose -f docker-compose-inspectIT.yml stop
```

## Without Docker
First of all, inspectIT should be installed on your computer. The easiest and fastest way to install inspectIT is by running the installer. The installer(s) can be found on the [official GitHub repository](https://github.com/inspectIT/inspectIT/releases). Download and install the latest stable version of inspectIT. The installers are available for Windows, Linux and Mac platform, so be careful which one you choose. The installer is a *Executable Jar File (.jar)* so make sure that you open it with the Java Runtime Platform. The installer will guide you through the installation of all inspectIT components. Warning: Avoid whitespaces in installation path. After the successful installation following inspectIT components are installed:
* Agent
* Central Management Repository (CMR, server)
* inspectIT RCP App (rich user interface)

After the installation start CMR:
```
cd [INSTALLATION_DIR]/CMR
./startup.sh 
```

Make sure to check the log and confirm that the CMR has started successfully. You should see the log message like following:

```
...
2016-06-01 12:41:23,243: 10631  [           main] INFO      rocks.inspectit.server.CMR - CMR started in 10262.008131 ms

``` 

The inspectIT user interface is also started quite easily. Navigate to the ```[INSTALLATION_DIR]/inspectit``` directory and run the *inspectIT* or *inspectIT.exe*.

By default, when started the application will try to connect to the CMR listening on the *localhost:8182*. As we already started the CMR this connection should be successful. Make sure you check that by opening the ![Repository Manager View](../images/server_instance.gif?raw=true) *Repository Manager View* and confirming that status of the *Local CMR* is ![On-line](../images/server_online_16x16.png?raw=true) on-line (*hint: Hit F5 or click on ![Refresh](../images/refresh.gif?raw=true) Refresh to update the view*).

### Linux
On linux first open a new terminal. Navigate to the code of the _Spring Petclinic Microservices_ application and execute: 
```
cd [CODE_DIR]
./start_all_with_inspectIT.sh <path to inspectIT agent>
```
The inspectIT agent can be found in the installation directory of inspectIT ([INSTALLATION_DIR]/agent). During startup the application will be opened automatically in the browser. First, the Eureka dashboard will be opened where you can track services availability (http://localhost:8761). Then the PetClinic application will be opened (http://localhost:8080). 

To stop all processes you can use the script : 
```
./stop_all.sh
 ```
 
### Windows
Windows users please open a windows terminal using cmd. Navigate to the code of the PetClinic Microservices application and start: 
```
cd [CODE_DIR]
start_all_with_inspectIT.bat <path to inspectIT agent>
```

The inspectIT agent can be found in the installation directory of inspectIT ([INSTALLATION_DIR]/agent). During startup the application will be opened automatically in the browser. First the Eureka dashboard will be opened where you can track services availability (http://localhost:8761). Then the PetClinic application is opened (http://localhost:8080). By closing the cmd windows all processes will be stopped automatically.


## ![Warning](../images/warning_obj.gif?raw=true) Warning: Linux users
On some Linux distributions there is a known Eclipse bug that can cause the first application start to fail. But don't worry. This bug is related to the Welcome screen displayed only the first time you run the application. Simply try starting the application for the second time and this bug should be overpassed.


## Get the first data
Make sure that five agents (named: customers-service, vets-service, visits-service, api-gateway, admin-server) of the _Spring Petclinic Microservices_ application are visible in the ![Repository Manager View](../images/server_instance.gif?raw=true) *Repository Manager View* under the *Local CMR* repository. 

Double-clicking on one of the agent item should open the ![Data Explorer View](../images/catalog.gif?raw=true) *Data Explorer View* where you can see the data already sent by the agent. Feel free to explore a bit, check the following things:

* ![Instrumentation Browser](../images/blue-document-tree.png?raw=true) *Instrumentation Browser* shows you which classes are instrumented by default
* ![System Overview](../images/system-monitor.png?raw=true) *System Overview* can already deliver data about system utilization (CPU, memory, etc.)
* You will also be able to see other data as well (like ![SQLs](../images/database-sql.png?raw=true) SQLs, ![Timer Data](../images/method_time.gif?raw=true) Timer Data, ![HTTP Data](../images/discovery.gif?raw=true) HTTP Data, etc).

## Deeper visibility with instrumentation

If you played around with the different views you noticed that you already get some information out-of-the-box. In the next section [Instrumentation configuration](PET_INSTRUMENTATION.md), we will create a basic set of instrumentation rules to increase the visibility into the _Spring Petclinic Microservices_ application.
