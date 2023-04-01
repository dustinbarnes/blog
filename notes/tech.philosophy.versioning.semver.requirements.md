---
id: requirements-for-semver
title: SemVer Requirements
desc: ''
updated: 1679957306305
created: 1679954518072
---
# Requirements
There are a few things that must be done if a project or team plans to adopt SemVer

## Requirement 1: Public API
SemVer is very clear that you must declare a public API. However, the SemVer spec itself is relatively vague on what this means, in item 1 of the specification:

> Software using Semantic Versioning MUST declare a public API. This API could be declared in the code itself or exist strictly in documentation. However it is done, it SHOULD be precise and comprehensive.

Note the purposeful use of MUST and SHOULD in these cases. You MUST declare a public API, but it only SHOULD be precise and comprehensive. Relaxing this requirement breaks all of SemVer, in this author's opinion. 

Then, item 5 in the spec comes along: 

> Version 1.0.0 defines the public API. The way in which the version number is incremented after this release is dependent on this public API and how it changes.

If we have an imprecise and non-comprehensive documentation suite, there is still a chasm of variability in what would be considered breaking or not. 

## Requirement 2: Not 0-version
The SemVer spec in item 5 is clear that 1.0.0 is where you determine how to bump the version for future changes. So long as you keep the `MAJOR` part set to `0`, you're free to do anything you want, including breaking downstream users. 

## Requirement 3: Bumping MAJOR
You MUST bump the major version when ANY breaking change is introduced to the public API. Yes, ANY breaking change. 

