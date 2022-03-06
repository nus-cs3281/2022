<panel header="### Creating & Publishing a NPM package" type="seamless" peek>
While I use packages from NPM all the time, working on MarkBind has motivated me to create and publish a NPM package from start to finish.
I chose to create a plugin for Markdown-it, a Markdown parser that is used by MarkBind in the process of converting Markdown to HTML. In 
the process of creating the plugin, I followed a tutorial by Kent C. Dodds and explored a few development tools such as 
- mocha and chai for testing, 
- semantic-release and commitizen for releasing the package, 
- Babel for transpiling the code, 
- Github Actions for automating the update.

Resources
- [How to Write an Open Source JavaScript Library](https://egghead.io/courses/how-to-write-an-open-source-javascript-library)
</panel>

<panel header="### Regular Expression" type="seamless" peek>
Regular expressions are a powerful way to match patterns in text. In JavaScript, basic checks to find out whether a string is
a substring can be done via the `includes` method. However, this approach may not be robust when we want to specify an exact
match.

For example:
```javascript
const existingClass = node.attribs.class;
if (existingClass
    && (existingClass.includes('line-numbers') || existingClass.includes('no-line-numbers'))) {
        return;
    }
// ...
```
in the above piece of code, the intention was to detect whether the class attribute of a HTML node contains the string 'line-numbers' or
'no-line-numbers'. However, the use of `includes` will wrongly match 'line-numbers-1' or 'foo-line-numbers'.

To match the exact pattern (for example just the word 'line-numbers'), we can use regular expressions.

In total, there are 4 cases that we need to handle:
- 'line-numbers' is at the start of the class attribute
- 'line-numbers' is in the middle of the class attribute
- 'line-numbers' is at the end of the class attribute
- 'line-numbers' is the only word in the class attribute

Thus to match it with regular expressions, we can use the following pattern:
```javascript
const existingClass = node.attribs.class || '';
const regex = /^line-numbers\s|\sline-numbers\s|\sline-numbers$|^line-numbers$/;
if (regex.test(existingClass)) {
    return;
}
```
This can be simplified by using the alternate operator `|`:
```javascript
const regex = /(^|\s)line-numbers($|\s)/;
```

To handle both 'line-numbers' and 'no-line-numbers', we can use the following pattern:
```javascript
const regex = /(^|\s)(no-)?line-numbers($|\s)/;
```

Lastly, in cases where we just want to know the existence of a pattern, we can use JavaScript's `test` method instead of `match` for better performance.

Resources
- [Onlilne Regex tool](https://regex101.com/)
- [Discussions around the examples above in MarkBind #1734 PR review](https://github.com/MarkBind/markbind/pull/1734)
</panel>

<panel header="### Devops & CI" type="seamless" peek>

#### npm i vs npm ci
The two commands are both used to install dependencies from the npm registry. However, to keep the installation process relabilable,
npm ci (stands for npm clean install) can be used in CI environments to ensure a fresh installation.

- [npm i](https://docs.npmjs.com/cli/v8/commands/npm-install)
- [npm ci](https://docs.npmjs.com/cli/v8/commands/npm-ci)
- [stackoverflow post](https://stackoverflow.com/questions/52499617/what-is-the-difference-between-npm-install-and-npm-ci)

#### GitHub Workflow
I have summarized what I learned from configuring the CI script in this [article](https://dev.to/tlylt/intermediate-github-ci-workflow-walk-through-1j6p).
The interesting parts are:
- How to run tests on various OSes
- How to run jobs dependent on one another and on certain conditions
- How to run steps based on certain conditions

#### Semantic versioning
Git tags
- Lightweight tag just points to a commit
	- `git tag <tagName> [commit]`
	- `git show <tagName>`
- Annotated tags
	- Full object in git database
	- `git tag -a <tagName> -m"<annotation>" [commit]`

Semantic versioning
- Major version
    - `1.0.0`
- Minor version
    - `1.1.0`
- Patch version
    - `1.1.1`

Annotated tags + semantic versioning = Semantic Releases

Typical steps for a release:
1. Create a annotated tag
2. Push tag to remote repository
3. Deployment
4. Release Notes

- [Semantic Version](https://semver.org/)
- [Manage releases with Semantic Versioning and Git Tags](https://www.youtube.com/watch?v=4wPjo5C-v8Y)
</panel>

<panel header="### Git & GitHub" type="seamless" peek>

#### Git
##### Context
In a feature branch, I updated the .gitignore to ignore some generated build files. When I wanted to switch over to the master branch and start work on another feature, the build files showed up because the master branch's .gitignore has not been updated yet. I was unsure how to create a new branch from master and work on a new feature, without the build files showing up as unstaged changes. The branch that contains the build files cannot be merged yet because the feature is still undergoing review.

This prompted me to post my first [stackoverflow question](https://stackoverflow.com/questions/71316258/how-to-deal-with-files-ignored-in-a-feature-branch-but-showed-up-when-switched-t):
> How to deal with files ignored in a feature branch but showed up when switched to the master branch?

Thanks to the developer community, I was able to get the following solution that I can use.
> just repeat the same changes to the .gitignore file on another branch, but leave it as a pending change and don't commit it

</panel>

<panel header="### HTML" type="seamless" peek>

#### HTML
Deprecation, in a technical sense, is the discouragement from using an old feature.
In HTML, there are many elements that get deprecated over time to improve the language. The main reason behind it is a separation of concerns.
The introduction of external stylesheets (CSS) means HTML has evolved to focus on content, and not on style.

Some reasons for deprecation of certain elements that I gathered from the reference article are:
- Avoiding duplication
- Ease of management
- Readability
- Caching
- Developer specialization
- User options
- Responsiveness and device independence

In fact, some of the above reasons might also be applicable to other long-lasting software projects.
One interesting thing about decrecation is that it is definitely not easy to ask people to make changes.
Sometimes it takes forever for developers to migrate their code over, perhaps because of the mantra that "if it works, it works".
Even when deciding on what features to decrecate, it could be a difficult argument to make. One unexpected thing that I discovered about HTML element deprecation is that althought the `<big>` tag is now deprecated, the `<small>` tag is not. When the use of a feature is so widespread, it is possible to
derive semantic meaning to save it from deprecation.

- [Why <big> is not in HTML 5 Tag list while <small> is?](https://stackoverflow.com/questions/2260024/why-big-is-not-in-html-5-tag-list-while-small-is)
- [Why Do Some HTML Elements Become Deprecated?](https://css-tricks.com/why-do-some-html-elements-become-deprecated/)
</panel>