[appManagerFTServiceHome]: img/appManagerFTServiceHome.png " "
[appManagerFtServiceRoute]: img/appManagerFtServiceRoute.png " "
[appManagerAutoScaleBoundApps]: img/appManagerAutoScaleBoundApps.png " "
[appManagerFTServiceV2Route]: img/appManagerFTServiceV2Route.png " "
[appManagerFTServiceV2AddRoute]: img/appManagerFTServiceV2AddRoute.png " "
[appManagerFTServiceV2RouteNew]: img/appManagerFTServiceV2RouteNew.png " "
[appManagerFTServiceV2Home]: img/appManagerFTServiceV2Home.png " "
[appManagerFtServiceStopped]: img/appManagerFtServiceStopped.png " "

# Lab 04 - Zero Downtime Deployment

## Update App Manifest
1. Open the /manifest.yml file and change the following:
```
name: fortune-teller-services-version2
host: fortunetellerservice_<STUDENT_X>_version2
```
2. Save your changes

## Push Version 2 App
1. Open a Terminal (or command prompt) and navigate to the app directory
```
> cd ~/DotNet-Core-SteelToe-Workshop/FortuneTeller
```
2. Confirm the API target is set
```
> cf target

API endpoint:   <PROVIDED_BY_INSTRUCTOR>
User:           <STUDENT-X>
Org:            Vantage
Space:          <STUDENT-X>
```
3. Push the app
```
> cf push
```
4. The cf cli will provide feedback about each step it takes to create the App Container and deploy

## About This
Currently we have 2 apps running for Fortune Teller Services, the original app which has a URL prefix of fortunetellerservice and the version 2 app which has a URL prefix of fortunetellerservice_version2. The fortune-teller-www app is consuming the original app's services. Now we need to transition the available service over to version 2, with no downtime. To do this we will use PCF's dynamic routing and scaling capabilities.

## Remove Autoscaling from Original fortune-teller-services App
### Disassociate the autoscaling service with the original app, so we can manually manipulate instance count.
1. Click the '<student-x>' space on the left, then click the 'Services' tab, then click the 'App Autoscaler' service, and finally click the 'Edit Bindings' button. This will show a list of all available apps that are/could be associated with the Autoscaler service.

![alt text][appManagerAutoScaleBoundApps]

2. Uncheck the fortune-teller-servces app and click 'Save'. Note this will not return the app back to 1 instance, the fortune-teller-servces app will remain at 3 instances even though it is not associated with Autoscaling.

## Reroute Some Traffic to Version 2
### Send a portion of the fortune-teller-servces app traffic to version 2
1. Add the original apps' route to the version 2 app, by clicking '<student-x>' space on the left, then fortune-teller-servces-version2 app, then the 'Route' tab.

![alt text][appManagerFTServiceV2Route]

2. Click the blue 'Map a Route' button, fill in the Hostname of the original app's route: fortunetellerservice_<student_x>, and click the blue 'Map' button.

![alt text][appManagerFTServiceV2AddRoute]

3. The fortune-teller-servces-version2 app's route should look like this:

![alt text][appManagerFTServiceV2RouteNew]

## About This
We now have the route with URL prefix 'fortunetellerservice' going to 2 different apps. The amount of traffic going to each app is proportionate to the number of instances for each app. The original app fortune-teller-services has 3 instances, while the fortune-teller-services-version2 has only 1 instance. Therefore ~70% of the traffic will continue to go to the original app and ~30% will go to the version 2 app.

## Associate the Version 2 with Autoscaling
1. Remembering your new skills for autoscaling an app; go to the <student-x> space home page, choose Services, and bind the Autoscaling service to the fortune-teller-services-version2 app.
2. Click the 'manage' link for the Autoscaler service and set a minimum of 3 maximum of 5 instances, and enable the service.

## About This
Both the old fortune-teller-services and new fortune-teller-services-version2 should have 3 instances. That means each app is getting 50% of the overall traffic from fortune-teller-www app.

## Turn Off the Old App
1. Open a Terminal (or command prompt) and navigate to the app directory
```
> cd ~/DotNet-Core-SteelToe-Workshop/FortuneTeller
```
2. Confirm the API target is set
```
> cf target

API endpoint:   <PROVIDED_BY_INSTRUCTOR>
User:           <student-x>
Org:            Student01
Space:          <student-x>
```
3. Change the instance count for the fortune-teller-services app to 1
```
> cf scale fortune-teller-services -i 1
```
4. Change the instance count to 0
```
> cf scale fortune-teller-services -i 0
```
5. Stop the fortune-teller-services app from running, by going to the fortune-teller-services home page and click the blue stop button at the top. (next to the app's name)

![alt text][appManagerFtServiceStopped]

## A Little Cleanup
You can optionally remove the alternate routing for the version 2 app. Go to the 'Routes' page of the fortune-teller-services-version2 app and click the red 'X' next to the route having URL prefix 'fortunetellerservice_<student_x>_version2'.


___

___
| **[Prev Lab](../Lab-04/README.md)** |_______________________________________________________________________________| **[Finished](../../README.md)** |