### Cypress

Cypress is a tool for testing anything that run in a web browser. It is fast and easy to use. It allows the user to write reliable tests. Cypress can cover unit tests, integration tests as well as end-to-end test. Cypress also records down all the states during test for easy debugging.

I learned how to set up a new Cypress test, fetch the target value and add assertions by using the [Official Cypress Documentation](https://docs.cypress.io/guides/getting-started/writing-your-first-test). This was used in of one the PRs when I modify some frontend behaviours.

I also learned about the best pratices as well as some common anti-patterns when writing Cypress tests. One issue was raised over the use of `wait()` most of our Cypress tests. This is actually an anti-pattern as described in the guide. In short, the previous command will not resolve until a response is received (or timeout in a negative case). Therefore, there is no need to explicitly wait. The use of `wait()` in the context only slows the the test unnecessarily. After removing the use of `wait()`, the local test speed was reduced from an average of 135 seconds to 60 seconds, which is quite a significant improvement. For more details, please visit [Cypress Best Practice](https://docs.cypress.io/guides/references/best-practices).

### Migration from Vue 2 to Vue 3
RepoSense uses Vue for frontend. The current Vue version used is Vue 2. Vue 3 was a major upgrade introduced in September 2020. It is a major revamp from Vue 2. It is faster, lighter and introduce better TypeScript support. Migrating to Vue 3 is inevitable from a long term perspective. However, with such a major upgrade, many of the exisiting sytanx are no longer supported/changed. This bring a lot of issue during migration. The [Vue 3 Guide](https://v3.vuejs.org/guide/migration/introduction.html#breaking-changes) summarises the breaking changes.

The migration process is also long and tedious. The [Official Vue Migration Build](https://v3.vuejs.org/guide/migration/migration-build.html#overview) provides a clear path to upgrade. This is also the one that I am currently using to work on the migration.

### (Vue) `v-if` and `v-for`
In Vue 2, when using `v-if` and `v-for` on the same element, `v-for` would take precedence. In Vue 3, `v-if` will always have the higher precedence than `v-for`. In the migration guide, it is stated that:
```
It is recommended to avoid using both on the same element due to the syntax ambiguity.

Rather than managing this at the template level, one method for accomplishing this is to create a computed property that filters out a list for the visible elements.
```
This makes a lot of sense. When we mix if and for together, we might end up confusing ourselevs and introduce bugs that are diffifcult to catch. When we operate on a filtered list, there is no ambiguity is the code. Not only does this help us to avoid bugs, it also makes the code easier to read and easier for someone else to understand.

### (Vue) `v-if` vs `v-show`
`v-if` is a “real” conditional rendering as it is created/destroyed when the condition is true/false. `v-if` is also lazy in the sense that the condition within the conditional block will only be rendered until the condition is true.

On the other hand, `v-show` always renders the element regardless of the initial condition. The visibility is controlled by the css.

In general, `v-if` has higher toggle cost because it needs to render the element when the condition is true. However, it can be used to save some initial loading time if it is false initially. `v-show` might incur higher initial reader costs because everything is rendered on loading, but the toggling cost is very little. The general advice is that if you need to toggle the element very often, use `v-show`. Use `v-if` if the condition is unlikely to change during runtime.

### (Vue) Watchers
Computed properties allow us to compute derived values declaratively. However, sometimes the values changes due to side effects of the program. Watchers allow us to trigger a function whenever a property changes.

We often use watcher on an array property. In Vue 2, the watcher is triggered whenever something within the array changes. However, this is not the default setting in Vue 3. In Vue 3, the watcher will only be shallow. Meaning that it will only be triggered when the watcher property has been assigned a new value. Any inner value changes will not trigger the watcher to fire. This is likely due to performance optimisation. To enable watcher action on all nested mutation, we need to use a deep watcher. The following code snippet is an example.

```
watch: {
    someObject: {
        handler(newValue, oldValue) {
            // action to take
        },
        deep: true // Needed in Vue 3 for mutation of nested values
    }
}
```
### Checking Plugin GitHub Issue Page
When dealing with a plugin (debugging/upgrading), we sometimes face some unexplained behaviours. I spent hours debugging and searching for decumentation only to realise that it is a bug of the plugin. Always check the issue page of the plugin when in doubt, especially when it comes to compatibility issues after upgrading the plugin or other plugins. 

### Opening Issues
When opening an issue, we should make the problem clear. We should explicitly state the difference and the expected behaviour so that anyone can understand the issue clearly. An unclear issue may lead to misunderstanding and lead to hours wasted on an unrelated work.

### Working with JSON files
Never put any comments in `.json` files. Comments of the form `//...` and `/*...*/` are not allowed. Some IDEs do not flag out this as an error and such files might be able to be used normally. However, when error arises due to this issue, it is extremely difficult to find the error as the error message often do not point to the incorrect files.