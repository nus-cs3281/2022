### Cypress

Cypress is a tool for testing anything that run in a web browser. It is fast and easy to use. It allows the user to write reliable tests. Cypress can cover unit tests, integration tests as well as end-to-end test. Cypress also records down all the states during test for easy debugging.

I learned how to set up a new Cypress test, fetch the target value and add assertions by using the [Official Cypress Documentation](https://docs.cypress.io/guides/getting-started/writing-your-first-test). This was used in of one the PRs when I modify some frontend behaviours.

I also learned about the best pratices as well as some common anti-patterns when writing Cypress tests. One issue was raised over the use of `wait()` most of our Cypress tests. This is actually an anti-pattern as described in the guide. In short, the previous command will not resolve until a response is received (or timeout in a negative case). Therefore, there is no need to explicitly wait. The use of `wait()` in the context only slows the the test unnecessarily. After removing the use of `wait()`, the local test speed was reduced from an average of 135 seconds to 60 seconds, which is quite a significant improvement. For more details, please visit [Cypress Best Practice](https://docs.cypress.io/guides/references/best-practices).

### Vue 2 vs Vue 3
RepoSense uses Vue for frontend. The current Vue version used is Vue 2. Vue 3 was a major upgrade introduced in September 2020. It is a major revamp from Vue 2. It is faster, lighter and introduce better TypeScript support. Migrating to Vue 3 is inevitable from a long term perspective. However, with such a major upgrade, many of the exisiting sytanx are no longer supported/changed. This bring a lot of issue during migration. The [Vue 3 Guide](https://v3.vuejs.org/guide/migration/introduction.html#breaking-changes) summarises the breaking changes.

One notable example that I encounter was the use of `v-if` vs `v-for`. In Vue 2, when using `v-if` and `v-for` on the same element, `v-for` would take precedence. In Vue 3, `v-if` will always have the higher precedence than `v-for`. In the migration guide, it is stated that:
```
It is recommended to avoid using both on the same element due to the syntax ambiguity.

Rather than managing this at the template level, one method for accomplishing this is to create a computed property that filters out a list for the visible elements.
```
This makes a lot of sense. When we mix if and for together, we might end up confusing ourselevs and introduce bugs that are diffifcult to catch. When we operate on a filtered list, there is no ambiguity is the code. Not only does this help us to avoid bugs, it also makes the code easier to read and easier for someone else to understand.

The migration process is also long and tedious. The [Official Vue Migration Build](https://v3.vuejs.org/guide/migration/migration-build.html#overview) provides a clear path to upgrade. This is also the one that I am currently using to work on the migration.