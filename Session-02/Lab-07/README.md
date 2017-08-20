[vsCodeFortuneControllerCs]: img/vsCodeFortuneControllerCs.png " "
[vsCodeAppSettingsCs]: img/vsCodeAppSettingsCs.png " "
[vsCodeStartupCs]: img/vsCodeStartupCs.png " "
[vsCodeManifestCs]: img/vsCodeManifestCs.png " "
[spaceGhost]: img/spaceghost_0.jpg ""

# Lab 07 - Refactor DotNet Core for Service Discovery

## Update AppSettings With Eureka
1. Within the Fortune Teller service app, go to the /Services/appsettings.json and update the following:
[Just after the closing bracket for the "Logging" node, add the below json]
[!!Remember to replace THE_URL_OF_EUREKA with the actual URL (AppManager>my space>Service tab>Service Registry>Manage>Server URL)]
```
,"spring": {
    "application": {
      "name": "fortuneTellerService"
    }
  },
  "eureka": {
    "client": {
      "serviceUrl": "<THE_URL_OF_EUREKA>",
      "shouldRegisterWithEureka": false,
      "validate_certificates": false
    }
  }
```
2. Your file should look like this:

![alt text][vsCodeAppSettingsCs]
## Add SteelToe Dependencies and Spring Cloud Discovery
1. Within the Fortune Teller service app, go to the /Services/Startup.cs file and update the following:
[Add the dependencies]
```
using Pivotal.Discovery.Client;
```
[Add the following line to ConfigureServices method]
```
services.AddDiscoveryClient(Configuration);
```
[Add the following line to the Configure method]
```
app.UseDiscoveryClient();
```
2. Your file should look like this:

![alt text][vsCodeStartupCs]
## Update Manifest With Spring Cloud Services
1. Within the Fortune Teller service app, go to the manifest.yml file and update the following:
[Add Spring Cloud services binding]
[!!Remember to replace NAME_OF_YOUR_SPRING_CLOUD_SERVICE with the actual service name]
```
  services:
    - mysql-100mb
    - <NAME_OF_YOUR_SPRING_CLOUD_SERVICE>
```
2. Your file should look like this:

![alt text][vsCodeManifestCs]
## About This
We have now told the app where to find the Eureka discovery server, configured it to only discover services and not register services. Once the app has started, the Discovery client will begin to operate in the background; both registering services and periodically fetching the service registry from the server. The simplest way of using the registry to lookup services is to use the Steeltoe DiscoveryHttpClientHandler together with a System.Net.Http.HttpClient.

Further reading and examples are available at: http://steeltoe.io/docs/steeltoe-discovery/

## Consume Discovered Service
1. Within the Fortune Teller service app, go to the /Services/Controllers/FortuneController.cs file and update the following:
[Add the SteelToe and Json dependency]
```
using Pivotal.Discovery.Client;
using Newtonsoft.Json;
```
[Within the FortuneController class, as a class wide variable add the discovery handler]
```
DiscoveryHttpClientHandler _handler;
```
[Refactor the FortuneController constructor method]
```
public FortuneController(IFortuneTeller iFortuneTeller, IDiscoveryClient client ){
      _iFortuneTeller = iFortuneTeller ;
      _handler = new DiscoveryHttpClientHandler(client);
  }
```
[Refactor the GetRandom method to use discovered services]
```
public Fortune GetRandom(){
	//return _iFortuneTeller.GetRandom();
  var client = new System.Net.Http.HttpClient(_handler, false);
  //get the random fortune
  var task = client.GetStringAsync("https://fortunes/random");
  task.Wait();

  var result = task.Result;

  Fortune f = JsonConvert.DeserializeObject<Fortune>(result);
  return f;
}
```
2. Your file should look like this:

![alt text][vsCodeFortuneControllerCs]

## About This
The discovered service is asynchronous but our endpoint is synchronous, so we used a blocking task to complete the request. Notice the format of the Http request "https://fortunes/random". If you break this down, the "fortunes" piece matches the name of the app that was registered in the Service Registry broker (AppManager>my space>Services tab>Service Registry>Manage). The "random" piece matched the endpoint address within the registered service.

## Push The App
1. Open a Terminal (or command prompt) and navigate to the app directory.
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

## View The App
1. In AppManager click the <STUDENT-X> space in the left box
2. The column labeled 'Route' will offer a link to execute the app. Click the route for the 'fortune-teller-www' app.
3. A new tab will be created, loading the FortuneTeller app web site
4. The site is now using SteelToe and Spring Cloud Services to get a random fontune from a Java microservice.


![alt text][spaceGhost]



___

___
| **[Prev Lab](../Lab-06/README.md)** |_______________________________________________________________________________| **[Finished](../../README.md)** |