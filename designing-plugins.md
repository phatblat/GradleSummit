# Designing and writing Gradle plugins

Benjamin Muschko

# Language

- prefer statically-typed language
  - java
  - kotlin
  - groovy
    - binary incompatibility
      - issues with different versions of groovy
    - `@CompileStatic`

- test code is fine to use groovy

# Public API

dependencies {
  compile gradleApi()
}

- use only public API if possible
- caution
  - dependency pulls in full gradle runtime API
- docs are final reference on "public" API

# Dependencies

- minimize external dependencies
- guava?

## Identifying External Dependencies

```
gradle buildEnvironment
```

- full classpath of build script

# Performance Impact

- could become expensive
- impact to configuration phase


## Guide

- https://guides.gradle.org/performance

# Dig Deep with the Gradle Profiler

- https://github.com/gradle/gradle-profiler

# Patterns

## Convention Over Configuration

- gradle site plugin
  - generates website of plugin info
  - good example of plugin
- set default values in plugin
  - not extension

```
site {
  outputDir = file "$buildDir/site" // overrides defaults
}
```

## Capabilities vs. Conventions

### Capabilities

- general purpose concepts
  - e.g. sourceSets
  - JavaBase plugin
  - custom tasks, not instantiated

### Conventions

- java plugin
- applies capabilities from java base

# Use plugin development plugin

- dependencies.compile 'java-gradle-plugin'
- sets gradle API dependency
- reduces boilerplate code significantly
- integration with TestKit

# Prefer writing custom task types

- over ad hoc DSL tasks

# Avoid Closures in API


```
task generateSiteWithCustomValues(type: SiteGenerate) {
  customData {
    ...
  }
}

public class SiteGenerate extends DefaultTask {
private final CustomData customData = new CustomData();

public void customData(Action<? super CustomData> action) {
 action.execute(customData);  }
}
```

- Action gets converted to Closure
- internal class may become public to handle deep nesting
- 2nd level would need closure

# Declare Inputs & Outputs

- Use annotations
  - @Nested
  - @OutputDirectory

# PropertyState

- lazy evaluation of properties
- build.gradle
  - extension
    - task
- capture values during config phase
- evaluate during execution

# Avoid Applying Plugins if Possible

- Imposes potentially unnecessary conven$ons

# React to applied plugins

```java


project.getPlugins().withType(JavaPlugin.class,
      new Action<JavaPlugin>() {
  @Override
  public void execute(JavaPlugin javaPlugin) {
    JavaPluginConvention javaConvention = project.getConvention()↵
        .getPlugin(JavaPluginConvention.class);
  projectDescriptor.setJavaProject(
    new JavaProjectDescriptor(
      javaConvention.getSourceCompatibility().toString(),
      javaConvention.getTargetCompatibility().toString()));
    }
});
```

# Plugin IDs

- Add domain to ensure uniqueness
- Create properties file per plugin

# Roadmap

- top 10 internal APIs

# Resources


- [Designing Gradle plugins](https://guides.gradle.org/designing-gradle-plugins/)
- [Implementng Gradle plugins](https://guides.gradle.org/implemen,ng-gradle-plugins/)
- [Gradle Site Plugin](https://github.com/gradle-guides/gradle-site-plugin/)


