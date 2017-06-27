# Next-level performance with Gradle's Build Cache

## Incremental Build

Designed for

- local dev
- one change at a time

### How

- inputs, outputs
- dev gives metadata with @Input*, @output

### Limitations

- local
- optimized for incremental changes
- "best-effort"
- undermined by clean builds

## Build Cache

- semi-permanente storage
- enabled by `--build-cache`
- task output caching
  -

### Local

useful when:

- working on branches
- git bisect

### Shared

- HTTP build cache
- gradle enterprise
- reuse outputs
  - between changesets
  - CI agents
  - CI jobs
- no need to rebuild other people's changes

## Sharing Strategies

- only CI pushes (recommended)
- both CI and developers push (not recommended)

## Measuring is Hard

- Gradle core
  - ~8% faster
    - **total time?**
  - 167 builds
  - slow connections
  - geographically diverse
- Gradle CI
  - build
    - 42 hours saved
    - 20+ builds
  - integ tests
    - 63 hours saved
    - 30+ builds

## Challenges

- cacheing uses same metadata* as incremental build
  - can be faulty
- more permanent
  - no clean to fix problems

### 3rd Party Plugins

- more warnings
- enforce good practices
- disable when unsafe
- later: isolated execution

# Roadmap

- opt-in
  - `@CacheableTask`
