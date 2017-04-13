# Performance analysis
The goal in this part of the workshop is to perform analysis of the performance problems existing in our sample application _DVD Store_.

The prerequisite for this part is that you have finished the [instrumentation part](INSTRUMENTATION.md) of the workshop. We also assume that you have your _DVD Store_ up and running with the inspectIT agent and that configuration is same as described in the [instrumentation section](INSTRUMENTATION.md).

#### Performance problems in the *DVD Store*
In our sample application we intentionally placed some performance problems. When you start the *DVD Store* none of these will be activated. In order to activate one or more problems please use the *Application Performance Settings* link in the main menu.

![Application Performance Settings](images/performanceSettingsScreenshot.png?raw=true)

## Slow search
The first problem that we will try to analyze is the *Slow Search*. Note that this problem will be analyzed by the workshop host as well, so try to keep up with the big screen.

On the *Application Performance Settings* activate the **Slow Search** problem. Then execute at least two searches on our shop (needs navigation to the *Shop*). For example we will search for:

1. Batman
2. Interview Vampire

You should already notice that the search action is taking few seconds to be executed.

##### Locate the traces

We will start our analysis by understanding what input do we have about this problem - it's happening on the search, thus what we can do is check the URL of the returned search results page. This can help us in order to locate the traces in inspectIT that are describing these two searches we made. In browser we can see that the URL ```http://localhost:8080/dvdstore/browse``` is returned, so we can try to find all the HTTP requests that have this URL. Open the **Data Explorer View** and click on the ![Http Timer Data](images/discovery.gif?raw=true) *Http Timer Data -> ![URI aggregation](images/url.gif?raw=true) URI aggregation* menu item. Try to locate the row with the URI we are searching for (you can use sorting on the URI column). Found it? 

OK great, but wait it seams like we have more than 2 requests landing on this URI? That's because the same URL is used when clicking on the *Shop* in the main menu. So how can we differentiate? Well the search is most likely calling the POST HTTP. So is there a chance that inspectIT can separate the Http data by the method? Yes there is, just click on the  ![Include Request Method in Categorization](images/showcat_co.gif?raw=true) *Include Request Method in Categorization* in the editor tool-bar.

Now when we located the two POST requests for the search operation, we can see these two traces by *Right click -> Navigate To -> Invocations*. We said that the getting concrete numbers is important part of performance analysis. Thus, take few seconds to fully understand what's the duration of these two search operations.

##### Analyze the traces
We can continue by analyzing the first trace, so double-click on the trace and we should get the complete call hierarchy of the trace.  Can the problem be found just by analyzing the call hierarchy? If this is too hard, try doing the following: Open the ![SQL](images/database-sql.png) SQL or ![Method](images/methpub_obj.gif) Method sub-view. These sub-views show the aggregated information about all SQLs and all methods that were executed inside this trace. Check the executed SQLs times or check the exclusive method durations. When you find an item that seams off, use the *Right click -> Locate in Call Hierarchy* option to switch back to the place in call hierarchy where the item was executed.

##### Problem found?
Are you ready to make first assumptions why is this problem occurring? If so, test your theory on the second trace and see if it fits. If it does, then congratulations, you solved your first performance problem using inspectIT.

## Refund calculation
The refund calculation problem occurs during the checkout process, when the refund on purchase needs to be calculated for the user. This example can show that executing the same use case in different situations can produce different performance results.

Start by activating the *Refund Calculation* performance problem on the *Application Performance Settings* page. Buy at least two or more movies. First do it as **user1** and then the second time buy the same movie(s) as **user8** (log-in credentials are user1/password and user8/password). 

As in the previous problem analysis, first we need to locate the traces that hold the problem in question. You can try searching for the traces by the HTTP URL as in previous example. Or you can alternatively start from the ![Timer Data](images/method_time.gif?raw=true) *Timer data -> ![Show All](images/all_instances.gif?raw=true) Show All* view and check average duration of all monitored methods in the application. It should be easy to locate the methods that are responsible for the refund calculations and navigate to traces from these.

Once you have located the (two) traces you should see a clear difference in response time, although the same use case was executed. Can you find out which user was affected by this performance problem: *user1* or *user8*? Can you identify what is the difference in the execution time? Once you gathered all the numbers you can start analyzing the traces in order to find out why is one slower than another. If you located the reason for the performance problem can you suggest the solution for fixing it without checking the source code? 

## Share your findings

You successfully managed finding performance problems. You wonder how you store your findings for later access or how you can share your findings with others? Check out the next section [Collaboration](COLLABORATION.md) to find answers to your questions.
