[appmanager-service-registry]: img/appmanager-service-registry.png " "

# Lab 06 - Service Discovery through Eureka

## Getting started

Start by downloading the course materials.  This can be accomplished either through the GitHub website or if you have Git installed, use the following commands:

```
$ git clone https://github.com/timmers-io/SpringCloudWorkshop.git
$ cd SpringCloudWorkshop/
$ git fetch --all
$ cd scs
```

## Create the required services for the fortune service

```
cf cs p-mysql 100mb fortunes-db

cf cs p-service-registry standard service-registry

cf cs p-circuit-breaker-dashboard standard circuit-breaker-dashboard

```

## Create the configuration server

On Windows:
```
cf cs p-config-server standard config-server -c "{\"git\": { \"uri\": \"https://github.com/spring-cloud-services-samples/fortune-teller\", \"searchPaths\": \"configuration\" } }"
```

On Mac OS:
```
cf cs p-config-server standard config-server -c '{"git": { "uri": "https://github.com/spring-cloud-services-samples/fortune-teller", "searchPaths": "configuration" } }'
```

Monitor the status of the service creation:

```
cf services
```

When all of the services have been created, push the Java fortune teller service and ui applications:

```
cf push -f manifest-pcf.yml
```

Use the 'cf apps' command and/or login to Apps Manager to get the URL for the fortune-ui. Open the URL in a browser and verify you are gettig random fortunes.

## You now have a Java service you can discover from your applications using service-discovery


___

___
| **[Prev Lab](../Lab-05/README.md)** |_______________________________________________________________________________| **[Next Lab](../Lab-07/README.md)** |