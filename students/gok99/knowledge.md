### Gradle
 
Gradle is a very flexible build automation tool used for everything from testing and formatting, to builds and deployments. Unlike with other build automation tools like Maven where build scripts written in XML (a widely hated feature of the tool), Gradle build scripts are written in a domain specific language based on Groovy or Kotlin, which are both JVM based languages. This means that it can interact seamlessly with Java libraries.
 
Gradle is also much more performant than alternatives like Maven because of its:
- Build caching: Only reruns tasks whose inputs have been changed.
- Gradle daemon: A background process that stores information about the project in memory so that startup time can be cut down during builds.
 
RepoSense recently added functionality for hot reload on frontend as a Gradle task, which made frontend development a lot more productive. Unfortunately, the feature isn't available on Linux because the package we were using (Apache Ant's [condition package](https://ant.apache.org/manual/api/org/apache/tools/ant/taskdefs/condition/package-summary.html)) could not specifically check for it. Migrating to Gradle's own [platform package](https://docs.gradle.org/current/javadoc/org/gradle/nativeplatform/platform/package-summary.html) recently taken out of incubation, allowed us to support all 3 prominent operating systems.
 
References:
* https://docs.gradle.org/current/userguide/userguide.html
* https://docs.gradle.org/current/userguide/gradle_daemon.html#sec:why_the_daemon
* https://docs.gradle.org/current/javadoc/index.html?overview-summary.html
* https://docs.gradle.org/current/javadoc/org/gradle/nativeplatform/platform/package-summary.html
* https://ant.apache.org/manual/api/org/apache/tools/ant/taskdefs/condition/package-summary.html
* https://stackoverflow.com/a/31443955
 
### GitHub Actions and API
 
Like Gradle, Github Actions help with automation of workflows like CI/CD and project management, and can be triggered by a variety of events (pull requests, issues, releases, forks, etc). It also has a growing library of plugins that make workflows a lot easier to set-up. I was surprised that there is some nice tooling support for GitHub actions on IntelliJ.
 
GitHub also provides a variety of API to interact with these objects. One quirk I came across with the API was that posting single comments on pull requests need to go through the issues endpoint instead of the pulls endpoint (the endpoint for pulls requires code to be referenced). This doesn't cause problems since issues and pulls will never have identical IDs.
 
Actions and APIs also have some great documentation:
* https://docs.github.com/en/rest/guides/getting-started-with-the-rest-api
* https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions
* https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows

### Git Remotes

Git exploded in popularity in large part due to Git hosting providers like GitHub. GitLab and Bitbucket are also commonly used Git hosts. RepoSense has thus far only largely supported GitHub, but there is a clear incentive to support other commonly used remotes. This is made a little challenging due to differences in conventions between the sites:

| |`base_url`|Commit View|Blame View|
|:-:|:-:|:-:|:-:|
|GitHub|github.com|`{base_url}/{org}/{repo_name}/commit/{commit_hash}`|`{base_url}/{org}/{repo_name}/blame/{branch}/{file_path}`|
|GitLab|gitlab.com|`{base_url}/{org}/{repo_name}/-/commit/{commit_hash}`|`{base_url}/{org}/{repo_name}/-/blame/{branch}/{file_path}`|
|Bitbucket|bitbucket.org|`{base_url}/{org}/{repo_name}/commits/{commit_hash}`|`{base_url}/{org}/{repo_name}/annotate/{branch}/{file_path}`|

For example, Bitbucket uses the term 'annotate' instead of 'blame' because the word 'blame' is insufficiently postive.

### Cypress Tests

Cypress is a frontend testing tool for testing applications that run in the browser, with tests that are easy to read and write. It uses browser automation (similar to Selenium) and comes with a browser and relevant dependencies out of the box, so it's very easy to set-up. Cypress also provides a dashboard for convenient monitoring of test runs. 

https://docs.cypress.io/guides/overview/why-cypress#In-a-nutshell

### Bash Scripting
 
Bash scripts can be run in a github action workflow, which greatly expands the scope of things you can do with actions. Bash is quite expressive (I hadn't realized just how much it could do). some cool things I learned you could do:
 
* {$VAR,,} to lowercase string in $VAR.
* `$*` gives parameter values separated by the value in `IFS` (Internal File Separator).
* Pipe output into `python3` with a `-c` flag and perform more complex processing with a single line python program.
* Standard output and error can be redirected separately (e.g. `ls 1> out 2> err`)
 
### JavaScript Quirks
 
There are a lot of these, and most just remain quirks but some result in actual unintended bugs in production (often in edge cases). It was interesting to see this in our contribution bar logic. A technique sometimes used to extract the integer part of a number is to use `parseInt` (it's even suggested in a Stack Overflow [answer]( https://stackoverflow.com/a/48262505)). It turns out we were using this for calculating the number of contribution bars to display for a user. This works for most values, but breaks when numbers become very large or small (less than 10^-7). In this unlikely situation, we'd display anywhere from 1 to 9 extra bars (moral: use `Math.floor` instead!).
 
### Miscellaneous helpful tools
 
* The command line tool [GitHub cli](https://github.com/cli/cli) provides a very handy way to access GitHub API, and has been useful for checking out PRs, interacting with issues, managing workflows, etc right from the command line.
* `git bisect` is a very nice way to find problematic commits. Given a bad commit and a previously good commit, `git bisect` does a binary search (either automatically with a test or with manual intervention) to find the problematic commit where the issue was introduced.
* Search through previously run commands (with the first few characters) with ctrl-r in a bash shell.
