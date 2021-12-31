# Linter and Formatters

Linters are tools that provide static analysis for your code while formatters help to check the style of the code.

There are certain overlaps in the functionality and rule-sets with both linters and formatters, but in general, it will be good to have both as development dependencies in a project.

In the CATcher project (at the current point of writing), we use `tslint` to enforce certain styles. 
This is integrated with continuous integration (CI) in order to ensure a certain quality of pull requests.

However, `tslint` does not fix simple errors that are being caught.
In fact, there are certain rules that are not caught at all, even though it is specified in the rule-set. (e.g. indent-level : 2)
This is where `prettier`, a formatter, should come in to enforce consistency of style across the code base.

Furthermore, `prettier` being an _opinionated_ formatter, means that there isn't much set up to be done. 

`prettier` can be integrated with `tslint` in a few ways.
In general, most projects use a plugin to disable all formatting rules so that they can be handled solely by `prettier`.
`prettier` can also be added as a linting rule and which defers to the linter to fix the formatting errors. 
However, there is usually a lot of _squiggly-underlines_ that cause a lot of distraction and speed of formatting is a little slower.

In this project, I decided to apply `prettier` as a pre-commit hook so that any new changes are automatically formatted. 
This also somewhat decouples the formatter from the linter. 
The formatting rules are still checked by the linter, but the formatter will fix them, as `prettier` doesn't throw errors.

> It is worth noting that `tslint` has been deprecated and future projects should look into using `eslint` for their linting requirements.

## Gitignore Pattern Format

While it is common to see the regular ignore patterns and wildcards in a gitignore file, 
gitignore also comes with a negation pattern that allows for exceptions.

The exceptions are specified using the `!` prefix, and files that start with the `!` literal can be
escaped using backslash `\`.

The most common usecase for would be something like ignoring all files and directories except a specific folder.

However, it is worthy to note that it is impossible to "unignore" a file whose parent folder have been ignored.
This is due to performance reasons.

Therefore a gitignore would look something like this:

```
# ignore everything except /foo/bar
/*
!/foo
/foo/*
!/bar
```

`prettier` in particular, uses the gitignore format in order to choose files that are subject to formatting.
Ignore rules reside in a `.prettierignore` file at the root of the project directory.

# GitHub Actions

GitHub Actions are a way to add automation to the repository. 
Most repositories make use of it to implement continuous integration (CI) and continuous deployment (CD).

One of the interesting things that I noticed about GitHub actions is that all commands are akin to some form of package, 
available through the GitHub Marketplace.

Running a GitHub Action simply requires the action name and a version to be specified in the `actions.yml` file.

# Angular

Angular is a web framework for building responsive websites.
As someone who has experience with building React Applications, the component-based framework is quite different.

Angular applications have 2 main parts:

1. Components
2. Templates

Components define the web component as a Javascript object that stores and update values.
These values are then reflected on the website via templating, which is a way to define variable values in an HTML file.

Decorators are Angular's way of defining the relationship between various actors in an application.
The `@Component()` decorator for example specifies the tags that match the component to the HTML element, as well as the style and template.

## Template Binding

The templates also allow a way for interactions on the webpage to trigger functions in the component to exact changes.

These come in the form of attributes in the HTML element, such as `(click)`, known as **event binding**.

## Dependency Injection

Unlike React whereby everything is pretty much "hand-traceable", Angular provides certain "code magic" behind the scenes, 
such as declaring dependencies via dependency injection.
This allows us to pass in various objects into the constructor of the component, without caring about initialization.

Dependencies can be declared via commandline, or manually by adding a `@Injectable()` decorator.
The `providedIn` field tells Angular the scope at which this dependency is accessible.

## Observables

Observables are Angular's way of passing information between parts of the application, and also used for event handling and data output.

> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of state changes. 

In this sense, it is similar to the case of `Streams` in Java. 
Observables are not executed until subscribed, and for each subscriber, the observable starts a new execution.
Therefore, we may consider this execution to be pure. RxJS also provides a way for subscribers to receive the same values while an execution is in progress, via [multicast](https://angular.io/guide/observables#multicasting).

Use of observables in the CATcher codebase is prevalent. A common pattern that is used is to compose pipes that define how the data passed from an observable should be transformed, and these pipes are then attached to the subscriber,
like how one would use monads.

# Jasmine Test Suite

Jasmine is a testing framework used by CATcher for tests.

Jasmine itself is a very lean set of functions to describe the behavior of functions and match it against its output.

## Test Structure

Jasmine tests are wrapped in a `describe` clause (function).
The wrapper serves as a way to group similar specifications (specs) together and also allowing for nested definitions.

Each spec consists of a string and the a function to be run as a test.
The test comes in the form of **expectations**, which takes an _actual_, the output of a function to be tested, chained with _matchers_, which form a human-readable boolean relation between the actual and the expected result.

One thing to note is that `describe` and `it` blocks can be nested indefinitely as required, and the setup and teardown routines behave as per a depth-first-search.

In CATcher, test files are matched with the directory and the filename of file being tested.
For example, 
```
src/app/phase-team-response/issues-faulty/issues-faulty.component.ts
```
will have a matching test file in 
```
tests/app/phase-team-response/issues-faulty/issues-faulty.component.spec.ts
```
