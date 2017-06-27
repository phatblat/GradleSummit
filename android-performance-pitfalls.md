# Android Performance Pitfalls

- make local development fast
- use flavors or properties
- legacy multi-dex is slow
  - use native multi-dex
- disable PNG crunching
- include minimal resources
  - resource merging is expensive
- disable unique ids and timestamps
  - unique ids are the base of local dev perf
- Use instant run
- maximise incrementalit
  - agp 3.0 get for free
  - compile avoidance
  - incremental compile
  - don't use annotation procesors
- `--parallel` and small, decoupled projects
  - decoupled: tasks don't influence each other

## Memory

- provide enough memory
- don't go overboard
- use forking for big compile tasks
- dex will soon be in its own daemon

## Caching
- `org.gradle.caching=true`
- local build cache speeds up branch switching

- insights with builds scans
- measure with gradle profiler
