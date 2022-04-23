## FileSystems, Bash and CMD
### Specifying File Paths in Command Line Arguments 
General:
- File path arguments should nearly always be wrapped in quotation marks to accommodate file paths containing spaces. 

For UNIX file systems:
- `/` is the separator used by UNIX systems
- UNIX file systems allows for virtually any character to be used in the filename except `/` and `null` (and some specific restricted names). As such, filenames and paths containing non-standard characters
can lead to unexpected errors in program execution. It is important to be aware of such a possibility. 
  - For example, in RepoSense, a local repository's path is first read as input and then used as a String in a CLI argument. A valid \ character in a filename 
  will end up behaving as an escape character in the new CLI command. 

For Windows file systems:
- \ is the separator for Windows file systems. However, it is also compatible with `/`. All `/` read in a file path are 
replaced with \ as per Microsoft's [documentation](https://docs.microsoft.com/en-us/dotnet/standard/io/file-path-formats#canonicalize-separators).
- Java's implementation of the `Paths::get` method performs Windows file path validation if run on a Windows system. However, if used in a test method run from UNIX, the behaviour will differ.

### Relative and Absolute File Pathing Specifics
- Both `./` and `../` function similarly in both UNIX and Windows (replacing `/` with \\).
- `~` is an absolute pathing feature used in [Bash](https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html#Tilde-Expansion) which expands to the
user's home directory.
  - If tilde expansion is used as follows: `~chan-jd/"some file path"/` then `~chan-jd` is expanded to the `$HOME` directory of the user `chan-jd`.
  - If wrapped within quotation marks, it becomes a literal `~` char in the file path. Thus, to use both together, 
  the tilde has to be left out of the quotation marks: `~/"some test file"`.
  - `~` does not work in Windows command prompt but does work in Windows PowerShell
- Windows has various methods to apply the current directory to a path
  - If a file path starts with a single component separator like `\utilities` and the current directory is `C:\temp` then normalization produces `C:\utilities`
  - If it starts with a drive without component separator like `D:sources`, it looks for the most recent directory accessed on `D:\`. If it is `D:\temp` then the normalization produces `D:\temp\sources`.
  - Otherwise, a path like `example\path` will be prepended with the current directory on normalization.

### Special Characters in Bash String Arguments
- Double quoting an argument will still allow for special characters like `$` (variable substitution) to work.
  - [Reference manual link](https://www.gnu.org/software/bash/manual/html_node/Double-Quotes.html#Double-Quotes)
- Single quoting preserves the literal value of every single character within the quotes. Another single quote will end the quotation and there is no way to escape it.
  - [Reference manual link](https://www.gnu.org/software/bash/manual/html_node/Single-Quotes.html#Single-Quotes)
  - This [stackoverflow post](https://stackoverflow.com/questions/1250079/how-to-escape-single-quotes-within-single-quoted-strings) suggests a good way to handle quoting arguments containing both single quotes and special characters.
    - Wrap all characters besides single quotes in `'`.
    - For `'`, replace it with `'"'"'`. Consecutive quoted strings not separated by spaces are treated as a single argument in Bash. This quotes the single quote in double quotes where it does not have special meaning.
- A (possibly incomplete) list of special characters in Bash that need to be escaped can be found in this [stackexchange post](https://unix.stackexchange.com/questions/347332/what-characters-need-to-be-escaped-in-files-without-quotes).
- _Relevant note_: On Windows CMD, the `'` single quote has no special meaning and cannot be used to quote arguments in CMD. Only double quotes works for arguments containing spaces to be treated as a single argument.

### Variable and tilde expansion in CMD and Bash
When we run something like `java -jar RepoSense.jar some cli args`, variable expansion and tilde expansion (for Bash) is performed for us before the Java program receives it.
- E.g. If I specify my repository as `~/reposense/Reposense`, the `main` method in the Java program will receive the String `/chan-jd/home/reposense/Reposense` in its `args` array.
- This behaviour is mostly beneficial for us but can cause some non-uniform program behaviour when user has more than one way to specify their arguments. An example relevant to RepoSense is that users can specify their local repository file paths in both the command line or the `repo-config.csv` file.
  But when RepoSense reads the data straight from the CSV file, it does not perform the necessary expansions. Using the example above, `RepoSense` will receive the raw version of the String, `~/reposense/Reposense` instead which might cause some issues.
- One possible way to work around this is to `echo` the command in CMD or Bash. The command output will include the substituted expansions.

## GitHub
### Deployments and Environments
- [Environments](https://docs.github.com/en/rest/reference/deployments#environments) can be viewed as the platform for which deployments are staged. There are generally fewer of them. For example in RepoSense, there is roughly two environments per active pull request for deployments.
  - Environments can be viewed on the main page of a repository. 
  - They will linger so long as the deployment on the environment continues to exist and would normally require manual deletion.
- [Deployments](https://docs.github.com/en/rest/reference/deployments) are requests to deploy a specific version of the repo such as a pending pull request. In the context of RepoSense, a single PR can have several tens of deployments if it is consistently updated.
  - It is generally difficult to track and control deployments on the GitHub page itself.
  - However, through the GitHub API, we can query all deployments relating to an environment and delete them. This will automatically remove the environment from the listing as well. This solution was taken from this [stackoverflow post](https://stackoverflow.com/a/61272173).

### GitHub API
The GitHub API provides us with a way to interact with repositories via a RESTful API. Using `curl` commands:
- We are able to query (via `GET`) for information such as branches, deployments and environments
- We are also able to `POST` commands to a repository to perform various actions such as deleting deployments. These generally require a security token which might not be available from a personal computer's CLI. When running GitHub Actions, it is possible to acquire one for the repository to perform actions such as closing deployments.

### GitHub Actions
We must add GitHub action workflow files to the directory `.github/workflows` in order to automatically perform certain scripts on certain GitHub actions.
- The general workflow of a `.yml` workflow file contains a declaration `on` which states under what scenarios will these actions be triggered
  - It is followed by a list of jobs. For each job, we can declare a name, platform to run on, environment variables, followed by sequential steps to perform.
  - Under steps, we can use `run` to run shell scripts similar to running the same command from a BASH terminal 
- When setting up an environment to perform a specific workflow, we can generally choose exactly what OS to run, what versions of Java or other languages/packages to use. 
  - It is additionally helpful to be aware of what are the versions used by default for the various OS-es. 
    - [Ubuntu 20.04](https://github.com/actions/virtual-environments/blob/c253e7f4d192e492988596167a1d461e99698c13/images/linux/Ubuntu2004-README.md) - here we can see the default Git used is 2.30.2. It might be helpful to be aware of such specifications as we might experience differing behavior in the future due to version differences.
  - Related to RepoSense, and caused me quite a bit of trouble is that GitHub Actions uses the default timezone of UTC+00.00 which leads to some commits being assigned to the previous day as compared to if it were run locally on my own machine which is at UTC+8. 

## Git Specifications:
### Git Diff Output
The `git diff` command is used heavily by RepoSense for analyzing. The following are behaviour in the output that I discovered from self-testing as documentation about the behaviour was difficult to find:
- The filename `/dev/null` represents a non-existent file. A mapping from `/dev/null` to a file indicates a new file.
  Similarly, a mapping from a file to `/dev/null` indicates a deleted file.
- For filenames containing certain special characters (i.e. the lines `--- a/some/file/name` or `+++ b/some/file/name`), the listing of filenames in the before-after comparison is adjusted slightly.
  
  - For files containing spaces, the filename will have a tab character at the end. E.g. `+++ b/space test\t`.
  - For files containing special characters (not including space) such as \\, `"`, `\t`, the filename will be placed in quotation marks. E.g. `+++ "b/tab\\ttest/"`
  - For files containing both of the above cases, the filename is first wrapped in double quotation marks followed by a tab character.
- These nuances in `git diff` filename output may be important for filename designation as is done in RepoSense.

### Git Log Output
Every commit in the log output displays the hash, author with email, date and message.
- If a user has set an email in their `git config` but set their Github 'keep my email address private' setting to true, web-based Git operations will list their email as `username@users.noreply.github.com`.
- It is possible to explicitly set the email to be empty through `git config --global user.email \<\>` which will set it to `<>`. No email will show up in a commit done by such a user config. 

### Git Commit & Config Interaction
`git commit` commit information details can be found [here](https://git-scm.com/docs/git-commit#_commit_information).
- When committing with a `user.name` and `user.email` that matches a GitHub account, commits on GitHub will be able to link the GitHub account to the commit.
  - However, committing with an empty email `<>` will cause the commit to look like [this on GitHub](https://github.com/reposense/testrepo-Alpha/commit/2536b8e0de3366989f4b124b9a5a613db010379e) where there is no account linked to it.
- It is possible temporarily set `user.name` as `<>`. However, it will not allow the user to commit, citing an invalid username.
  - It is also possible to set `user.name` as an empty string which is equivalent to resetting it. Git will not allow commits until a `user.name` has been set.

### Git Changelogs
This section refers specifically to the changes to the `Git` tool itself. I have found out from my own experience that finding information relating to `Git` versions can be difficult as most search results relate to how `Git` can be used to manage those versions.
- The release notes can be found at https://github.com/git/git/tree/master/Documentation/RelNotes
- One (rather inefficient) way I have found to attempt to search for relevant information regarding when a specific change was made was to do the following:
  1. Clone the `git` repository locally (Note the repository is quite large)
  2. Create a bash script that takes in a string from the command line and `grep`-es it against all the text files in the folder `Documentation/RelNotes`
  - Queries are generally quite fast. For example, if I wanted to find out when the `git blame --ignore-revs-file` flag was added, I could search for `git blame` and see all relevant release notes and look them up manually. 
  - `grep` can be set to quiet mode if I'm just looking for the file containing the reference. Otherwise, in non-quiet mode, the line in which the string match is found is printed. I can read the line and see if it is directly related to what I am looking for without having to look-up the file myself.
- Descriptions of the changes can be somewhat vague. It is usually easier to look for the specific command and see if it showed up in the specific release notes rather than trying to find keywords relating to the change in mind.
  - For example the `ignore-revs-list` flag addition was done in 2.23.0. The release notes reads `"git blame" learned to "ignore" commits in the history, ...`.
  - For full details on the change, we need to go to the commit message itself. The commit messages are extremely detailed, e.g. `git blame --ignore-revs-file` commit can be found [here](https://github.com/git/git/commit/ae3f36dea16e51041c56ba9ed6b38380c8421816).

## Javascript/Vue.js
New language for me, no prior experience. Learnt how to read and interpret most of the lines of Javascript in RepoSense.
- Javascript has `Object`s which are a container of key-value pairs listing its properties, and some pre-defined methods. 
  - Can perform operations on `Object`s similar to what can be done to a map. Use `Object.entries(object)` to enumerate the key-value pairs.
  - The properties of an object can be created, edited and accessed from anywhere more similar to languages like Python's OOP
- Javascript uses `(args...) => output` to write lambda functions as opposed to `(args...) -> output` for Java.
- Javascript has something known as object destructuring where we can extract the properties of an object.
  - Given something like `author = {name: JD, repo: RepoSense}`, then doing `const {name, repo} = author` would allow us to access the `name` and `repo` as local variables.

Interacting with objects on a webpage:
- Each click on an interactive component on the webpage fires off a `clickEvent`. 
  - In Vue, we can tag listener methods to a specific event a component might encounter. 
  - The event itself also contains details about the circumstances in which it was triggered such as other buttons that were pressed while the `clickEvent` was created.
  - The default behaviour for clicking on an anchor link object is to open the hyperlink reference. This can be prevented with a listener that does `event.preventDefault()`.

## HTML
- `href` attribute of an anchor object provides the hyperlink reference to whichever URL page the anchor object is supposed to open.
  - Notably, if `href` is not set and left as `undefined`, then a link will not be opened even if `target` is set.
- In conjunction with the `href` property, there is the `target` property which designates where the hyperlink will be opened.
  - Most commonly used in RepoSense is `target="_blank"` which means to open a new window or tab.
  - There are other alternatives such as `_self` for opening in the same frame, `_parent` and `_top`.

## Regular Expressions
Java provides extensive support for Regex matching via the `Pattern` and `Matcher` classes which facilitate string parsing. 
- [`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) is Java's class for handling various regex pattern.
  - The API provides a list of all the recognised syntax. In particular, I would like to go over the different quantifier types as I believe they are quite important for anyone who wants to build complex regex patterns. 
  - `Greedy quantifiers` - what we see the most often: `x?`, `x*`, `x+`.
    - These will always try to be met if possible. They take precedence over `Reluctant quantifiers`.
  - `Reluctant quantifiers` - e.g. `x??`, `x*?`, `x+?`.
    - These matched only if it is required for the regex to fully match. 
    - For example, matching `(?<greedy>.*)(?<reluctant>.*)` on the string `Example word` will result in `greedy = Example word` and `reluctant = ""`;
  - `Possessive quantifiers` - e.g. `x?+`, `x*+`, `x++`.
    - When Java does the matching from left to right, these quantifiers will be matched first. Once Java reaches the end of the line, it actually does backtracking to see if some greedy/reluctant quantifiers can give up some characters for the regex to match.
    - However, possessive quantifiers will never give up their characters on backtrack even if it means that the matcher would otherwise have matched. 
    - Possessive quantifiers should be used carefully. However, their behaviour is more straightforward and easy to understand.
- [`Matcher`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html)
  - We can perform `Pattern p = ...; p.matcher("some String")` to obtain a `Matcher` object. Most of the information that we want can be obtained from this object.
  - However, there are some points to be noted about the implementation of the `Matcher` object in Java.
    - `Matcher` objects (for Java 8 and 11) are initially just a mutable container containing the regex and the String. Matching logic has not yet been performed.
    - Suppose we have named groups within our regex pattern. We have to run a `matches` or `find` method at least once for the object to mutate and support group queries. 
      - Calling a `Matcher::group` directly without first calling `matches` or `find` leads to an `IllegalStateException` as the object never attempted to match anything yet. 

Regex testing can be particularly cumbersome, slow and difficult to grasp why the regex is behaving the way it is. I personally found this website [regex101](https://regex101.com/) which allows convenient testing of various regex patterns on different inputs. It also supports regex testing for different languages.

## GSON package
Gson (also known as Google Gson) is an open-source Java library to serialize and deserialize Java objects to (and from) JSON. (Taken from Wikipedia's description)
- I used this [baeldung.com guide](https://www.baeldung.com/gson-json-to-map) to learn about converting JSON objects into HashMaps. They also have various other articles describing other features like deserialization to greater depth. Their API can also be easily found online. This is their [2.9.0 API](https://javadoc.io/doc/com.google.code.gson/gson/latest/index.html).
- From my experience, the general way in which GSON reads a JSON file is to read it into a `JsonElement` abstract class with four different types, a `JsonArray`, `JsonNull`, `JsonObject` and `JsonPrimitive`. 
  - Most of the types listed above are very similar to their Java counterparts. The one that stands out is the `JsonObject` which is more similar to the Javascript object as it is a collection of name-value pairs where each name is a string and each value is another `JsonElement`.
- In order to read JSON files into more exotic objects that we might have previously defined, we can either use a `TypeToken` for the GSON `JsonParser` or implement our own `JsonDeserializer` for each class we want to be read from a JSON file.
  - One naive way of parsing a JSON file into a Map is to do `Map map = new Gson().fromJson(JSON_FILE, Map.class);`. However, this immediately runs into the problem where the map class itself is using raw types.
    - To avoid this, we can use a `TypeToken`. By creating a new type as such
    
          Type mapType = new TypeToken<Map<String, Commit>>() {}.getType();
          Map<String, Commit> map = new Gson().fromJson(JSON_FILE, mapType);
      We are able to preserve the generics class types used.

## Others
### "file" URI Scheme
- Specification can be found here [RFC8089](https://datatracker.ietf.org/doc/html/rfc8089).
  - A file URL looks like `file://host.example.com/path/to/file`.
    - Double slashes following the colon `://` indicates that the file is not local and `host.example.com` is the 'authority' which hosts the file
    - Single slash or triple slashes after the colon `:/` or `:///` will both be treated as a local file. Everything after the last slash forms the path to the file.

### cURL command
cURL is a command-line tool to transfer data to or from a server using support protocols. In particular, GitHub Actions shows many examples of RESTful API calls using the `curl` command.

### RESTful API
- REST stands for _Representational state transfer_. It is an architectural style for an API that uses HTTP requests to access and use data. (description taken from [here](https://www.techtarget.com/searchapparchitecture/definition/RESTful-API)). 
- GitHub has its own RESTful API where we can make queries to repositories and possibly post commands to a repo provided we have proper access rights. The return type for queries is normally in the JSON format.

### Bash Shell Scripting
- Can use `.sh` files to write shell scripts that can be run in Linux environments
- Can specify functions as `function_name() { ... }`
  - Functional arguments can be referenced from within the function declaration block with `${1}` referring to the first argument, `${2}` referring to the second argument, etc.
  - These functional arguments can be specified by a user via `function_name "$argument1" "$argument2"`
  - `$@` can also be used to refer to all of the shell script's CLI arguments. It can be iterated through like an array.

### Other popular remote repository hosts
- Any remote site that allows access to a `.git` directory is capable of hosting a remote repository
- Popular remote repository domains besides GitHub include sites like GitLab and BitBucket
  - Interacting with these are nearly identical to interacting with GitHub, with `https` and `ssh` options to clone a repository.
  - Relevant to RepoSense: the paths to commits and other features are usually different between the sites. For RepoSense to support all these websites, it'll have to take into account the differences in the path to these resources on the website.
  
### IntelliJ Inspect Code
The Inspect Code tool allows IntelliJ to look for common code quality issues such as
- Declaration redundancy
- Probable bugs
- Proofreading

Limitations:
- A full run of the code cleanup takes a long time to complete. A run on RepoSense itself takes ~10 minutes.
  This can be cut significantly shorter if we ignore the proofreading checks which cuts it down to 30 seconds.
- 'Unused' (as declared by IntelliJ) fields might not be redundant such as in [`SummaryJson.java`](https://github.com/reposense/RepoSense/blob/master/src/main/java/reposense/report/SummaryJson.java) where the fields are necessary for conversion to a JSON file.
- 'Unused' methods are sometimes necessary for other tools such as JavaBeans (requires setters and getters) and JavaFX (@FXML methods are not detected as 'used')

Thus, we should still exercise discretion in using this tool even if it is something as simple as removing unused variables or methods.

### File locks
When Java opens a file for writing, it obtains a file lock in the form of `filename.extension.lck` with `lck` standing for lock. 
This serves to support mutual exclusion for access to writing to the file. This appears in RepoSense when loggers attempt to write to the log file in which case, some kind of mutual exclusion gurantee is required.
- Notably, file locks (and other process resources) are released automatically when the main Java process `exits()`. 
  - However, in some scenarios, releasing the lock only at the end of the entire process might be too late. One example I encountered is that when running `gradlew` system tests, the `log` file lock is held on to for the entire duration of the run over multiple system tests. This causes issues with running consecutive system tests as I am unable to delete the previous system test's report due to the lingering file lock.
- In some scenarios when the program does not exit properly, the `.lck` file might be left behind. 
  - An example execution that results in this is running RepoSense via `gradlew` command line with the `-v` command. After `ctrl+C` to close the server, the `.lck` file persists even though it should have been cleaned up. For some reason, running `gradlew` on Ubuntu 18.04 does not have the same issue.
    - However, this seems to be mostly harmless as it does not affect anything else.
- Suggested by this [stackoverflow post](https://stackoverflow.com/questions/12849138/close-log-files/22957009#22957009), one way to easily release all log resources at the end of Java execution is to use `LogManager.getLogManager().reset()` which immediately releases all resources.

### Checkstyle
Their GitHub repository can be found [here](https://github.com/checkstyle/checkstyle) where we can view the features they are working on and bugs that other people are experiencing. 
- In particular, there is a rather strange bug relating to `forceStrictCondition` which is not able to properly detect parent lines of nested line wrapppings.
  - The relevant issues can be found in [Issue #6024](https://github.com/checkstyle/checkstyle/issues/6024) and [Issue #6020](https://github.com/checkstyle/checkstyle/issues/6020)
- The above issues result in some somewhat strange enforcements, for example (taken from #6024), the code below has violations though it is what we expect the indentations to be

```
Arrays.asList(true,
        Arrays.asList(true,
                true, //violation
                true));  //violation
```
- While the same line of code below is what passes the checkstyle but has unusual indentation.
```
Arrays.asList(true,
        Arrays.asList(true,
        true, // no violation, but should be
        true));  // no violation, but should be
```
- There appears to be quite a distinct tradeoff here as without `forceStrictCondition`, checkstyle only enforces the minimum required indentation level. A user's indentation can be as large as they desire so long as it does not exceed the line character limit.
  - However, if we were to `forceStrictCondition`, then for nested line wrappings, the indentation being enforced can be somewhat strange.

