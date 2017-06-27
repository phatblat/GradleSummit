#  Correct and Flexible Native Builds... By Default

Daniel Lacasse
- @lacaseio
- https://github.com/lacasseio

native is driven using software model

workshop
- rule based configuration workshop
- info on rule engine
- task graph at config time

Oscar
- LinkedIn
- iOS

No slides online, took pictures

binaries.withType(NativeBinarySpec) {
  lib library: 'interface', linkage: 'api'
  lib library: '?', linkage: 'static'
}

- api is headers
- static

- $ is special syntax to use input to rule
  - binary container

# Automation Patterns

- modeling your build


# Roadmap

## Dep Mgmt

- trans deps
- remote dependency resolution
- artifact publishing to local/remote repos
  - 4.1 has model alignment?
- composite build support for multi-repo project

## Work Avoidance

- build cache for native
- better incremental build
- avoid static library creation when possible
- incremental static lib creation
  - reuse existing object files
- skip linking for non-interface changes
- improved build scan support

## Converging Both Models

- software model
    - rule engine
    - managed type
    - multiple component based configuration
  - capture pattern similar to all projects
  - perf goal
    - ordering isue
  - 2016 decision back port to current model
- ongoing work in 2017
- prefer DSL & task-based config over rule engine
- delay port of @Managed type support
  - later version of gradle
- opt for better variant support over multi-component project
- migration plan for software model users
  - caching
  - performance
  - composite build
- software all about config of build
  - everything else should stay the same
- provide clear added value through the new effort
  - static configuration initally
  - as config evolves, entire workflow will benefit
- help us by providing real life edge case samples
- follow [gradle/gradle-native](https://github.com/gradle/gradle-native) on GitHub

# Questions

- rules engine/software model will eventually be phased out
- java library plugin
- play framework still on software model
- will native dep mgmt cross over to other domains?
  - with variants
  - variant aware is already used in java, e.g. android
  - native will use same
  - already connected to both
  - go plugin uses?
- CPP template changed, how to trigger/detect incremental?
  - gradle parsing file to detect
  - right way to ask compiler
    - flags exist
    - not used yet
    - include tree
- GCC takes long time to detect things, would gradle use LLVM?
  - gradle would assume out of date on first invocation


# Videos

- [Rule Based Model Configuration Workshop Part 1](https://youtu.be/6Qsfbjtx3Is) 1:05
- [Rule Based Model Configuration Workshop Part 2](https://youtu.be/bKlPPWfqi5A) 1:00
- [The New Gradle Model](https://youtu.be/ukKkYp_sM38) 54:09
