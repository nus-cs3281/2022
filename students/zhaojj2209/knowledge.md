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

### Google Cloud Datastore

TEAMMATES uses the Google Cloud Datastore as its database. Though it is fast and highly scalable, it comes with its limitations:
- All properties used in queries have to be indexed. Though this speeds up the query process, it uses up storage space and therefore may lead to higher costs.
- Pricing is based on entity reads and writes, hence improper management of the database can lead to high costs. There are ways to reduce such costs, such as using key-only queries.

Resources:
- [Datastore Query Documentation](https://cloud.google.com/datastore/docs/concepts/queries)
