[vsCodeStartupCs]: img/vsCodeStartupCs.png " "
[vsCodeFortuneControllerCs]: img/vsCodeFortuneControllerCs.png " "
[vsCodeFortuneCs]: img/vsCodeFortuneCs.png " "



# Lab 01 - Clone Source and Push Apps

## Clone Source

1. Download the .NET app source code, by cloning Git Repo or download the zip
```

> git clone https://github.com/jennymclaughlin/DotNetAttendees

```
2. Download the .NET Core app source code, by cloning Git Repo or download the zip
```

> git clone https://github.com/jennymclaughlin/dotnetcoreCFdemo

```


## Push The .NET App
1. Open command prompt and navigate to the app directory.
```
> cd ~\pcf-asp.net-mvc-attendees\PivotalWorkshop
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
> cf push <dotnetappName>-studentX -s windows2012R2 
```
4. The cf cli will provide feedback about each step it takes to create the App Container and deploy.

## View The .NET App
1. Verify your app in the apps manager
2. Question: What buildpack does it use?

## Use App Manifest for pushing the .NET app (optional)
1. You can also use the manifest file to push. Please include the following information:
---
applications:
- name: workshop-attendees
  host: attendees-studentX
  stack: windows2012R2

**Remember to change host value for each app. A requirement of PCF is, no 2 apps can have the same host name.

2. push again
> cf push

## Push The .NET Core App
1. Open command prompt and navigate to the app directory.
```
> cd ~\dotnetcoreCFdemo
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
> cf push <appName>-studentX 
```
** No need to specifiy the windows stack

4. The cf cli will provide feedback about each step it takes to create the App Container and deploy.

## View The .NET Core App
1. Verify your app in the apps manager
2. Question: What buildpack does it use?


___

___
| **[Prev Lab](../AppMgr-Login/README.md)** |_______________________________________________________________________________| **[Next Lab](../Lab-02/README.md)** |
