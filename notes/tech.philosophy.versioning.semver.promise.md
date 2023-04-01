---
id: promise-of-semver
title: Promise of SemVer
desc: ''
updated: 1679954291672
created: 1679953610669
---

c.f. https://semver.org

> Given a version number `MAJOR.MINOR.PATCH`, increment the:
> 1. `MAJOR` version when you make incompatible API changes
> 2. `MINOR` version when you add functionality in a backwards compatible manner
> 3. `PATCH` version when you make backwards compatible bug fixes
>
> Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

The promise is that so long as `MAJOR` doesn't change, nothing will break and you can accept the new version without issue. The special exception is when `MAJOR` is `0`.