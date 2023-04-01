---
id: software-versioning
title: Versioning
desc: ''
updated: 1679953586732
created: 1679892074530
---

# What is the Purpose a Software Version?
The only actual, functional requirement of a version is that it uniquely identifies a build of software. 

That's all a version does. Everything else is folks applying their comfort levels and background to how it should be done. 

## What a Version Does Not Necessarily Do

### Tell which version is newer

This is not a requirement of a version. It's simply a practice that most people have adopted out of convenience and convention. When you're trying to figure out what version scheme to adopt, you'll likely make this a point of consideration. 

You might choose to drop this convention in CI/CD pipelines, as the git commit ref can often suffice as the version. 

## Tuple of dot-joined numbers

This is a convention of the industry, but not required. Some tools version based on codenames, and most of the industry has accepted some postfix string to further classify builds (-alpha, -beta, rc1, etc)

Then, you have to consider if you're zero-padding, how many items to have (3 is conventional, 2 and 4 are not uncommon). 


# Some of the options
1. [[tech.philosophy.versioning.semver]]
1. CalVer
1. ZeroVer/0ver
1. SentimentalVer
1. SemVer-ish

# Version Differences Between Libraries and Services
- services should generally use calver
- libraries should use SemVer-ish