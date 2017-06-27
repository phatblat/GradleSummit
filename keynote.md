# Gradle Summit keynote

600 devs 1m of build time work $1 million per year

# Dev Productivity pillars

- fast feedback cycles
- high degree of automation
- faster debugging
- productivity culture

# Dependency Resolution

- faster in 4.0
- Netflix partnered with G on this feature
- one build went from 13m to 2m

# Build Cache

- Android plugin 3.0
- off by default
- on by default in 4.2?
- https://build-cache-node1-demo.gradle.com
- Don't need gradle enterprise to use remote build cache

# Gradle Enterprise
https://enterprise-demo.gradle.com/cache/
- /cache-admin/
- 2017.4
- docker image

# Worker API

- replaces `--parallel`

## worker daemons

- long-lived process
- replace javaExec
- maximize resources
- parallel by default

```groovy
workderExecutor.submit(WsImportRunnable.class) {conig -> ... 
    config.isolationMode = .PROCESS, .CLASSLOADER, .NONE
    config.classpath ...
    config.params = (constructor parameters)
}
```

https://eskatos.github.io/gradle-wsdl-plugin

# Roadmap

- worker API for core plugins
  - enabled by default
- incremental compile w/ annotation processors
- cacheable scala and c/c++ tasks
- faster config time
- N-level build caches
    - use S3 or gogole cloud for cache
- multi-location support for cache backend
- cache bandwidth sensitivity

need to be able to see build performance for all builds

# Build Reliability

- reliable parallelism
- cache is a forcing function
- devs would rather waste time in clean builds for less dimensions of uncertainty
- build scan: no need to reproduce, because it's recorded

# Build Scan

- navigate from a cache hit to the task source that populated the cache
- timeline: view non-cacheable tasks
  - opportunities for performance improvement by adding cacheability

