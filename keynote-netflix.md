# Dependencies, distributed code and engineering velocity

Mike McGarr
Netflix

## Shaared Code

netflix ribbon httpclient

client "library" pulls in a set of dependencies
makes consuming libraries easy?

software isn't static

semantic versioning is insufficient
- value is about intent at release
- time moves on
- things happen

pinning versions is technical debt
no lifecycle to manage dependencies
consuming libraries is hard
at least publishing libraries is easy

is upgrading a transitive dependency a patch/minor/major version change?
who will I break?
who is consuming my library?
who is using this API that I want to change?

publishers lack organization-wide visibility

share less?
be cognizent of the use

# Monorepo

- single repo for all code
- source integration
- head is production
- no versions
- atomic API changes
- lock step deployments

## Constraints

- depends on gates
- distrust

netflix freedom & responsibility
- monorepo antithetical to their culture

# Approach

- benefits of a monorepo without the constraints

## Publisher Feedback

- know your consumers
- Astrid
  - gains dependency graph knowledge
  - answer via API call
- Niagra
  - give feedback on library
  - run build with new library version
  - runs previous version on fail
  - generate aggregate report

## Managed Source

- managed versions
- reduce pain of consumption
- Niagra triggers version bump in all consumers if all checks pass
- shortens adoption curve
- give me a version that works

## Distributed Refactoring

- 2016 Gradle Summit talk by jon schneider
  - Gradle int
- api owner: how do I change my API across hundreds of repos?

eventually consistent distributed monorepo
