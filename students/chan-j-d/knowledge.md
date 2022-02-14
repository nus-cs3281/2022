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

### "file" URI Scheme
- Specification can be found here [RFC8089](https://datatracker.ietf.org/doc/html/rfc8089).
  - A file URL looks like `file://host.example.com/path/to/file`.
    - Double slashes following the colon `://` indicates that the file is not local and `host.example.com` is the 'authority' which hosts the file
    - Single slash or triple slashes after the colon `:/` or `:///` will both be treated as a local file. Everything after the last slash forms the path to the file.
 
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

