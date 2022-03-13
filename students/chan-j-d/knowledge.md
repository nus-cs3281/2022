## IntelliJ Tools
### Inspect Code
the Inspect Code tool allows IntelliJ to look for common code quality issues such as 
- Declaration redundancy
- Probable bugs
- Proofreading

Limitations:
- A full run of the code cleanup takes a long time to complete. A run on RepoSense itself takes ~10 minutes.
This can be cut significantly shorter if we ignore the proofreading checks which cuts it down to 30 seconds.
- 'Unused' (as declared by IntelliJ) fields might not be redundant such as in [`SummaryJson.java`](https://github.com/reposense/RepoSense/blob/master/src/main/java/reposense/report/SummaryJson.java) where the fields are necessary for conversion to a JSON file.
- 'Unused' methods are sometimes necessary for other tools such as JavaBeans (requires setters and getters) and JavaFX (@FXML methods are not detected as 'used')

Thus, we should still exercise discretion in using this tool even if it is something as simple as removing unused variables or methods.

## FileSystems
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
 
## GitHub
### Deployments and Environments
- [Environments](https://docs.github.com/en/rest/reference/deployments#environments) can be viewed as the platform for which deployments are staged. There are generally fewer of them. For example in RepoSense, there is roughly two environments per active pull request for deployments.
  - Environments can be viewed on the main page of a repository. 
  - They will linger so long as the deployment on the environment continues to exist and would normally require manual deletion.
- [Deployments](https://docs.github.com/en/rest/reference/deployments) are requests to deploy a specific version of the repo such as a pending pull request. In the context of RepoSense, a single PR can have several tens of deployments if it is consistently updated.
  - It is generally difficult to track and control deployments on the GitHub page itself.
  - However, through the GitHub API, we can query all deployments relating to an environment and deleting them. This will automatically remove the environment from the listing as well. This solution was taken from this [stackoverflow post](https://stackoverflow.com/a/61272173).

### GitHub API
The GitHub API provides us with a way to interact with repositories via a RESTful API. Using `curl` commands:
- We are able to query (via GET) for information such as branches, deployments and environments
- We are also able to POST commands to a repository to perform various actions such as deleting deployments. These generally require a security token which might not be available from a personal computer's CLI. When running GitHub Actions, it is possible to acquire one for the repository to perform actions such as closing deployments.

### GitHub Actions
We must add GitHub action workflow files to the directory `.github/workflows` in order to automatically perform certain scripts on certain GitHub actions.
- The general workflow of a `.yml` workflow file contains a declaration `on` which states under what scenarios will these actions be triggered
  - It is followed by a list of jobs. For each job, we can declare a name, platform to run on, environment variables, followed by sequential steps to perform.
  - Under steps, we can use `run` to run shell scripts similar to running the same command from a BASH terminal 

## Git Specifications:
### Git Diff Output
The `git diff` command is used heavily by RepoSense for analyzing. The following are behaviour in the output that I learnt:
- The filename `/dev/null` represents a non-existent file. A mapping from `/dev/null` to a file indicates a new file.
  Similarly, a mapping from a file to `/dev/null` indicates a deleted file.
- For filenames containing certain special characters, the listing of filenames in the before-after comparison is adjusted slightly.
  (I.e. the lines `--- a/some/file/name` or `+++ b/some/file/name`).
  - For files containing spaces, the filename will have a tab character at the end. E.g. `+++ b/space test\t`.
  - For files containing special characters (not including space) such as `\`, `"`, `\t`, the filename will be placed in quotation marks. E.g. `+++ "b/tab\\ttest/"`
  - For files containing both of the above cases, the filename is first wrapped in double quotation marks followed by a tab character.
- These nuances in `git diff` filename output may be important for filename designation as is done in RepoSense.

### Git Log Output
Every commit in the log output displays the hash, author with email, date and message.
- If a user has set an email in their `git config` but set their Github 'keep my email address private' setting to true, web-based Git operations will list their email as `username@users.noreply.github.com`.
- However, it is possible to explicitly set the email to be empty through `git config --global user.email \<\>` which will set it to `<>`. u 

### Git Commit & Config Interaction
`git commit` commit information details can be found [here](https://git-scm.com/docs/git-commit#_commit_information).
- When committing with a `user.name` and `user.email` that matches a GitHub account, commits on GitHub will be able to link the GitHub account to the commit.
  - However, committing with an empty email `<>` will cause the commit to look like [this on GitHub](https://github.com/reposense/testrepo-Alpha/commit/2536b8e0de3366989f4b124b9a5a613db010379e) where there is no account linked to it.
- It is possible temporarily set `user.name` as `<>`. However, it will not allow the user to commit, citing an invalid username.
  - It is also possible to set `user.name` as an empty string which is equivalent to resetting it. Git will not allow commits until a `user.name` has been set.

## Javascript
New language for me, no prior experience. Learnt how to read and interpret most of the lines of Javascript in RepoSense.
- Javascript has `Object`s which are a container of key-value pairs listing its properties, and some pre-defined methods. 
  - Can perform operations on `Object`s similar to what can be done to a map. Use `Object.entries(object)` to enumerate the key-value pairs.
  - The properties of an object can be created, edited and accessed from anywhere more similar to languages like Python's OOP
- Javascript uses `(args...) => output` to write lambda functions as opposed to `(args...) -> output` for Java.
- Javascript has something known as object destructuring where we can extract the properties of an object.
  - Given something like `author = {name: JD, repo: RepoSense}`, then doing `const {name, repo} = author` would allow us to access the `name` and `repo` as local variables.

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
