[appManagerMarketplace]: img/appManagerMarketplace.png " "
[appManagerAutoscaler]: img/appManagerAutoscaler.png " "
[appManagerAutoscalerValues]: img/appManagerAutoscalerValues.png " "
[appManagerAutoscalerHome]: img/appManagerAutoscalerHome.png " "
[AppManagerAutoscalerSettings]: img/AppManagerAutoscalerSettings.png " "
[appManagerAutoscalerMinMax]: img/appManagerAutoscalerMinMax.png " "
[appManagerAutoscalerEnabled]: img/appManagerAutoscalerEnabled.png " "
[appManagerFTServiceHome]: img/appManagerFTServiceHome.png " "

# Lab 03 - Auto Scaling the App Instances

## Bind AutoScaling to App
1. In AppManager, click the Marketplace link in left box.

![alt text][appManagerMarketplace]

2. Choose the App Autoscaler service.

![alt text][appManagerAutoscaler]

3. Click the blue 'Select this plan' button, name the new instance as autoscaler, select the <STUDENT-X> space, select the fortune-teller-services app, and click the blue 'Add' button.

![alt text][appManagerAutoscalerValues]

4. The page will refresh with a summary of the services bound to the Development space. Click the "App Autoscaler" item, to view the individual service's page.

![alt text][appManagerAutoscalerHome]

5. Click the 'Manage' link at the top, to be shown the Autoscaler's settings.

![alt text][AppManagerAutoscalerSettings]

6. Currently the Fortune Teller service is using 1 instance. To make the app HA (Highly Available), our minimum number of instances should match the number of availability zones. The maximum number of instances is relative to an acceptable response latency. Click the 'Edit' link next to 'Instance Limits' and set minimum to 3 and maximum to 5. Then click the blue 'Save' button.

![alt text][appManagerAutoscalerMinMax]

7. Now that you have set limits, the auto scaler can be enabled. Toggle the 'Disabled' option at the top. (you may need to close the green success message)

![alt text][appManagerAutoscalerEnabled]

## Review the Changes
1. Go back to the AppManager window and go to the fortune-teller-services app home page, by clicking the left 'Development' link and then the 'fortune-teller-services' link.

![alt text][appManagerFTServiceHome]

2. Look at what we did! The current instance count is 3 (because that was our minimum), and if CPU utilization on any of the 3 instances goes above 80% another instance will be added automatically. Yeah!


___

___
| **[Prev Lab](../Lab-02/README.md)** |_______________________________________________________________________________| **[Next Lab](../Lab-04/README.md)** |