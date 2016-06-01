# Collaboration
The goal in this part of the workshop is to show that inspectIT enables easy data exchange between operators and developers.

Until now we have learned how to [setup inspectIT](SETUP.md), [configure instrumentation](INSTRUMENTATION.md) and how we can use inspectIT to [analyze performance problems](ANAYSIS.md).

## Storages
The inspectIT stores most of the monitoring data in memory to improve performance and scalability on the CMR and to provide faster access to the data for the client. However, sometimes it is necessary to persist the important data to disk and have it for future reference and easy data exchange. To handle this inspectIT provides the [complete storage functionality](https://inspectit-performance.atlassian.net/wiki/display/DOC16/Working+with+disk+storage). 

### Save data to storage
Usually when performance problem is located, the performance data that shows such a problem should be exchanged between developers, operators and/or performance team. In inspectIT we can save performance data to the storage(s) that can be easily manipulated. The first thing do to is locate the data to save. For this exercise, we would like to save all the "problematic" traces that we found during our performance analysis into one storage:

1. Two traces describing the Search problem
2. Two traces describing the Refund calculation problem.

You already learned how to locate these traces. Once you found them execute the *Right click -> Save to Server* action. The *Save Data to Storage* wizard will open asking if we want to create a new or use existing storage to write data to. We will select the creation of new storage first. Note that storage is always initially created on the CMR repository. Give storage name and description (optional). Be careful about the **Auto-finalize storage** option, if this option is selected the new storage will be finalized after the save making it read-only. If you plan to add traces from several tries, make sure you leave this option unchecked.

### Export storage
Once all the data you want is in the storage you created you can export this data from the ![Storage Manager View](images/storage_overlay.gif?raw=true) *Storage Manager View*. Make sure that the storage is finalized and thus set in the read-only mode. Export is possible only for the finalized storages.

The *.itsd* file that you created can easily be sent to anyone having inspectIT.

### Import storage
Importing of *.itsd* files is also possible via ![Storage Manager View](images/storage_overlay.gif?raw=true) *Storage Manager View*. Simply select the file to import and location. Location can be CMR repository or your local RCP app. If you imported the storage locally it will be visible in the ![Storage Manager View](images/storage_overlay.gif?raw=true) *Storage Manager View* if the **Show available: Local** is selected.
