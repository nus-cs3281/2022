### Angular

Angular is the framework used for the TEAMMATES frontend, and it has the following features:
- Components and services are organized into NgModules, according to their usage
- Decorators such as `@Component()` and `@Injectable()` to define whether each class is a component or service
- Component templates dictate how to render the component, with directives to apply application logic (e.g. `*ngIf` to render a component conditionally, `*ngFor` to iterate over a list)
- Services shared across components are injected as dependencies

Resources:
- [Angular Tutorial](https://v9.angular.io/tutorial)

### Jasmine

The TEAMMATES frontend uses Jasmine for component testing. Its features include:
- Running setups shared across all tests in a test suite using `beforeEach()`
- Mocking services and methods using `spyOn()` and `callFake()`
- Using expectations (`expect()`) to ensure correct behaviour

Resources:
- [Jasmine Tutorial](https://jasmine.github.io/tutorials/your_first_suite)

### Selenium

TEAMMATES uses Selenium, which is a set of tools that enable the automation of web browsers, for end-to-end (E2E) testing. Its features include:
- Opening and navigating a web browser using `WebDriver`
- Finding web elements by id, class name, etc
- Clicking elements such as buttons
- Retrieving element attributes

By using the above features, the testing of various user flows from start to finish is automated, so as to ensure that the application is working as intended for all of the application's use cases.

Resources:
- [Selenium tutorial](https://www.selenium.dev/documentation/webdriver/getting_started/first_script/)

### Google Cloud Datastore

TEAMMATES uses the Google Cloud Datastore as its database. Though it is fast and highly scalable, it comes with its limitations:
- All properties used in queries have to be indexed. Though this speeds up the query process, it uses up storage space and therefore may lead to higher costs.
- A query can only contain inequality filters on at most one property, so as to avoid scanning the entire index. To query for entities using inequality filters on multiple attributes, we need to make multiple queries and combine their results.
- Pricing is based on entity reads and writes, hence improper management of the database can lead to high costs. There are ways to reduce such costs, such as using key-only queries.

Resources:
- [Datastore Query Documentation](https://cloud.google.com/datastore/docs/concepts/queries)
