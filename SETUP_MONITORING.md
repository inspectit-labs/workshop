# Setup monitoring
The goal in this part of the workshop is to setup up all necessary components that are needed in order to use inspectIT for the monitoring.

The prerequisite for this part is that you have finished the [setup part](SETUP.md) of the workshop.

## InfluxDB and Grafana
The inspectIT is using InfluxDB time-series database as the storage for all the monitoring data. The data from the influxDB can be then visualized with the Grafana web-based user interface.

### Install InfluxDB
Visit the [InfluxDB download page](https://www.influxdata.com/downloads/#influxdb) and follow the instructions in order to install the *InfluxDB Time-Series Data Storage*. The inspectIT is compatible with version 1.x of InfluxDB. Start the *influxdb* service and verify that everything is working by opening the web-based view of InfluxDB located by default on the [http://localhost:8083](http://localhost:8083).

In the inspectIT UI open the ![Configure Repository](images/build.gif?raw=true) *Configure Repository* dialog and perform the following changes:

1. In the preference page *Database* set the property **Write Data to influxDB** to true (On).
2. In the preference page *influxDB* set the following properties:

Property | Value
--- | ---
Host | localhost
Port | 8086
Username | root
Password | root
Database Name | inspectit

In the log output of the CMR process you should see the following message: 
```[service-thread-1] INFO  .server.influx.dao.InfluxDBDao - |-InfluxDB Service active and connected...```.

### Install Grafana
Visit the [Grafana download page](http://grafana.org/download/) and install the Grafana distribution for your operating system. Start Grafana service. Verify that Grafana is running by visiting the Grafana's web-based UI located by default on [http://localhost:3000](http://localhost:3000). Login into the UI using the default credentials ```admin / admin```.


The last thing to do is to connect Grafana to the **inspectit** database that we previously created in InfluxDB instance. Open the Grafana administration page (click on the icon in the top left corner) and select *Data Sources -> Add New*. Then add following information for the new data source:

Property | Value
--- | ---
Name | inspectit-influx
Type | InfluxDB
Default | ✓
URL | http://localhost:8083
Access | proxy
Basic Auth | 
Database | inspectit 
User | root
Password | root

#### Import inspectIT dashboards (optional)
The inspectIT has predefined basic dashboards for the Grafana. Select the *Import* option in the dashboard navigation menu and import one or more available dashboards that could be found in the [inspectit-labs repository](https://github.com/inspectit-labs/dashboards).
