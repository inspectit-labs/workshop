# Setup
The goal in this part of the workshop is to install all inspectIT components on your machine and to integrate the inspectIT agent in the sample application _DVD Store_. This application will be used as the demonstration application during this workshop.

## Install inspectIT
The easiest and fastest way to install inspectIT is by running the installer. The installer(s) can be found on the [official GitHub repository](https://github.com/inspectIT/inspectIT/releases) or in the USB given to you at the beginning of the workshop. Note that for this workshop we will be using the inspectIT version 1.6.8.x. The installers are available for the Windows, Linux and Mac platform, so be careful which one you choose.

The installer is a *Executable Jar File (.jar)* so make sure that you open it with the Java Runtime Platform. The installer will guide you through the installation of all inspectIT components. After the successful installation following inspectIT components will be installed:
* Agent
* Central Management Repository (CMR, server)
* inspectIT RCP App (rich user interface)

##### ![Warning](images/warning_obj.gif?raw=true) Warning: Avoid whitespaces in installation path
It's advised that the installation path you specify during installation does not have any whitespaces. This way you will not encounter any difficulties when you need to integrate inspectIT Agent using JVM parameters.

## Start CMR
The Central Management Repository (CMR) is the main (server) component in inspectIT. As both agent(s) and inspectIT RCP App(s) are depending on the repository, we will start it first. Simply navigate to the CMR repository and run the *startup.sh* or *startup.bat* script.

```
cd [INSTALLATION_DIR]/CMR
./startup.sh 
```

Make sure to check the log and confirm that the CMR has started successfully. You should see the log message like following:

```
...
2016-06-01 12:41:23,243: 10631  [           main] INFO      rocks.inspectit.server.CMR - CMR started in 10262.008131 ms

``` 

## Start inspectIT RCP App 
The inspectIT user interface is also started quite easily. Navigate to the ```[INSTALLATION_DIR]/inspectit``` directory and run the *inspectIT* or *inspectIT.exe*.

By default, when started the application will try to connect to the CMR listening on the *localhost:8182*. As we already started the CMR this connection should be successful. Make sure you check that by opening the ![Repository Manager View](images/server_instance.gif?raw=true) *Repository Manager View* and confirming that status of the *Local CMR* is ![On-line](images/server_online_16x16.png?raw=true) on-line (*hint: Hit F5 or click on ![Refresh](images/refresh.gif?raw=true) Refresh to update the view*).


##### ![Warning](images/warning_obj.gif?raw=true) Warning: Linux users
On some Linux distributions there is a known Eclipse bug that can cause the first application start to fail. But don't worry. This bug is related to the Welcome screen displayed only the first time you run the application. Simply try starting the application for the second time and this bug should be overpassed.

## Setup the sample application
As said we will use the _DVD Store_ application as the sample one for this workshop. This application runs on the JBoss 5.1 and it's a simple web application where you can buy DVDs.

### Unpack DVD Store
The _DVD Store_ application is packed into a ZIP archive and you can find it alongside data files on the USB stick you received. Simply unpack the ZIP archive in a directory of your choosing and application should be ready for starting. You can test that application works by executing the *startJBoss.sh* or *startJBoss.bat* and opening [localhost:8080/dvdstore](http://localhost:8080/dvdstore) in your browser.

### Integrate inspectIT agent
As the last step in our setup process we need to attach the inspectIT agent to the Java process running the _DVD Store_. The integration is as simple as adding the following to the startup script of the Java application:

```
-javaagent:[INSPECTIT_AGENT_HOME]/inspectit-agent.jar -Dinspectit.repository=localhost:9070 -Dinspectit.agent.name=DVD_Store
```

In the case of the JBoss 5.1, the configuration of the startup script is possible by altering the *run.conf* or *run.conf.bat* (Windows users) located in the ```[DVD_STORE_DIR]/jboss-5.1.0.GA/bin```. Simply append the inspectIT options to the existing JAVA_OPTS property:

**run.conf (Linux)**
```
JAVA_OPTS="$JAVA_OPTS -javaagent:[INSPECTIT_AGENT_HOME]/inspectit-agent.jar -Dinspectit.repository=localhost:9070 -Dinspectit.agent.name=DVD_Store"
```
**run.conf.bat (Windows)**
```
set "JAVA_OPTS=%JAVA_OPTS% -javaagent:[INSPECTIT_AGENT_HOME]\inspectit-agent.jar -Dinspectit.repository=localhost:9070 -Dinspectit.agent.name=DVD_Store"
```

## Get the first data
You need to restart the *DVD Store* application once you configured the start script. This will activate the inspectIT agent. Once you restarted the application, switch back to the inspectIT UI and make sure that the agent with the name *DVD_Store* in visible in the ![Repository Manager View](images/server_instance.gif?raw=true) *Repository Manager View* under the *Local CMR* repository. 

Double-clicking on the agent item should open the ![Data Explorer View](images/catalog.gif?raw=true) *Data Explorer View* where you can see the data already sent by the agent. Feel free to explore a bit, check the following things:

* ![Instrumentation Browser](images/blue-document-tree.png?raw=true) *Instrumentation Browser* shows you which classes are instrumented by default
* ![System Overview](images/system-monitor.png?raw=true) *System Overview* can already deliver data about system utilization (CPU, memory, etc.)
* If you open the *DVD Store* again in your browser, you will be able to see other data as well (like ![SQLs](images/database-sql.png?raw=true) SQLs, ![Timer Data](images/method_time.gif?raw=true) Timer Data, ![HTTP Data](images/discovery.gif?raw=true) HTTP Data, etc).


