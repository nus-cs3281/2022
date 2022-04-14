### Sass

Sass is a preprocessor scripting language used in TEAMMATES that compiles to CSS. It extends CSS with additional functionalities such as

1. **Variables:** Variables can be defined as follows: `$variable-name`. Once defined, they can be reused throughout the project. This can be used to help keep things consistent throughout the project. For example, we could define a color scheme to be used throughout the project.

1. **Nesting selectors:** Sass allows one to nest CSS selectors, helping to keep things neat. An added benefit is that the nested CSS selectors follow the same visual hierarchy as the corresponding HTML, making it easier to follow logically. [This commit (authored by me)](https://github.com/TEAMMATES/teammates/pull/11545/commits/84880f5fd01920438f38a15daa10a820d983044c) is an example of how nested CSS selectors can help to make CSS neater, more readeable and improve future maintainability.

1. **Modules and Inheritence:** Sass has powerful functionalities that allows one to compose two stylesheets together, or inherit one stylesheet from another. This helps promote the Don't Repeat Yourself (DRY) principle, reducing repetition in our stylesheets.


1. **Lots more features:** There are lots more features such as mix-ins and operators that I didn't manage to explore! Take a look at the reference if you are interested :)

References: https://sass-lang.com/guide

### Google Cloud Datastore

Cloud Datastore is a NoSql Database used in TEAMMATES. In extremely simplified terms, Cloud Datastore is built upon what are essentially giant immutable maps (SSTable) across several servers(tablets), with servers organised into a hierarchy based on a B+ tree like structure. Software and fancy algorithms are used to coordinate the distributed storage system. A paper on this can be found [here](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf). This architecture results in certain limitations, some of which are discussed below, which we need to keep in mind when working with Cloud Datastore.

#### Queries at scale

Cloud Datastore (like most other NoSql databases) is built to be extremely scalable. Something I found rather interesting was that all queries scale with the size of the result set, not the size of the dataset. While pretty cool, this also [limits the types of queries](https://cloud.google.com/datastore/docs/concepts/queries#restrictions_on_queries) we can make. In particular, inequality filters cannot be applied on more than one attribute (e.g. `startTime > 19:00 && endTime < 20:00`). When we design data models which require complex queries, we need to be careful of what operations are supported.

A common anti-pattern is to use monotonically increasing sequential key names. This reduces performance as it creates "hot-spots" or keys that are close together, resulting in one server handling all the load due to how the database is organised (look up database sharding). An interesting side note is that a two byte scatter key is appended to the end of data properties in indexes (since data properties are used as the key) to increase cardinality of data to ensure database sharding works efficiently.

#### Indexes

Indexes help speed us efficiently query Cloud Datastore for entities. An over-simplified way to think about indexes are a map of property to key of the entitity. While indexes help speed up queries, each entity indexed [requires storage space](https://cloud.google.com/datastore/docs/concepts/storage-size#index_entry_size), calculated as the sum of key size, indexed property values and some fixed overhead. Storing an index incurs storage costs. 

A point to note is that we have to be careful with how many indexes we have. Having too many indexes is an anti-pattern and will result in a longer time before consistency is reached, as discussed in the next section.

#### Consistency

Consistency is an issue for most distributed databases and Cloud Datastore is no exception. One common misconception is that all operations are eventually consistent, [this is not true](https://cloud.google.com/datastore/docs/articles/balancing-strong-and-eventual-consistency-with-google-cloud-datastore). In particular, lookup by key (normal read ops) and ancestor queries are strongly consistent. However, operations that involve indexes are eventually consistent, because it takes awhile to update the indexes. We can get around this by being smart about how we handle consistency issues. For example, after creating an entity, we might want to display all entities of the same type to the user. Apart from using the index to retrieve all the entities we can also query the exact entity we just added in order to give the illusion of strong consistency.

References: 
* https://www.youtube.com/watch?v=0EIqacNVuAo
* https://cloud.google.com/datastore/docs/concepts/indexes
* https://cloud.google.com/datastore/docs/articles/balancing-strong-and-eventual-consistency-with-google-cloud-datastore
* https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf
* https://cloud.google.com/datastore/docs/concepts/queries#restrictions_on_queries
* https://cloud.google.com/datastore/docs/concepts/storage-size#index_entry_size

### Higher-Order RxJs Mapping Operators

[RxJS](https://rxjs.dev/guide/overview) is a library used in TEAMMATES for composing asynchronous and event-based programs using observable sequences. While the basic operators (map, filter, etc.) are relatively straighforward to use, operators that work on higher-order observables can be quite challenging to wrap one's head around.

Higher-order Observables are observables that operate on or emit other observables (analogous to the relationship between functions and higher-order functions). The three most common higher-order mapping operators (operators that work on higher-oder observables) are `concatMap`, `mergeMap` and `switchMap`.

#### Basics

Higher-order mapping operators maps an observable that emits multiple observables (inner observables) into a single observable. As a rule of thumb, if your code contains nested `.subscribes`, you probably want to use a higher-order mapping operator.

#### Differences between concatMap, mergeMap and switchMap

So what's the difference between the three operators mentioned? Well they solve the problem of _how_ the inner observables should be mapped. Since the timing in which observables emit values are not fixed, the order in which they emit values may be random. Depending on our application, we need different strategies to combine the values emitted from these observables into one sequence of values.

* `concatMap`: Use this when sequential ordering is desired. `concatMap` subscribes to the first inner observable. It only subscribes to the second inner observable once the first one is complete.
* `mergeMap`: First come first serve! Subscribe to all observables and emit values in order in which they are emitted by the inner observables.
* `switchMap`: It's like concatMap, except we unsubscribe from the first observable as soon as the second observable starts to emit values. This captures the intent "I only care about the most recent request". 

Refer to this [excellent resource](https://blog.angular-university.io/rxjs-higher-order-mapping/) for a more comprehensive look at higher-order mapping operators including the motivations behind each operator.

References:
* https://rxjs.dev/guide/overview
* https://blog.angular-university.io/rxjs-higher-order-mapping/

### End-to-end Testing with Selenium

E2E testing is used in TEAMMATES to test that different components of the application work well as a whole. Selenium WebDriver is used in TEAMMATES to interact with elements in the UI.

#### How To

In general, most E2E tests in TEAMMATES follow the following steps:
1. Navigate to page: which involved opening the browser via Selenium WebDriver, navigating to the page and ensuring that the correct page has loaded.
1. Interact with UI element: locate the UI element and interact with it, simulating an actual user input. 
1. Verify state: get information about the state of the UI/Database and verify correctness. For example, verify the correct text is displayed and that the correct entity has been persisted into the database.

For more details on how to set up selenium and the available methods, refer to the [selenium documentation](https://www.selenium.dev/documentation/webdriver/getting_started/).

#### Testing Methodology

Testing Methodology is influenced by the following factors:
1. In general, the cost of E2E tests are much higher than lower level tests in TEAMMATES as data is persisted to the actual database. In addition, the time taken for each test is much higher, although this is a less important factor. This means that we have to be careful when choosing E2E testcases.
1. Edge cases are already covered in lower level tests, where the costs of doing exhaustive testing is much lower. This means that we should not be too concered about covering edge cases in E2E tests.

Because of this, it is common when designing E2E tests to focus on testing that the components work well together on the happy path and leave comprehensive testing to lower level tests.

#### Page Object Pattern

The [Page Object Pattern](https://www.selenium.dev/documentation/test_practices/encouraged/page_object_models/) is used in TEAMMATES (and is one of the design patterns recommended by selenium) to reduce code duplication and seperate test and page specific code.

In essence, we encapsulate all page related code in a page object, for example, code that locates elements and interacts with it. Interaction with the page is exposed as methods in the page object. This way, any changes to the UI elements of the page only affects the page object. In addition, test related code do not have to concern themselves with the low level details of how to interact with the UI.

References:
* https://github.com/TEAMMATES/teammates/blob/master/docs/e2e-testing.md
* https://www.selenium.dev/documentation/webdriver/getting_started/
* https://www.selenium.dev/documentation/test_practices/encouraged/page_object_models/

### Tips and Tricks

* When viewing a repository or pull request, press `.` to bring up the built in vs-code based web editor. Especially handy if you need to view other parts of the codebase when reviewing a PR.
* You can [create a branch directly from an issue on GitHub](https://github.blog/2022-03-21-start-working-on-github-issues-faster/). Yes, even if the branch is in a forked repo!
* Press `t` to open file finder in github repos to easily find files!
