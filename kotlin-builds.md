# Powering-up your builds with Kotlin

Nadav Cohen
- Netflix
- Nebula
- groovy kon

 Rodrigo B. de Oliveira
- Gradle, project lead Kotlin

# Groovy

- params implicitly converted to Map
- Closures 1st class citizens

## Delegates

- can be altered
- basically, change what `this` means

## Metaprogramming

- methodMissing 'methodName', args: []

Gradle DSL is AST Transformation

# Kotlin

- closure -> Lambda
- full IDE support
- extension definitions are available at build time
- API-defined delegation
- don't need to rely on documentation
- can dive into source
- docs in markdown
  - F1 to view docs/context

## Tasks

// `by` gives an identifier
val hello by tasks.creating {
  doLast {...}
}

val hello by tasks.creating(Copy::class) {
  from()
  into(buildDir)
}

- Uses Action instead of Closure
  - SAM
  - `this` is value of string
- strongly typed task dependencies

# Domain Object Collections

## Groovy

- configurations {}
- tasks.configure {}
- different APIs

## Kotlin

- same API

```
// ConfigurationContainer NamedDomainObjectContainer<Configuration>
configurations {
  "myConfiguration" { // will create if not there
    isVisible = false
  }
}

// TaskContainer (Polymorphic) NamedDomainObjectContainer<Task>
tasks {
  val hello by creating { // explicitly create element, fail if already there
    doLast {}
  }
}

tasks {
  val hello by getting // explicit reference, fail if not there
}

- invoke operator overloaded
- uniform configuration
- explicit collection operation semantics


# Extensions vs. Conventions

- conventions deprecated soon

## Groovy

- extensions add new names
- java uses convention mechanism
- maven-publish uses extension mechanism

## Kotlin

- both conventions and extensions share same namspacing mechanism
  - java {}
  - publishing {}
- uniform access
- feedback at author time
- dash is not valid char in identifier
  - `maven-publish`

# Legacy APIs

- dual names
- groovy needs method that takes Action/Closure

```kotlin
plugins {
  java
  `maven-publish`
  id("com.jfrog.artifactory") version "4.1.1"
}

publishing {
  // PublicationContainer is domain object
  // parens disambiguates expression as property
  (publications) {
    "mavenJava"(MavenPublication::class) {
      from(components["java"])
    }
  }
}
```

- plugins expecing groovy closures
  - delegateClosureOf<GroovyObject>
  - groovy interop
  - adapter
    - Kotlin function to Groovy Closure
    - argument will be closure delegate
- can't use property syntax
  - setPropertyName("value")
  - caused by lack of getter
  - no write-ony properties now in Kotlin
  - also happens when setter/getter don't agree on type
- not a problem if plugin uses Action

https://gitlab.com/gradle-summit-2017-kts
@rodrigoBamboo
@nadavc