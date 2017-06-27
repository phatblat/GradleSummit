# What's New with Gradle Enterprise

http://gradle.com/enterprise/releases/2017.4

## Build Cache

- docker: gradle/build-cache-node
- can be used standalone
- part of enterprise

## Build Scan

- publishAlways()

> Build scan data will not be captured due to this build being a Composite Build.

### Comparison

- success / fail different machines
- tabs show bade when different
  - inputs
  - dependencies
  - custom values
  - switches
  - infrastructure (environment)
- "Compare Prev/Next" in upper right nav
- transitive dependency changes visible
  - not visible from diff in build script

# Build Scan DSL

- tag
- Shows under build title header
- tags can be used to filter builds
    - `not:TAG`
- capture CI info
    - value
    - shows in "Custom Values" section near top
- Attach code quality issues as values

```
buildScan {
    link "Source", "https://..."
    if (...) {
        tag "CI"
    } else {
        tag "local"
    }
    value "Git commit", commitId
}

buildScan.value "Checkstyle Issue", issue
```

## Configuration Time

- can see each plugins apply time
- scripts split out

## Build Time Optimization

- `allprojects.tasks.shouldRunAfter longRunningTask`
  - use on long running tasks to request Gradle to move it up in task graph
- look at slowest tests
  - make long tests faster
  - **how to expose test time to build scan from opaque test task??**

## Cacheable

- Performance > TaskExecution tab
- all tasks > not cacheable
  - https://enterprise-demo.gradle.com/s/nqtgecv3wuese/timeline?cacheableFilter=ANY_REASON&outcomeFilter=SUCCESS&outcomeFilter=FAILED&search

# Export

- https://github.com/gradle/gradle-enterprise-export-api-samples
- Sam (Tableau) used this data export to build visualizations

# Roadmap

## Scan

- more cross build analysis
- resource usage insight
- finer grained perf insight
- dev oriented feedback

## Cache

- cache federation / distribution
- sophisticated cache topoligies
- transparent load balancing and fault tolerance
- cache fetch cost/benefit analysis

# Getting Started

- scans.gradle.com free node
- install cache node
