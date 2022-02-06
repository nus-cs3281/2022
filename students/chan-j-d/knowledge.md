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

### Special Relative and Absolute File Pathing Characters
- Both `./` and `../` function similarly in both UNIX and Windows (replacing `/` with \\).
- `~` is an absolute pathing feature used in [Bash](https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html#Tilde-Expansion) which expands to the
user's home directory.
  - If wrapped within quotation marks, it becomes a literal `~` char in the file path. Thus, to use both together, 
  the tilde has to be left out of the quotation marks: `~/"some test file"`.
  - `~` does not work in Windows command prompt but does work in Windows PowerShell