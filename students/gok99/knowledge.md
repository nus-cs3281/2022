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
 
GitHub actions allows users to run CI on a variety of operating systems, such as Ubuntu XX.04, macOS and Windows Server (which is virtually the same as Windows 10/11 but with better hardware support and more stringent security features). 

GitHub also provides a variety of API to interact with these objects. One quirk I came across with the API was that posting single comments on pull requests need to go through the issues endpoint instead of the pulls endpoint (the endpoint for pulls requires code to be referenced). This doesn't cause problems since issues and pulls will never have identical IDs.

GitHub deployment APIs also returns deployment information in pages, which is a sensible thing to do but can cause slight inconvenience when long running PRs have more deployments than can fit in a page. 
 
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

For example, Bitbucket uses the term 'annotate' instead of 'blame' because the word 'blame' is insufficiently positive.

### Triangular Git workflows

In investigating the output of `git remote -v`, I noticed there were 2 remotes (fetch and push) for each remote name, which I was confused by. The utility of the separation of fetch and push remotes is for triangular workflows. 

We are probably familiar with the common workflow for updating a branch on a forked repo which involves first pulling updates from upstream master, then making changes and pushing to our own fork. This requires remembering to fetch and push to separate repos. With triangular workflows, you can have fetch and push apply to separate repos but with the same remote name, which makes this process much more convenient.

### Cypress Tests

Cypress is a frontend testing tool for testing applications that run in the browser, with tests that are easy to read and write. It uses browser automation (similar to Selenium) and comes with a browser and relevant dependencies out of the box, so it's very easy to set-up. Cypress also provides a dashboard for convenient monitoring of test runs. 

https://docs.cypress.io/guides/overview/why-cypress#In-a-nutshell

### Bash Scripting
 
Bash scripts can be run in a github action workflow, which greatly expands the scope of things you can do with actions. Bash is quite expressive (I hadn't realised just how much it could do). some cool things I learned you could do:
 
* {$VAR,,} to lowercase string in $VAR.
* `$*` gives parameter values separated by the value in `IFS` (Internal File Separator).
* Pipe output into `python3` with a `-c` flag and perform more complex processing with a single line python program.
* Standard output and error can be redirected separately (e.g. `ls 1> out 2> err`)

### Vue

Being relatively new to frontend tools, I found Vue.js to be quite interesting. Vue allows code reusability and abstractions through components. While I didn’t work extensively on the frontend, what I learned from the bits that I did work on was quite cool:

**Vue state**: I found it interesting that you could have computed properties that are accessed the same way as properties, but can depend on other properties and can dynamically update when these properties change. This is often more elegant than using a Vue watcher to update a field. You can even have computed setters that update dependent properties when set. A watcher, however, can be more appropriate when responses to changing data are expensive or need to be done asynchronously.

**Vue custom directives**: Directives are ways to reuse lower level DOM logic. Directives can define vue life-cycle hooks and later be bound to components (can actually take in any JavaScript object literal). For implementing lazy loads, I needed to use the vue-observe-visibility (external library) directive with slight modification to the hooks to be compatible with Vue3.

References:
* https://v2.vuejs.org/v2/guide/computed.html
* https://vuejs.org/guide/reusability/custom-directives.html

### Pug

Pug is a templating language that compiles to HTML. It is less verbose and much more maintainable than HTML, and also allows basic presentation logic with conditionals, loops and case statements.

### JavaScript Quirks
 
There are a lot of these, and most just remain quirks but some result in actual unintended bugs in production (often in edge cases). It was interesting to see this in our contribution bar logic. A technique sometimes used to extract the integer part of a number is to use `parseInt` (it's even suggested in a Stack Overflow [answer]( https://stackoverflow.com/a/48262505)). It turns out we were using this for calculating the number of contribution bars to display for a user. This works for most values, but breaks when numbers become very large or small (less than 10^-7). In this unlikely situation, we'd display anywhere from 1 to 9 extra bars (moral: use `Math.floor` instead!).
 
### Browser Engines

An investigation into string representations in browsers led me down a rabbit hole of JavaScript runtimes and engines, and ultimately improved my understanding of JavaScript in general. Different browsers have different JS engines - Chrome uses V8, Firefox uses SpiderMonkey (the original JS engine written by Brendan Eich), Edge used to use Chakra but now also uses V8, Safari uses WebKit, etc. Engines often differ significantly in terms of the pipeline for code execution, garbage collection, and more.

The V8 engine as an example, first parses JavaScript into an Abstract Syntax Tree (AST) which is then compiled into bytecode. This bytecode is interpreted by the Ignition interpreter (Ignition also handles compilation of the AST into bytecode). Code that is revisited often during interpretation is marked "hot" and compiled further into highly efficient machine code. This technique of optimising compilation based on run-time profiling (Just-In-Time (JIT) compilation) is also used in other browser engines like SpiderMonkey and the JVM.

The engine is used for running things that are on the browser stack. JS is run in a single thread, and asynchronous tasks are done through callbacks in a task queue. The main script is first run, with things like promises and timeouts inserting tasks into a task queue. Tasks (and microtasks which are more urgent, lower overhead tasks that can execute when the call stack is empty even while the main script is running) in a task queue wait for the stack to be empty before being executed. Page re-renders are also blocked by running code on the stack (long delays between re-renders are undesirable). Using callbacks and hence not blocking the task queue, allows re-rendering to be done more regularly, improving responsiveness. The precise behaviour of task de-queueing (and lower overhead microtasks) can actually differ between browsers, which causes considerable headache.

References:
* https://cabulous.medium.com/how-v8-javascript-engine-works-5393832d80a7
* https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/
* https://www.youtube.com/watch?v=8aGhZQkoFbQ


### General Software Engineering/Design Considerations

Discussions over PRs, issues and generally attempting to solve issues, were a great way to explore design considerations. Here is a non-exhaustive list of interesting points that came up this semester:

**In-house vs External Library**

In implementing new functionality or extending existing functionality (Git interface for example), there is usually a question of whether it would be easier to maintain features in-house, or use external libraries. It might be a good idea to maintain core functionality in-house since we'd want more fine-grained control over these features and new features can be added/fixed quickly as needed. At the same time, external libraries save time and cost of learning about and solving possibly complex problems.

External libraries can however introduce vulnerabilities (several incidences of dependency sabotage with npm packages like color.js and node-ipc hit fairly close to home over the course of the semester). Hence, selection of libraries should be a well-deliberated process and should include considerations like active-ness of the project and diversity of maintainers.

**Recency vs Ubiquity**

In maintaining versions of dependencies, it is often important to weigh upgrading to a new version to get the newest features against possibly alienating users who don't already have that version. Neither is necessarily better than the other and will likely depend on the nature of the product. A new product for developers would probably have users who want new versions with the bleeding edge of features. On the other hand products that already have a large user base and aimed at less technical users might want to favour ubiquitous versions. Since RepoSense is aimed at users of all skill levels, including novice developers, we often default to the later approach. 

In a similar vein, it might be important to make sure that new features don't break backward compatibility so that the end-user won't face significant hindrances with making upgrades. At the same time, the need to be backwards compatible can be a root of evil, introducing all manners of hacks and fixes. This highlights the importance of foresight in the early stages of development. Also, deciding when to stop backwards compatibility with a significant version bump can be a non-trivial decision. Doing so should come with thorough migration documentation (sparse documentation for Vue2 -> Vue3 migration caused a lot of developer grievances).

**Isolated Testing**

While it's fairly obvious that modularity with tests is important and that components should be tested in isolation with unchanging inputs, it is easy to let slip lapses in the form of hidden dependencies that prevent components from being isolated, or having inputs that are actually non-static. Some of these issues came up over the course of the semester but it struck me just how easy it was for them to slip by unnoticed. There aren't necessarily language-level features that enforce coupling rules for the most part since many of these dependencies can be quite implicit.

This had me thinking about the importance of being explicit in crucial sections of code, as described below.

**Being Explicit**

It is important that programmers make the behaviour of certain crucial sections of code as explicit as possible. One way of doing this is through good naming of methods and variables, and grouping statements semantically into methods or classes. Large chunks of code is detrimental and allows implicit slips in behaviour that can go unnoticed. So we might even want to make new special classes that do very specific things to make it clear that it is an important subroutine/behaviour that deserves its own abstraction.

At the same time, high reliance on object orientation can lead to too many classes, each class doing trivial things and with high coupling between the classes leading to spaghetti logic that doesn't do very much to alleviate implicit behaviour. There exists a delicate middle ground characterised by semantically well partitioned code.

**Behavioural Consistency**

The earlier section on Javascript quirks were a result of an overly accommodating feature integration during the early stages of development. It's become a cautionary tale of sorts of the importance of consistency and predictability in behaviour. In adding new features, it was personally very tempting to allow small inconsistencies in behaviour in favour of simplicity of implementation. While simplicity is a desirable outcome, I'd argue that consistency is more important (small inconsistencies can runaway into larger un-fixable differences).

Consistency can be with respect to various things. For example, we might want that identical inputs behave the same under similar conditions (differing in non-semantic respects) or that similar inputs (differing in non-semantic respects) behave the same under the identical conditions, etc.

### Miscellaneous helpful tools
 
* The command line tool [GitHub cli](https://github.com/cli/cli) provides a very handy way to access GitHub API, and has been useful for checking out PRs, interacting with issues, managing workflows, etc right from the command line.
* `git bisect` is a very nice way to find problematic commits. Given a bad commit and a previously good commit, `git bisect` does a binary search (either automatically with a test or with manual intervention) to find the problematic commit where the issue was introduced.
* Search through previously run commands (with the first few characters) with ctrl-r in a bash shell.
* GitHub issues and PRs have advanced search syntax like `involves:USER` for all items that involve a user. This was very useful for updating `progress.md`. More features [here](https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests).
