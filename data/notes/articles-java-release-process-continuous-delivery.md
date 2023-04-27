
> This is a re-post of an article that is no longer hosted online. You can see it on the wayback machine at https://web.archive.org/web/20141221084323/http://www.dev9.com/blog/2014/9/java-release-process-with-continuous-delivery

## Intro

One of the most interesting things we deal with is releases. Not a deployment -- which is actually running the new software. A release, in our parlance, is creating a binary artifact at a specific and immutable version. In the Java world, most of us use Maven for releases. More pointedly, we use the `maven-release-plugin`. I am going to show you why you should stop using that plugin.

## Why Change?

This is a question I field a lot. There are several reasons, but the primary one is this: In a continuous delivery world, any commit could theoretically go to production. This means that you should be performing a maven release every time you build the software. So, let's revisit what happens inside your CI server when you use the maven-release-plugin properly:

- CI checks out the latest revision from SCM
- Maven compiles the sources and runs the tests
- Release Plugin transforms the POMs with the new non-SNAPSHOT version number
- Maven compiles the sources and runs the tests
- Release Plugin commits the new POMs into SCM
- Release Plugin tags the new SCM revision with the version number
- Release Plugin transforms the POMs to version n+1 -SNAPSHOT
- Release Plugin commits the new new POMs into SCM
- Release Plugin checks out the new tag from SCM
- Maven compiles the sources and runs the tests
- Maven publishes the binaries into the Artifact Repository

Did you get all of that? It's **3 full checkout/test cycles, 2 POM manipulations, and 3 SCM revisions**. Not to mention, what happens when somebody commits a change to the pom.xml (say, to add a new dependency) in the middle of all this? It's not pretty.

The method we're going to propose has **1 checkout/test cycle, 1 POM manipulation, and 1 SCM interaction**. I don't know about you, but this seems significantly safer.

## Versioning

Before we get into the details, let's talk about versioning. Most organizations follow the versioning convention they see most frequently (often called Semantic Versioning or SEMVER), but don't follow the actual principles. The main idea behind this convention is that you have 3 version numbers in dotted notation `X.Y.Z`, where:

- `X` is the major version. Any changes here are backwards-incompatible.
- `Y` is the minor version. Any changes here are backwards-compatible, but there may be bug fixes or new features.
- `Z` is the incremental version. All changes here are backwards-compatible.

However, most organizations do not use these numbers correctly. How many apps have you seen that sit at 1.0.x despite drastic breaking changes, feature addition/removal, and more? This scheme provides little value, especially when most artifacts are used in-house only. So, what makes a good version number?

- *Natural order*: it should be possible to determine at a glance between two versions which one is newer
- *Build tool support*: Maven should be able to deal with the format of the version number to enforce the natural order
- *Machine incrementable*: so you don't have to specify it explicitly every time

While subversion offers a great candidate (the repository commit number), git does not have the same. However, all build systems, including both Bamboo and Jenkins, expose an environment variable that is the current build number. This is a perfect candidate that satisfies all three criteria, and has the added benefit that any artifact can be tied back to its specific build through convention.

### What about Snapshots?

Snapshots are an anti-pattern in continuous delivery. Snapshots are, by definition, ephemeral. However, we're making one exception, and that's in the POM file itself. The rule we're following is that the pom.xml always has the version `0-SNAPSHOT`. From here on out, no more snapshots!
The New Way

So, we're going to use the build number as the version number, and not have snapshots (except as described above). Our POM file is going to look a little something like this:

```
<project ...>
  ...
  <version>0-SNAPSHOT</version>
</project>
```

This is the only time we will use `-SNAPSHOT` identifiers. Everything else will be explicitly versioned. I am assuming your distributionManagement and scm blocks are filled in correctly. Next, we need to add 2 plugins to our POM file:

```
<build>
    ...
    <plugins>
    ...
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>versions-maven-plugin</artifactId>
            <version>2.1</version>
        </plugin>
        <plugin>
            <artifactId>maven-scm-plugin</artifactId>
            <version>1.8.1</version>
            <configuration>
                <tag>${project.artifactId}-${project.version}</tag>
            </configuration>
        </plugin>
    </plugins>
</build>
```

The devil is in the details, of course, so let's see what should happen now during your release process. Note that I am using Bamboo in this example. You should make sure to modify it for your CI server's variables. The process is:

1. CI checks out the latest revision from SCM
1. CI runs `mvn versions:set -DnewVersion=$ {bamboo.buildNumber}`
1. Maven compiles the sources and runs the tests
1. Maven publishes the binaries into the Artifact Repository
1. Maven tags the version

> Steps 3, 4, and 5 are run with one command: mvn deploy scm:tag.

That's it. We have one specific revision being tagged for a release. Our history is cleaner, we can see exactly which revision/refs were used for a release, and it's immune to pom.xml changes being committed during the process. Much better!

## Gotcha!

Ok, this all works great, unless you have a bad setup. The primary culprit of a bad setup is distinct modules having snapshot dependencies. Remember how I told you snapshots are an anti-pattern? Here's the general rule: if the modules are part of the same build/release lifecycle, they should be put together in one source repository, and should be built/versioned/tagged/released as one unit. If the modules are completely separate, then they should be in a separate source repository, and you should have fixed-version dependencies between them to provide a consistent interface. If you are depending on snapshot versions, you are creating non-repeatable builds, as the time of day you run the build/release will determine which exact dependency you fetch. 