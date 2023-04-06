---
id: 12fa-05-build-release-run
title: Build Release Run
desc: ''
updated: 1680547860152
created: 1680547687522
---
> Strictly separate build and run stages

- Three stages:
    - Build is a transform of source code into an executable bundle called a build. Using a specified commit, this stage fetches dependencies and compiles binaries/assets.
    - Release takes the build produced above and combines it with a deploy's Configuration. The release contains both the build and the config, ready for immediate execution.
    - Run runs the app in an execution environment by launching processes against a selected release.
  
  - Releases should be uniquely identifiable from a timestamp or version identifier.
    - Releases are an append-only ledger and a release cannot be mutated once it is created. Any change must create a new release.
- Shift left: the run stage should be kept as simple as possible to ensure engineers aren't required for recovery during unsociable hours.
