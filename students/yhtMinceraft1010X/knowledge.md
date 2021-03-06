### Git Cloning/Windows Credential Manager

These are the pointers that I learned while testing out git cloning for private repositories:
* When cloning private repositories for the first time, access is determined by GitHub account credentials.
* These credentials are stored on Windows Credential Manager, which allows cloning private repositories again without having to log in.
* The stored credentials can be deleted. In that case, for the next private repo cloning attempt, the credentials have to be keyed in again.

### Java Date/Time APIs

#### Below is a table comparing some Java 8 date/time APIs that I worked with in [PR #1625](https://github.com/reposense/RepoSense/pull/1625):

| Point of Comparison | [`Date`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html)                                                          | [`Calendar`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html)                                            | [`LocalDateTime`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)                                                    | [`ZonedDateTime`](https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html)                                                                            |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Packaged In         | `java.util`                                                                                                                      | `java.util`                                                                                                                | `java.time`                                                                                                                                  | `java.time`                                                                                                                                                          |
| Formatter           | [`SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)                                  | [`SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)                            | [`DateTimeFormatter`](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)                                     | [`DateTimeFormatter`](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)                                                             |      
| Time-Zone           | Dependent on time zone. Intended to use UTC, but may depend on host environment.                                                 | Dependent on time zone.                                                                                                    | Itself not dependent on time zone, but can be combined with a time zone to give `ZonedDateTime`.                                             | Dependent on time zone.                                                                                                                                              |
| Month Indexing      | Zero-based indexing (E.g. January is represented by the int 0)                                                                   | Zero-based indexing                                                                                                        | One-based indexing (E.g. January is represented by 1)                                                                                        | One-based indexing                                                                                                                                                   |
| Thread-Safety       | No                                                                                                                               | No                                                                                                                         | Yes                                                                                                                                          | Yes                                                                                                                                                                  |
| Usage in RepoSense  | **No longer in use.** <br/> _Before [PR #1625](https://github.com/reposense/RepoSense/pull/1625):_ <br/> Store commit timestamp. | **No longer in use.** _Before [PR #1625](https://github.com/reposense/RepoSense/pull/1625):_ <br/> Add/subtract date-time. | Store commit timestamp. Time-zone is stored separately. | Format dates for git commands and convert commit timestamps to user's time-zone. |

#### What happened when `SimpleDateFormat` was used in RepoSense:
* When formatting or parsing dates, the internal state of the `SimpleDateFormat` is mutated.
* This causes race conditions, as described in [this post](https://github.com/reposense/RepoSense/issues/1601#issuecomment-1016130764).
* There are two possible error scenarios:
  * A `NumberFormatException` is thrown, just like in [issue #1601](https://github.com/reposense/RepoSense/issues/1601).
  * A date string is parsed with no exceptions, but the date turns out to be wrong/weird. This can be detected by running tests or system tests involving date parsing/formatting and checking the test output against the expected output.

#### Additional Resources:
* https://www.baeldung.com/java-8-date-time-intro
* https://www.baeldung.com/migrating-to-java-8-date-time-api
* https://www.callicoder.com/java-simpledateformat-thread-safety-issues/

### Toolstack upgrades over many versions

Sometimes, the toolstack used in a project may be very out-of-date, requiring a jump over many versions to reach the latest version. Here are some tips for upgrading the toolstack:
* Start a separate branch for your upgrades.
* Check the current versions of the tools that your project is currently using.
* Also take note of any requirements from your current toolstack/project that need to be preserved.
* Check through the release notes for all versions from the current version to the target version that you intend to upgrade to.
  * Although it is ideal to upgrade to the latest version, it may be unrealistic to jump straight there, given that major compatibility-breaking changes can accumulate.
  * Instead, aim for an intermediate version to gradually resolve issues with backward compatibility.
  * While searching through the release notes, take note of any deprecations and compatibility-breaking changes.
  * Take note of any third-party dependencies. Some of them may not have been upgraded alongside the main tools. 
* After upgrading the relevant tools, check that the toolstack/project requirements are preserved.

### Gradle Task Configuration Avoidance
Gradle has [3 build phases](https://docs.gradle.org/current/userguide/build_lifecycle.html#sec:configuration_and_execution_of_a_single_project_build):
* Initialization - determine which projects are part of the build
* Configuration - evaluate build script file and the task properties and dependencies
* Execution - run the relevant tasks

While trying to upgrade Gradle for RepoSense, I came across the [task configuration avoidance](https://docs.gradle.org/current/userguide/task_configuration_avoidance.html) feature, which allows skipping the configuration of unwanted tasks:
```
tasks.register("taskA") {
    // Configure the task here
}
```
What `tasks.register` does is to indicate that such a task exists. However, the task is only created and configured proper if something else in the build needs it.

Using the below syntax eagerly configures the task, regardless of whether the task is ultimately needed or not:
```
task taskA {
    // Configure the task here
}
```
By avoiding task configuration for unwanted tasks, build time for the necessary tasks can be reduced.

### Java 8 vs 11: Why Java 8 is still popular
While exploring the possibility of [migrating RepoSense to Java 11](https://github.com/reposense/RepoSense/issues/1304), one consideration was whether potential RepoSense users are also migrating beyond Java 8.
If most users stay with Java 8, this would decrease the size of our user base.

A few surveys attest to the popularity of Java 8 vs other versions (including Java 11). These surveys can be found below:
* https://www.infoworld.com/article/3652408/java-8-still-dominates-but-java-17-wave-is-coming-survey.html
* https://snyk.io/blog/developers-dont-want-to-leave-java-8-as-64-hold-firm-on-their-preferred-release/

While long-term support (LTS) was cited as a key factor in staying with Java 8, this can only explain why Java 8 is more popular than non-LTS versions. Java 11 and 17 are also LTS versions after all.

Another possible factor is the [change in Java licensing for Java 11 and beyond](https://blogs.oracle.com/java/post/oracle-jdk-releases-for-java-11-and-later). As explained in [this article](https://www.frameworktraining.co.uk/blog/why-is-java-8-more-popular-than-java-14/), organisations using the Oracle JDK version of Java 11+ for commercial purposes need to pay for it.
There is also OpenJDK, but in the same article, it was mentioned that some organisations are reluctant to use OpenJDK.

On an additional note, [Java 8's Extended Support](https://www.oracle.com/java/technologies/java-se-support-roadmap.html) is expected to last longer compared to other LTS versions at this point in time. 
For example, while Java 8 has Extended Support until December 2030, Java 11's Extended Support will end in September 2026. However, support for Java 11 may be extended in the future.
