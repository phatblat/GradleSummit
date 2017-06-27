# Multi-repo development with Composite Builds

existence of a settings.gradle file tells gradle that it's a separate build

task publishModules {
  dependsOn gradle.includedBuilds*.task(":publish")
}

build.identityPath
- shows full path within composite build
