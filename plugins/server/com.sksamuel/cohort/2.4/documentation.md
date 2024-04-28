Enabling health checks and actuator endpoints.

## Usage
```kotlin

    // example health check registry with a single health check
    // see https://github.com/sksamuel/cohort for all health checks
    val checks = HealthCheckRegistry(Dispatchers.Default) {
      // detects if threads are mutually blocked on each others locks
      register(ThreadDeadlockHealthCheck(), 1.minutes)
    }

    // adds the cohort plugin for ktor
    install(Cohort) {
    
      // enable an endpoint to display operating system name and version
      operatingSystem = true
    
      // enable runtime JVM information such as vm options and vendor name
      jvmInfo = true
    
      // show current system properties
      sysprops = true
    
      // enable an endpoint to dump the heap in hprof format
      heapdump = true
    
      // enable an endpoint to dump threads
      threaddump = true
    
      // enable health checks for kubernetes
      // each of these is optional and can map to any healthcheck url you wish
      // for example if you just want a single health endpoint, you could use /health
      healthcheck("/liveness", checks)
      healthcheck("/readiness", checks)
      healthcheck("/startup", checks)
    }
```