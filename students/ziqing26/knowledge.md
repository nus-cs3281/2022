## Angular
Angular is a development platform built on TypeScript. There are three types of Angular directices in general:

1. Components: directives with a template.
2. Attribute directives such as `NgClass` and `NgStyle`: directives that change the appearance or behaviour of an element.
3. Structural directives such as `NgIf`, `NgFor` and `NgSwitch`: directives that change the DOM layout by adding and removing DOM elements.
4. Some less obvious built-in directives: `a, form, input, script, select, textarea`.

There are two types of forms in Angular:

1. Reactive: Reusuable and synchronous data flow between the view and the data model.
2. Template-driven: TEAMMATES generally uses this type of forms. It focuses on simple scenarios despite being not reusable.

Modules vs Directives vs Services:

1. Modules provide a way to namespace services, directives and filters. It helps avoid global variables. 
2. Services are singletons. BUIlt in services start with `$`. Dependency injection is required on the dependent.
3. Directives allow componetized HTML. 

Pipes encapsulates custom transformations and could be used in template expressions. We can chain pipes using a series of pipe operator `|` in templates.

Binding:

1. Property binding: sets a specific elemtn property. (e.g. `[disabled]="isNotificationEditFormExpanded"`)
2. Event binding: listens for an element change event. (e.g. `
3. Two-way binding
4. We can use `@Input()` to receive data from parent and `@Output()` to send data to parent.

Resources:

[Angular Developer Guide Overview](https://angular.io/guide/developer-guide-overview)
, [Tour of Heroes app and tutorial](https://angular.io/tutorial)

[Introduction to Service Worker](https://developers.google.com/web/fundamentals/primers/service-workers/o), [Service Worker and PWA in Angular](https://morioh.com/p/984afc91af1c)

## d3.js
D3.js is a JavaScript library for manipulating documents based on data using HTML, SVG, and CSS. It is flexible due to its low-level approach that focuses on composable primitives such as shapes and scales rather than configurable charts.

Resources:

[d3 Tutorials](https://observablehq.com/@d3/learn-d3)

## OAuth2.0
TEAMMATES staging and production server uses [OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749) for authorization.

Resources:

[Using OAuth 2.0 with the Google API Client Library for Java](https://developers.google.com/api-client-library/java/google-api-java-client/oauth2)

## Google Cloud Datastore and Objectify
TEAMMATES uses Google Cloud Datastore as database.
Through the onboarding task, I learnt about keys-only query which is a subtype of projection queries. Such queries allow querying for specific properties with lower latency and cost. From project of notification feature, I got the chance to apply my knowledge of key-only queries on GET API to fetch the ids of all notifications such that it saves cost for checking whether a notification is in the database or not.

I also learnt about composite index which index multiple property value per index entity. It needs to be configured in an index configuration file.

I learnt that eventually consistent queries generally run faster but may return stale results compared to strongly consistent queries which guarantee the most updated result but take longer to run. As we move from Datastore to Firestore in Datastore mode, it becomes strongly consistent instead of eventually consistent.

Resources:

[Documentation Guides on Datastore Queries](https://cloud.google.com/datastore/docs/concepts/queries#projection_queries)

[Data Consistency in Datastore Queries](https://cloud.google.com/appengine/docs/standard/java/datastore/data-consistency)

## Backend 
### Workflow
As I built API for notification features, I learnt the backend workflow in TEAMMATES, from endpoint design, to access control and getActions, to execute the action, to use Logic and Db layers to create result. I learnt how to use Postman when designing API endpoints.

## Testing

### Frontend - Jest

1. `jest.spyOn(object, methodName)` allows tracking calls to `object[methodName]` and creating a mock function. The spied methods are useful for mock implementation of services in frontend testing.
2. Snapshot testing renders a UI component, takes a snapshot and then compares it to a reference snapshot file stored together with the test. Snapshot testing is great when you want to validate the structure of something like a component or an object.

Resources:

[Jest Snapshot Testing documentation](https://jestjs.io/docs/snapshot-testing)

### Backend

1. I learnt how to use `dataBundle` to create different instance of testing objects.
2. Test Driven Development is helpful especially to catch bugs before fixing it.

### E2E - Selenium, PageObject

1. Selenium provides extensions for website test automation. `Selenium WebDriver` APIs identifies Web elements. WebDriver provides bindings which support classes. It has two-way communication with broswer through a driver (eg. ChromeDriver). WebDriver passes commands to Broswer through driver, while receives information back via the same route.

2. Selenium helps identify web elements using locator strategies (e.g. class name, css selector, id, name or link text). `findElement` method will return the first element found in the context. `findElements` returns all elements matching a locator.

3. Selenium helps interact with web elements. Basic commands are `click`, `send keys`, `clear`, `submit` and `select`. `select` could be useful to selection an `<option>` in a `<select>` element.

4. PageObject design pattern is useful to model UI as objects in test code, reducing duplicated code. The public method in page object represents serivices that the page offers with proper abstraction. They return page objects. `PageFactory` package is used in TEAMMATES.
