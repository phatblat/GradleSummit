# Extending the Gradle Android Plugin

EYAL LEZMY

nested extensions

```
project.genymotion.extensions.create "config", GenymotionConfig
```

```
genymotion {
  devices { // project.genymotion.devices(Closure c)
    Nexus5 {...} // Add "Nexus5" container
  }
}
```

```
class GenymotionPlugin implements Plugin<Project> {
  void apply(Project project) {
    def instantiator = project.gradle.services.get(Instantiator)
    // containerr
    def deviceLaunches = project.container(DeviceLaunch,
                    new DeviceLaunchFactory(instantiator))
    project.extensions.create(“genymotion”,
                    GenymotionExtension, project, deviceLaunches)

    // common, waits for build.gradle config to be read
    project.afterEvaluate {
      project.genymotion.injectTasks()
    }
}

class GenymotionExtension {
  NamedDomainObjectContainer<DeviceLaunch> deviceLaunches
  GenymotionExtension(Project project, deviceLaunches) {
    this.project = project
    this.deviceLaunches = deviceLaunches
}
  def devices(Closure closure) {
    deviceLaunches.configure(closure)
}

class DeviceLaunchFactory implements NamedDomainObjectFactory<DeviceLaunch> {
   final Instantiator instantiator
   public DeviceLaunchFactory(Instantiator instantiator) {
       this.instantiator = instantiator
}
   @Override
   DeviceLaunch create(String name) {
       return instantiator.newInstance(DeviceLaunch.class, name)
   }
}

// Model
class DeviceLaun
```


# Make it Intuitive

- methods > properties

# Make it Talkative

- no autocomplete
- no suggestion
- no integrated documentation
- log is yur voice
  - respect Gradle convention levels
- errorrs are part of the docs
  - anticipate the mistakes and deliver the appropriate explicit message

---

# Android Gradle Plugin

- android.applicationVariants
  - app plugin
- android.libraryVariants
  - library plugin
- android.testVariants
  - both

```
android.applicationVariants.all { variant ->
  // launches the generation of tasks
  // call after configurationevaluation
}
```

## Docs

http://tools.android.com/tech-docs/new0build-system-user-guide

- "manipulating tasks" section
- 30% wrong info
  - not up to date
  - good entry point
- source code 100% accurate

```
git clone https://android.google.source.com/platform/tools/base
git checkout tags/gradle_2.3.0
```

## Internals are changing a log

- avoid using explicit values
  - "connectedAndroidTest"
  - use dedicated properties

- 1.0.0
  - connectedAndroidTest
- 1.2.0
  - connectedAndroidTestDebug
- 1.3.0
  - connectedDebugAndroidTest

Integration tests for plugin compatibility

# Test

- Unit tests
  - no gradle API needed
- Integration tests
  - stub of project instance
  - ProjectBuilder
    - create project stub
    - canAddGenymotionExtension
      - apply plugin
      - project.evaluate()
        - internal method
        - supported mechanism in the near future (gradle)
          - luke daley
- Functional tests
  - execute a build for real
  - check results

## Integration Test Class

```groovy
@Test
@Category(Android)
public void canInjectToVariants() {
  project = getAndroidProject()
  project.android.productFlavors {
    flavor1
flavor2 }
  project.evaluate()

   project.android.testVariants.all { variant ->
    Task connectedTask = variant.connectedInstrumentTest
    assert connectedTask.getTaskDependencies().getDependencies()
         .contains(genymotionTask)
  }

Project project = ProjectBuilder.builder()
                .withProjectDir(new File("res/test/android-app"))
                .build()

project.apply plugin: 'com.android.application'
project.apply plugin: 'genymotion'

project.android {
   compileSdkVersion 21
   buildToolsVersion "21.1.2"
}
// create a project from a folder
```

# Compatibility

- test with several Android plugin versions
- control android plugin version from outside the project
- test with beta releases
  - set the default plugin vversion to '+'

```
def androidVersion = "+"
if (hasProperty("androidPluginVersion")) {
   androidVersion = androidPluginVersion
}
dependencies {
  testCompile 'junit:junit:4.12'
  testCompile "com.android.tools.build:gradle:$androidVersion"
}
./gradlew test -PandroidPluginVersion=2.3.0
```

## Drawback

- switch gradle version on the fly

```python
for gradle_version in gradle_versions:
  # Set the current project to the good Gradle version
  cmd = ['gradle', 'wrapper', '--gradle-version', gradle_version] check_call(cmd)
  for android_version in ANDROID_TO_GRADLE[gradle_version]:
    # Run the Android tests specifying the Android Plugin version cmd = ['gradle', 'test',
    check_call(cmd)
```

# Functional Tests

- TestKit
- create projects on the fly and build it
- check result of build
- test full chain
- choose gradle version from tests

## Limitation

- no access to build steps
- just result
- hack around this
- add the test into the build script to test


