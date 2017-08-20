[appmanager-service-registry]: img/appmanager-service-registry.png " "
[vsCodeStartupCs]: img/vsCodeStartupCs.png " "

# Lab 05 - Refactor DotNet Core with MySql Abstractions

## Change MySql Connection
1. Within the Fortune Teller service app, go to the /Services/Startup.cs file and update the following:
[Add dependencies]
```
using Steeltoe.Extensions.Configuration;
using Steeltoe.CloudFoundry.Connector.MySql.EFCore;
```
[Update the builder configuration to include CloudFoundry service]
```
var builder = new ConfigurationBuilder()
				.SetBasePath(env.ContentRootPath)
				.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
				.AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
-->			.AddCloudFoundry()
				.AddEnvironmentVariables();
```
[Remove the below code]
```
//Using MySql Datastore
string connString = "";

try{
	dynamic vcap = JObject.Parse(Environment.GetEnvironmentVariable("VCAP_SERVICES"));
	connString = String.Format("Server={0};port={1};Database={2};uid={3};pwd={4};",
		vcap["p-mysql"][0].credentials.hostname,
		vcap["p-mysql"][0].credentials.port,
		vcap["p-mysql"][0].credentials.name,
		vcap["p-mysql"][0].credentials.username,
		vcap["p-mysql"][0].credentials.password);
}catch(Exception ex){
	Console.Error.WriteLine("Error retrieving database connection string from environment variables");
	Console.Error.WriteLine(ex);
	Environment.Exit(1);
}

services.AddDbContext<FortuneTellerContext>(opt => opt.UseMySQL(connString));
```
[Add the new new MySql connection]
```
services.AddDbContext<FortuneTellerContext>(opt => opt.UseMySql(Configuration));
```
2. Save your changes
3. The Startup.cs file should look like this:

![alt text][vsCodeStartupCs]

## Push The App
1. Open a Terminal (or command prompt) and navigate to the app directory.
```
> cd ~/DotNet-Core-SteelToe-Workshop/FortuneTeller
```
2. Confirm the API target is set
```
> cf target

API endpoint:   <PROVIDED_BY_INSTRUCTOR>
User:           <student-x>
Org:            Vantage
Space:          <student-x>
```
3. Push the app
```
> cf push
```
4. The cf cli will provide feedback about each step it takes to create the App Container and deploy

## View The App
1. In AppManager click the '<student-x>' space in the left box
2. The column labeled 'Route' will offer a link to execute the app. Click the route for the 'fortune-teller-www' app.
3. A new tab will be created, loading the FortuneTeller app web site

![alt text][fortuneTellerWebSite]

4. How easy was that?! The app is using the same MySql database but not more connection string.


___

___
| **[Home](../../README.md)** |_______________________________________________________________________________| **[Next Lab](../Lab-06/README.md)** |