---
id: 12fa-02-dependencies
title: Dependencies
desc: ''
updated: 1680513471728
created: 1680513158855
---
> Explicitly declare and isolate dependencies.

* Never relies on implicit existence of system-wide packages. 
    * It declares all dependencies, completely and exactly, via a dependency declaration manifest.

* Basically, use a dependency management tool. 
  * Java: Maven, Gradle
  * Node: npm, yarn
  * etc