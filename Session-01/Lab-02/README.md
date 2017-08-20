[vsCodeStartupCs]: img/vsCodeStartupCs.png " "
[appManagerMarketplace]: img/appManagerMarketplace.png " "
[appManagerMySql]: img/appManagerMySql.png " "
[vsCodeManifest]: img/vsCodeManifest.png " "

# Lab 02 - Using MySql as Data Store

## Change App Data StoreTo MySql
1. Within the Fortune Teller service app, go to the /Services/Startup.cs file and update the following:
[Add the MySql dependency to the file]
```
using MySQL.Data.EntityFrameworkCore.Extensions;
```
[Comment out the InMemory data context within the ConfigureServices method]
```
//services.AddDbContext<FortuneTellerContext>(opt => opt.UseInMemoryDatabase());
```
[Add the ability to retrieve the VCAP_SERVICES environment variable. This variable will have connection string values. Within the ConfigureServices method]
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
```
[Add the MySql data context within the ConfigureServices method]
```
services.AddDbContext<FortuneTellerContext>(opt => opt.UseMySQL(connString));
```
[Add EntityFramework Core MySql workaround within the Configure method]
```
try{
	fortuneContext.Database.Migrate();
}catch(Exception){
	fortuneContext.Database.ExecuteSqlCommand("CREATE TABLE `__EFMigrationsHistory` ( `MigrationId` nvarchar(150) NOT NULL, `ProductVersion` nvarchar(32) NOT NULL, PRIMARY KEY (`MigrationId`) );");
	fortuneContext.Database.Migrate();
}
```
2. Save your changes
3. The Startup.cs file should look like this:
![alt text][vsCodeStartupCs]

## Bind MySql to App
1. In AppManager, click the Marketplace link in left box.
![alt text][appManagerMarketplace]
2. Choose the MySql service, and a capacity plan. In this case the 100mb plan will be used
![alt text][appManagerMySql]
3. Click the blue 'Select this plan' button, name the new instance as mysql-100mb, select the <STUDENT-X> space, select the fortune-teller-services app, and click the blue 'Add' button
![alt text][appManagerMySqlValues]

## Update App Manifest
1. Open the /manifest.yml file and add the following:
```
  services:
    - mysql-100mb
```
**Note the spacing, from the left margin there should be 2 spaces before "services" and 4 spaces before "- mysql-100mb"
2. Save your changes
3. The manifest.yml file should look like this:
![alt text][vsCodeManifest]

## Push The App
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
4. The cf cli will provide feedback about each step it takes to create the App Container and deploy.

## View The App
1. When you refresh the [already opened] Fortune Teller browser window, a new fortune should be shown. Yeah! You have changed the data store to MySql.


___

___
| **[Prev Lab](../Lab-01/README.md)** |_______________________________________________________________________________| **[Next Lab](../Lab-03/README.md)** |