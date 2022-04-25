### Frontend

#### Angular

Angular is the framework used for the TEAMMATES frontend, and it has the following features:
- Components and services are organized into NgModules, according to their usage
- Decorators such as `@Component()` and `@Injectable()` to define whether each class is a component or service
- Component templates dictate how to render the component, with directives to apply application logic (e.g. `*ngIf` to render a component conditionally, `*ngFor` to iterate over a list)
- Services shared across components are injected as dependencies
- `@Input` and `@Output` are used to share data between parent and child components
- Lifecycle hooks such as `ngOnInit` are provided for use when there are changes made to the component
- Availability of pipes to transform data displayed in templates

Other than the above features, some of the important things I learnt were:
- Understanding the usage of lifecycle hooks - Similar to the React `useEffect` hook, the Angular `ngOnInit` and `ngOnChanges` hooks can be used to respond to any changes in the component. One important thing to note is the difference between the component constructor and the `ngOnInit` hook - the former is usually used to set up dependency injection, while the latter is called once the component has finished instantiation. One key difference is that variables passed using `@Input` can only be accessed in `ngOnInit`.
- Writing custom pipes that can be used throughout the application to display data such as dates in a specific format.

Resources:
- [Angular Tutorial](https://v9.angular.io/tutorial)

#### Jasmine/Jest

The TEAMMATES frontend uses Jasmine/Jest for component testing. Its features include:
- Running setups shared across all tests in a test suite using `beforeEach()`
- Mocking services and methods using `spyOn()` and `callFake()`
- Using expectations (`expect()`) to ensure correct behaviour
- Generating snapshot tests to check the display of the component once it has been rendered

Through TEAMMATES, I have learnt to use tests effectively in various scenarios, for example:
- Using snapshot tests to check that the display of the component is correct at different states (e.g. loading, loading failed, loaded with data)
- Designing tests that test the expected behaviour (success/failure) of the component (e.g. by checking the expected result, checking that a function has been called, etc)

Resources:
- [Jasmine Tutorial](https://jasmine.github.io/tutorials/your_first_suite)

### Backend

#### Google Cloud Datastore

TEAMMATES uses the Google Cloud Datastore as its database (now updated to Firestore in Datastore mode, making it strongly consistent). Each entity in the database has a unique key as well as one or more properties. One thing to note is that Datastore does not enforce the structure of entities, and therefore it is important for TEAMMATES to enforce checks on the database values, as it may cause issues where adding an attribute to an existing entity type causes `null` to be returned when accessing the value of the attribute for entities that are already in the database.

Though Datastore is fast and highly scalable, it comes with its limitations:
- All properties used in queries have to be indexed. Though this speeds up the query process, it uses up storage space and therefore may lead to higher costs.
- A query can only contain inequality filters on at most one property, so as to avoid scanning the entire index. To query for entities using inequality filters on multiple attributes, we need to make multiple queries and combine their results.
- Pricing is based on entity reads and writes, hence improper management of the database can lead to high costs. There are ways to reduce such costs, such as using key-only queries.

Resources:
- [Datastore Query Documentation](https://cloud.google.com/datastore/docs/concepts/queries)

#### Java Serialization

Objects in Java can be serialized into a stream of bytes so that it can be saved in a database. Java data structures such as `HashMap` are serializable by default, but any class can implement the Serializable interface, as long as all the fields in the class are serializable. If a field cannot or need not be serialized, the `transient` keyword can be used to exclude it from the serialization.

Resources:
- [Java Serialization tutorial](https://docs.oracle.com/javase/tutorial/jndi/objects/serial.html)

### End-to-End Testing

#### Selenium

TEAMMATES uses Selenium, which is a set of tools that enable the automation of web browsers, for end-to-end (E2E) testing. Its features include:
- Opening and navigating a web browser using `WebDriver`
- Finding web elements by id, class name, etc
- Clicking elements such as buttons
- Retrieving element attributes

By using the above features, the testing of various user flows from start to finish is automated, so as to ensure that the application is working as intended for all of the application's use cases.

In designing E2E tests, I learnt about the Page Object Model, where all code related to a certain page in the application is encapsulated in a Page Object, such as methods for interacting with the page or checking the page contents.

Resources:
- [Selenium tutorial](https://www.selenium.dev/documentation/webdriver/getting_started/first_script/)
