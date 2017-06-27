# Re-envision build execution with the Gradle Worker API

- Paul Merlin
- Gary Hale

#

- in-process
- out-of-process (daemons)
- isolation modes

## Less Work

- classloaders
- exec/javaexec
  - spawning processes
- caching

### Worker Daemons

- reuse jvm process
- JavaExec spawns new JVM
- relegate GC impact to separate JVM

## More Work in Parallel

- fill "empty space"
- divide and conquer
- immutability

### Worker Threads

## Isolation Modes

- None
  - worker thread
- Classloader
  - worker thread
  - use different library versions
- Process
  - worker daemon
  - different environment variables
  - system properties
  - java version

Immutable work items enable "parallel by default"

# Case Study

Paul

https://github.com/eskatos/gradle-wsdl-plugin

@Inject - required on constructor
WorkerExecutor getWorkerExecutor()

workerExecutor.submit(class) { config ->
    config.isolatinoMode = IsolationMode.PROCESS
    config.forkOptions { systemProperty() }

}

# Current Status 4.0

- in-process, out-of-process
- daemons are build session scoped
- supports runnable

# Future 4.1+

- daemons reused 4.1-M1
- control reuse level
- gradle tasks use workers
- worker API convenience task
- service injection
- support for cllable
- composition and chaining of work
- in-process workers use cached classloaders


# Exec Task

- use None isolation mode
- get some parallelism
- wait for service injection
