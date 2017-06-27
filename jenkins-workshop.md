# Workshop: Use Gradle with the new Jenkins 2 to construct a Continuous Delivery Pipeline - please see details for required setup

https://summit.gradle.com/schedule


Brent Laster @BrentCLancaster
Senior Manager, SAS
Friday, June 23 - 9:25 AM
Room: MEDITERRANEAN III

In this workshop, attendees will learn how to use Gradle with Jenkins 2 to author pipelines in code and create a full stage CD pipeline. We'll cover how to utilize Git, Gradle, Jenkins 2 and integrate Gradle with Artifactory and other tools in the new Jenkins 2 pipeline-as-code world. Participants will create a functioning CD Pipeline while learning about how to integrate the key parts of these technologies for maximum productivity.

Since this is a hands-on workshop, some prep is required. We will be running a VM on VirtualBox. Prior to the workshop, attendees should download and install VirtualBox and ensure that it runs ok on a laptop that they can bring to the session. VirtualBox can be installed from https://www.virtualbox.org/

Additionally, a VM image, available for download from https://www.dropbox.com/s/i27thblashhf6vt/gsummit17.ova?dl=0 is also required.

After installing VirtualBox and downloading the image, please access and follow the setup instructions at
https://github.com/brentlaster/cdpjdocs/blob/master/gsummit17-setup.pdf
It is important to do the setup prior to the session if possible, since there will be limited time in the session.

## Continuous Delivery book

- Jez Humble
- David Farly

CD: proving that each change is "deployable" at any time
- environment changes should be under source control and trigger a new pipeline instance

# Jenkins 2

- Declarative pipeline DSL
- BlueOcean advanced interface
- still supports FreeStyle?
- automatic project creation based on Jenkinsfile presence in repo
- DSL syntax
  - scripted (groovy)
  - declarative
    - no imperative logic
    - direct Blue Ocean compatibility
    - predefined sections
    - better error procesing/reporting

## Multibranch Pipeline

- jobs created automatically based on Jenkinsfile presence in branches
- vary build by branch
- exclude branches? yes
  - branch sources > git > include/exclude branches
- shared pipeline libraries?

## GitHub Org

- moves granularity of job creation up to org repo level

# Shared Pipeline Libraries

- general mode (modern?)
  - any scm repo
  - external library
- legacy mode
  - git repo hosted by jenkins
  - internal library
- repo structure
  - src
    - classpath
  - vars
    - global variables or scripts
  - resources
- file.groovy
  - file() function
  - def call() is implementation function
- Need to be defined in Jenkins > Configure System > Global Pipeline Libraries
  - cloned automatically
  - usable by any script
  - trusted
    - put potentially unsafe methods in libraries here to encapsulate them
- folders can have shared libraries

## Versions

- @Library('Utilities')
- @Library('Utilities@1.4')
- @Library('Utilities@tag')
- @Library('Utilities@branch')
- libraryResource 'org/foobar/input.json
- @Grab('group:artifact:version')

def func = load ("...groovy")
func(...)

# Parallel

- DSL has `parallel` operation
  - takes map as arg
  - value is closure
- use nodes in closures if available to get best parallelism
- can't use stage stpe in parallel block
- build map first, pass to parallel
- add node blocks inside parallel

## Stash/Unstash

- use stash to save files and share with other nodes
  - use for small files

## Workspace

- Jenkins doesn't clean workspaces
- install [Workspace Cleanup Plugin](http://wiki.jenkins-ci.org/display/JENKINS/Workspace+Cleanup+Plugin)
    - step([$class: 'WsCleanup])

# Credentials

- Install [Credentials Binding Plugin](https://plugins.jenkins.io/credentials-binding)
- withCredentials([[$class: 'UsernamePasswordMultiBinding]])

```groovy
withCredentials([usernamePassword(credentialsId: 'amazon', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
  // available as an env variable, but will be masked if you try to print it out any which way
  sh 'echo $PASSWORD'
  // also available as a Groovy variable—note double quotes for string interpolation
  echo "$USERNAME"
}
```

## SSH Agent Plugin

- `sshagent`

- ${env.DBUSER}
- env. not needed
- $DBUSER

## Sonar

- Demo: nemo.sonarqube.com
- localhost:9000 in VM
- artifactory at :8081

### pipeline integration

- lab 5
  - Change DB env vars
- use webhook to tie into pipeline
- invoke scanner
- `withSonarQuebeEnv` dsl
- `waitForQualityGate()`

- running Sonar from gradle doesn't wait for quality gate
  - bug in jenkins
  - error: can't find project info


## Pipeline Flow Control

- Parameters -> `input` step
- `retry(5)`
- `sleep`
- `timeout(time: 600, unit: 'NANOSECONDS') {}`
- `waituntil`
  - waits until block returns true
  - multiplies time by 1.2 and tries again
  - wrap with timeout
  - catch exceptions, return false

${params.MAJOR_VERSION}

## Workflow Remote Loader Plugn

- fileLoader
- fromGit


# Artifactory

- not straight-forward
- especially with declarative pipelines
  - script block, added to decl
  - use shared library
- def server = Artifactory.server "<name>"

```
def artifactoryGradle = Artifactory.newGradleBuild()
with artifactoryGradle {
  .tool =
  .deployer repo: '', server server
  .resolver repo: '', server: server
}
```

# Docker

- cloud
  - slave jenkins agent on docker image
  - Docker Plugin (not pipeline)
- dsl run a command such as `inside`
  - Docker Pipeline Plugin
  - snippet generater: "...functions are not variables..."
    - other operations > global variable reference
  - anything run inside the "inside" step runs inside the docker contains
    - like prefixing with dockerexec
  - withDockerContainer()
    - essentially same as inside
- shell out to docker command

-----

# Labs

## Replay

- edit jenkins dsl and rerun job

## Testing Pipeline Scripts

- shared pipeline libraries
- [JenkinsPipelineUnit](https://github.com/lesfurets/JenkinsPipelineUnit)
- `mvn hpi:run` class with mocks?

------------------------


# Questions

- Move Jenkinsfile to central repo, how to trigger builds based on project change?
