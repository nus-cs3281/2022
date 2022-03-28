### Project: [Godot](https://github.com/godotengine/godot)

Godot Engine is a feature-packed, cross-platform game engine to create 2D and 3D games from a unified interface. It provides a comprehensive set of common tools, so that users can focus on making games without having to reinvent the wheel. Games can be exported with one click to a number of platforms, including the major desktop platforms (Linux, macOS, Windows), mobile platforms (Android, iOS), as well as Web-based platforms (HTML5) and consoles.

Godot, released in 2014, is currently the most popular open source game engine. However it still lags behind established proprietary game engines such as Unity or Unreal Engine which already have large communities, having been released much earlier than Godot. Godot is written almost completely in C++.

### My Contributions

For my contributions, I focused on the issues related to the user interface components and compiler. 

1. PR: [Select save dialog file name after popup](https://github.com/godotengine/godot/pull/56457)

I started off by working on a seemingly simpler issue, selecting the file name in the dialogue box whenever a new file is created. For this task, I had to first locate the respective components that contain the dialogue boxes which was much harder than expected. This was as the UI was completely built from ground up using almost no external libraries. I had to trace the exact function which handles the window popping up, and specify the exact characters to highlight in the selection.

2. PR: [Fix shortcut collapse after edit](https://github.com/godotengine/godot/pull/56476)

By my second contribution, I was more familiar with the code base, and I fixed an issue regarding how tree traversal was not handled properly. The existing code checked for the existence of a sibling of the parent, which caused a bug when the parent was an only child. This caused some lists opened in the UI to misbehave.

3. PR: [Add shortcut_cell double click functionality](https://github.com/godotengine/godot/pull/56503)

In my third contribution, I added a feature to the options menu in which double clicking an shortcut assignment would allow one to immediately assign a key to it. Although it seemed like a big feature to add, I was surprised by how most of the required functions required already exist. For example, a signal is already sent whenever the shortcut assignment is double clicked. I only had to figure out exactly which signals and variables were relevant. However there were many inadequately named variables which made it hard to pick apart the noise.

4. PR: [Fix leak when function returning self type](https://github.com/godotengine/godot/pull/56651)

My last contribution was to fix a memory leak in the game script compiler. From my understanding of memory leaks, it can usually be solved by changing a single line. However I underestimated how hard it was to determine which line it was. Godot uses a custom garbage collection mechanism to track object dependencies, which allows one to free objects when all pointers to them are freed. The memory leak can hence easily be inferred to be caused by a circular dependency, in which the ancestor of some object is itself. As a result, the object can never be completely dereferenced and hence freed. The issue can be fixed by informing children about who exactly is referencing them, to prevent objects from tracking self references. There was already a function to do this, but it was not applied to all objects. Unfortunately, I was unable to completely resolve the issue.

### My Learning Record

#### Workflow in Godot
Pull Requests get reviewed by Godot developers and need to pass checks via Github Actions to ensure a stable system after the Pull Request has been merged.

Credits: [Contributing Guide](https://github.com/godotengine/godot/blob/master/CONTRIBUTING.md)

#### Lessons Learnt from contributing to Godot

The preferred IDE for working on C++ is Visual Studio. I learnt how to compile C++ via visual studio as stated in the [Godot compiling guide](https://docs.godotengine.org/en/latest/development/compiling/compiling_for_windows.html). I had also learnt how to use Visual Studio to examine and keep track of references in [memory](https://stackoverflow.com/a/20346096). I also learnt more about the overall structure of garbage collectors, and how signals are propagated through the code.

#### Possible Practices to be adopted by Reposense

1. Use of a public project chat.

Godot encourages aspiring contributors to join the to join the Godot Contributors Chat. It is a somewhat active chat where people are generally willing to help. Currently, Reposense uses Slack as the main messaging platform, which is closed to outsiders. Opening an public messaging chat on discord for example would make it easier for newer contributors to ask general questions about the project.

#### Suggested areas of improvement for the external project

1. Better code documentation.

Most functions are not documented properly. As a result, it can be fairly hard to determine what each function does, as well as what side effects they might cause.