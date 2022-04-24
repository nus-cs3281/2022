## Frontend

### Rounded vs Square edges for signalling functionality

Researching whether to use rounded corners, sqared off corners, or fully rounded boxes was interesting from a usability perpective. Some resources I used to learn about them:

* https://ux.stackexchange.com/questions/40744/mixing-rounded-corners-and-square-corners
* https://medium.com/@carolinalina/how-to-design-ui-buttons-that-convert-d5ebb1080969
* https://prototypr.io/post/the-rounded-user-experience/

The information from https://uxdesign.cc/make-sense-of-rounded-corners-on-buttons-dfc8e13ea7f7 in particular made a case for fully rounded buttons for primary content when you have space to spare, to direct users attention to those buttons. They suggested to avoid fully rounded buttons when many are used next to each other as it may not be obvious which to click. I used this information to infer what the average user might takeaway if minimized panels were pills rather than rounded buttons.
### Vue

* https://v1.vuejs.org/guide/instance.html
* Scoped styles: https://vue-loader.vuejs.org/guide/scoped-css.html, also informing the issue I created [#1768](https://github.com/MarkBind/markbind/issues/1768) in MarkBind
* Learning about slots: https://learnvue.co/2021/03/when-why-to-use-vue-scoped-slots/#conclusion, https://www.smashingmagazine.com/2019/07/using-slots-vue-js/
### Scrollbars

Using overflow-x: scroll on the default navbar, seemed to cause the dropdown to break.

After a few stack overflow posts and reading, I found this article: https://css-tricks.com/popping-hidden-overflow/ that explains that setting `overflow-x` sets `overflow-y` as well, and it's just not possible to have a dropdown peep out of a scrollable overflow without setting positions relatively. [This discussion](https://www.sitepoint.com/community/t/css-drop-down-menu-hidden-behind-horizontal-scrollbar/367783) with the offered [solution](https://codepen.io/paulobrien/embed/vYxWppv?) was also interesting.

I briefly explored existing libraries like https://floating-ui.com/. Libraries like this exist to make it easier to accomplish this surprisingly complex task.

I also learned about the accessibility of scrollbars (https://adrianroselli.com/2019/01/baseline-rules-for-scrollbar-usability.html) and (https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/proposed/), which discussed what goes into making scrollbars accessible. Visually, visible scrollbars provide an obvious indication that there is more content. These design tips on scrollbars (https://www.nngroup.com/articles/scrolling-and-scrollbars/) were also interesting, particularly the note to avoid horizontal scrolling wherever possible. 

This informed my decision that it would be better not to make a scrollable navbar the default, but have a dropdown menu with more options for smaller screens

[]::webkit-scrollbar](https://developer.mozilla.org/en-US/docs/Web/CSS/::-webkit-scrollbar) pseudo-element does not work for all browsers and should be used with caution.

## Open source dependencies

### ghpages

Used when researching the deploy and build commands for MarkBind.

* https://github.com/tschaub/gh-pages
### Commander

Used to write CLI programs.

* https://www.npmjs.com/package/commander
* https://en.wikipedia.org/wiki/Usage_message (conventions in defining parameters)
### jest

Mainly studied the changelog to see if this would break when dependencies were updated.

* Introduction: https://jestjs.io/ and repository(https://github.com/facebook/jest)
* relevant blog post: https://jestjs.io/blog/2021/05/25/jest-27 
* [changelog](https://github.com/facebook/jest/blob/main/CHANGELOG.md#2700)
* Jest testrunners: they plan on changing the default test-runner from `jasmine2` to `jest-circus` in version 27, with `jasmine2` [to be discontinued afterwards](https://jestjs.io/blog/2020/05/05/jest-26). Though I think we're using `jasmine2` and not `jest-circus`, but MarkBind we never explicitly specify a change from the default

### fs-extra

Handy utility that I ended up using extensively
* https://github.com/jprichardson/node-fs-extra
## Git

CI pipeline (particularly with git):
* https://docs.github.com/en/actions/automating-builds-and-tests/about-continuous-integration, particularly the section on [github actions](https://docs.github.com/en/actions/automating-builds-and-tests/about-continuous-integration#about-continuous-integration-using-github-actions)
* Follow-up research about github actions
  * https://docs.github.com/en/actions/quickstart
  * https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions
* Basic research about [Travis CI](https://travis-ci.org/) and [Netlify](https://www.netlify.com/)

## Logging Framework

* https://www.sentinelone.com/blog/logging-framework/ as an introduction
* https://se-education.org/se-book/errorHandling/#-12 also as an introduction
* https://github.com/winstonjs/winston (library used with markbind)

## Ways Versioning is Implemented

* Learn about semantic versioning: https://semver.org/
* Alternate versioning solutions:
  * https://github.com/jimporter/mike
  * https://docusaurus.io/docs/versioning

## Javascript

### Javascript with regard to object oriented programming

Looking into this was inspired by the issues on refactoring the large `core/site/index.js` file which is over 1.5k lines into more manageable class. At present, most of the file is made up of the `Site` class, which makes sense from an object oriented perspective. All the functions which are supported by Site are things which affect what the site itself holds or does: generating itself, deploying itself, initialising itself.

One suggestion for refactoring would be separating out each command into separate files. We could abstract away the command logic might be separating each command into classes, having each command inherit from a Command class, and having the site class just generate and execute each command when it is called to do so. But is this necessary or desirable?

Java and Javascript are different in that Java is class based and Javascript is prototype-based. Class based languages are founded on the concept of classes and instances being distinct, where classes are an abstract description of a set of potential instances which have the properties defined in the class - no more and no less. Prototype based languages have a 'prototypical object' which is the template used to create a new object, but once you create it or at run time the object can specify its own additional properties and be assigned as the prototype for additional objects (source: [mozilla, class-based vs prototype based languages](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model))

Nevertheless, Site.js does use "classes" of managers to manage externals, etc, so perhaps in production avoiding classes is not a big deal. Would still be a useful abstraction to manage the complexity of the file. 

### Certain functions in javascript

<panel title="JavaScript forEach (and async loops)">

"JavaScript Array.prototype.forEach loop is not asynchronous. The Array.prototype.forEach method accepts a callback as an argument which can be an asynchronous function, but the forEach method will not wait for any promises to be resolved before moving onto the next iteration." ([Source](https://atomizedobjects.com/blog/javascript/is-javascript-foreach-async/)).

`forEach` does not look at what is returned, and won't handle the promise that an async function would return. Naturally, this means you cannot use async or await with it. The algorithm for forEach creates a loop that calls the callback function for each([StackOverflow Source](https://stackoverflow.com/questions/5050265/javascript-node-js-is-array-foreach-asynchronous), [more information about loops](https://thecodebarbarian.com/for-vs-for-each-vs-for-in-vs-for-of-in-javascript))

Instead, we could use map and the promise 'class' functions.

</panel>

<panel title="Javascript map can be destructive sometimes">

Just needed to note this, and consider other options. Refactoring my code allowed me to avoid destructively modifying the list.
[source](https://dev.to/lofiandcode/
can-map-mutate-the-original-array-yes-dmb)
</panel>
