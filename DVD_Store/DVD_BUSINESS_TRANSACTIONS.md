# Setup business transactions
The goal in this part of the workshop is to setup up logical applications and correlate the gathered data to business contexts.

The prerequisite for this part is that you have finished the [setup monitoring part](DVD_SETUP_MONITORING.md) of the workshop.

### Creating a Business Context

In inspectIT a business context represents an application and consists of application definitions and business transactions definitions. We're starting by creating an application context which relates the data to an application.

At first, you have to switch to the ![Configuration perspective](../images/compass.png?raw=true) *Configuration perspective*. As in all Eclipse-based application, perspective bar is available on the top-right of your application window.

Open the ![Business Context Manager](../images/briefcase.png?raw=true) **Business Context Manager** and click on the *Create Application* button. In the appearing dialog, you can specify the name of the apllication and assign a description, for example:

Property | Value
--- | ---
Name | DVDStore
Description | My e-commerce DVD web-shop

### Creating Application Definitions

Once a business context has been created, it is opened in the editor. Now, we have to specifie the application definition. In order to correlate data to an application **application mappings** are used.

Create a new **application mapping** by clicking on the plus icon in the upper right corner of the view.

We know that all application specific interfaces starting with ```/dvdstore/```, therefore, a **URI Matcher** should be created which matches all URIs which starts with ```/dvdstore/```.

Now, all data matching this application definition is correlated to it.

### Creating Business Transaction Definitions

In the next step, we can group the data of an application and combine them with a certain business context, for example: the business transaction *checkout* contains all data belonging to the checkout process.

First, switch to the **Business Transaction Definitions** tab (at the bottom of the view) and click on the **Create** button. As before, a dialog appears where we can name the newly created business transaction definition. Enter a name, e.g. checkout and click on the *Finish* button.

Like a application definition, a business transaction definition consists of **business transaction mappings** which are used to correlate data to a business transaction.

As before, create an **URI Matcher** which accepts all data where the value ```/checkout``` exists in its URI.

Press ```CTRL + S``` to save the changes. And that's it.

## Results

We have create an application definition including a business transaction definition. If a request is collected by inspectIT which URI is in the following pattern ```http://example.org/dvdstore/checkout/shipping``` it is correlated to the DVD-Store application and, furthermore, correlated to the **checkout** business transaction.

Note: data which exists in the buffer will be updated when a business context is changed.

## Want to do more ?

* Checkout the other performance problems contained in section *Application Performance Settings*.
* Improve your business transaction. Checkout the URIs used by the DVD Store application and define new Business Transaction Mappings.
* Feel free to monitor your own Java application with inspectIT. Can you find parts in your application which are worth to improve?
