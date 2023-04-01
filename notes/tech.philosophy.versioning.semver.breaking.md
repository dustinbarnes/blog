---
id: g19cobmk3lnn50wk095xqeg
title: SemVer Breaking Changes
desc: ''
updated: 1680034919517
created: 1679957276085
---

# What is a Breaking Change?
This seems easy, but what does "breaking" mean? [[tech.philosophy.laws.hyrum]] says that at some usage level, any behavior in your system is depended upon by downstream users. 

This means that, if you are doing active work to your project, you'll quickly be in double- or triple-digits for your major version. Some may say this means a poor up-front design, but it ignores some real-world concerns. Let's walk through some examples:

## Behavioral Change
Initial code:

```
function doThing(text) {
    return text.replace('<>', 'Your Name Here');
}
```

New code:

```
function doThing(text) {
    return text
      .replace('<>', 'Your Name Here')
      .replace('[]', 'Your Address Here');
}
```

Contractually and by the public API, nothing has changed. It's still a `f(string) => string` function. However, users that depended upon '[]' passing through unfiltered will now experience a break.

This is a breaking change, and requires a major version bump -- unless you explicitly documented that things like `[]` or `()` are future reserved tokens. 

## Performance Changes
If you introduce a change where you have the same function, with the same shape, but the previous version executed in O(1) time, and the new version executes in O(N) time.

Without any changes to the API interface itself, you're introducing a potentially breaking change. 

## Automated Testing
Your automated test are the closest most teams have to Consumer Contract Testing. Any time a test breaks, it indicates a behavior change. Ergo, any time you fix a test, that's a major version bump. 

By changing the behavior of your contract (say, by fixing a bug) and updating your automated tests (to ensure new behavior persists, and to prevent regression), you are establishing a break to your API. 

## They're All Potentially Breaking!
There's a very strong argument to be made that every change to the system is a breaking change. I know it sounds like pedantic wordplay, but we're talking about a versioning scheme whose primary purpose is to allow the automatic and error-free update of dependency upgrades (and their transitive dependencies). 

So long as a human is hedging the definition of a breaking change or staying at ZeroVer or somehow putting their fingers on the scale, this effort is insufficient. 
