### Tool/Technology 1

List the aspects you learned, and the resources you used to learn them, and a
brief summary of each resource.

### Special characters encoding on URI

On windows, file paths are separated by the directory separator character which
is a `\`. This is different from the linux and mac's `/` separator. When
building tools to be used cross-platform, it is important to be able to parse
and encode file paths correctly. This is especially important when dealing with
URI since they only accept 84 characters with `\` not being one of them. Hence,
if a URI contains a `\` character, it will be encoded as `%5C` and may cause
issues with the path on a webpage. Read more about this
[here](https://stackoverflow.com/questions/1547899/which-characters-make-a-url-invalid)
