# spring-boot-micrometer-datadog-quickstart
  
## What is it?

A Quickstart for Spring Boot and Micrometer with Datadog

## Dependencies

### JDK 13
https://openjdk.java.net/

Mac:
* https://www.macports.org/ports.php?by=name&substr=jdk
* https://github.com/AdoptOpenJDK/homebrew-openjdk

### Maven 3.5.4 or higher
https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/

### Datadog credentials
For this quickstart you will need an api-key and application-key for Datadog integration.  


## Build

### Maven

`
mvn clean install
`

### Run

#### spring boot

`
mvn clean package spring-boot:run -Dspring-boot.run.profiles=local
`

## Application Metrics

Starting Spring Boot 2, micro meter is the default metrics engine and support a number of integration ranging from Influx, prometheus, statsd, datadog etc. A full list of integration and more can be found under the following blog: 
https://spring.io/blog/2018/03/16/micrometer-spring-boot-2-s-new-application-metrics-collector

In this quickstart we will be looking at integrating with Datadog (with the Datadog API approach). More details can be found here:
https://micrometer.io/docs/registry/datadog

### Datadog properties

#### Required Properties

`management.metrics.export.datadog.api-key` Datadog API key.

`management.metrics.export.datadog.application-key` Datadog application key.Improve the Datadog experience by sending meter descriptions, types and basic units to Datadog.

#### Optional Properties 

`management.metrics.export.datadog.batch-size = 10000` The number of metrics per request for this backend. If more measurements are found, multiple requests will be made.

`management.metrics.export.datadog.connect-timeout = 1s` The connection timeout for this backend request.

`management.metrics.export.datadog.descriptions = true` whether to publish the description metadata to the Datadog. Turn it off to minimize the amount of metadata sent.

`management.metrics.export.datadog.enabled = true` Whether to enable exporting metrics to this backend.

`management.metrics.export.datadog.host-tag = instance` A flag that maps to the "host" when the metric is sent to the Datadog.

`management.metrics.export.datadog.num-threads = 2` Number of threads used by the indicator release scheduler.
 
`management.metrics.export.datadog.read-timeout = 10s` Read the timeout for this backend request.
 
`management.metrics.export.datadog.step = 1m` The step size to use (ie the reporting frequency).

`management.metrics.export.datadog.uri = https://app.datadoghq.com/` 

### Micrometer Properties


Micrometer and spring metrics have loads of custom configuration available. 

Below are some that are most commonly used for application performance monitoring that we will use in this example:

`server.tomcat.mbeanregistry.enabled=true` Enables additional tomcat metrics under the metrics endpoint.

`metrics.distribution.percentiles.http.server.requests=0.5, 0.95, 0.99 `  - Percentiles for the http requests

`metrics.distribution.percentiles-histogram.http.server.requests=true` Enables histogram for your http requests, broken down by URI

`metrics.distribution.sla.http.server.requests=200ms, 400ms, 600ms`  cumulative histogram with buckets defined by your SLAs.


#### tags for metrics

`management.metrics.tags.application`  A unique name identifier for your application. Usually configured to ${spring.application.name}

`management.metrics.tags.env` Environment label usually picked up from ${spring.profile}


### Actuator

`management.endpoints.web.exposure.include=*` Enables a bunch of endpoints including `/actuator/metrics`, `/actuator/health` etc.

A full list of endpoints can be found here: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints

