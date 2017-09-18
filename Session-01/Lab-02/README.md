[vsCodeStartupCs]: img/vsCodeStartupCs.png " "
[appManagerMarketplace]: img/appManagerMarketplace.png " "
[appManagerMySql]: img/appManagerMySql.png " "
[vsCodeManifest]: img/vsCodeManifest.png " "

# Lab 02 - Bind, Scale and HA

## Bind MySql to App
1. In AppsManager, click the Marketplace link in left box.
![alt text][appManagerMarketplace]
2. Choose the MySql service, and a capacity plan. In this case the 100mb plan will be used
![alt text][appManagerMySql]
3. Click the blue 'Select this plan' button, name the new instance as mysql-100mb, select the <STUDENT-X> space, select the <dotnetappName>-studentX app, and click the blue 'Add' button
![alt text][appManagerMySqlValues]
4. Restart and view the app
5. Add more attendees and delete some attendees

## Scale the App manually
1. Change the number of instances to 3
2. Refresh the page and watch the instance index changes. it should be round robin.

## Kill one instance
1. Click on the Kill button on the app home page
2. Watch the app node gets resurrected in AppsManager


___

___
| **[Prev Lab](../Lab-01/README.md)** |_______________________________________________________________________________| **[Next Lab](../Lab-03/README.md)** |
