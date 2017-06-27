# Identifying and addressing build performance problems

https://github./oehme/anaysing-gradle-performance

- use latest gradle version

# Red Flags

- startup > 1s
- config > 1s
  - < 100 projects
- single line change > 10s
- no-op build doing _any_ work at all

# measure with grade profiler

- configuration syntax
- configurationTime
- automated performance schenario

# Optimize Config Time

- Runs all the time
- don't do dependency resolution at config time
- might not need them
- shown on dep resolution tab on scans
- use lazy evaluation

# Inefficient Plugins

- reuse expensive calculations
- (or) only apply to root, plugin iterates over subprojects

# Incremental

- nothing changed
  - executed tasks should be zero!
- timestamp in manifest bad idea
  - make development the default build
  - only use timestampts on CI

# Incremental Tasks

- specify required inputs/outputs
- not more
- not less

## Talk

- Minutes to seconds: maximizing incrementality
- Cedric Champeau
- http://blog.gradle.org/incremental-compiler-avoidance

# Parallelism

- `--parallel` can only be used on decoupled projects
- gradle
  - ~40s
  - 28s in parallel
- test task
  - maxParallelForks = gradle.?.maxWorkerCount
  - forkEvery = 1000

# Plugins

- Use buildSrc for heavy lifting
- statically compile plugins
- don't use dynamic languages to write plugins
  - groovy: [@CompileStatic](http://docs.groovy-lang.org/latest/html/gapi/groovy/transform/CompileStatic.html)
- optimize on the method level
  - `gradle-profiler --profile jprofiler`


# performance issue

- attach JProfiler/YourKit snapshot to an issue
- performance@github.com

