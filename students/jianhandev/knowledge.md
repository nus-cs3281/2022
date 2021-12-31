## Front-End Knowledge
---

### Angular

#### Basics
Components are the building blocks of the web application. 
The component class is where data is temporarily stored and loaded
into the HTML pages. Components handle user interactions e.g. what happens
when a user clicks a button. Components form the bridge to other parts
of the application e.g. backend, via services e.g `HttpRequestService`, `AccountService`.  

**Aspects Learned**
* Understanding the component lifecycle 
* Understanding the underlying web technologies (HTML, CSS)
* Injecting data into webpages 
* Understanding event emitters

**Resources:**
[Intro to Angular Components](https://angular.io/guide/component-overview),
[Component Lifecycle](https://angular.io/guide/lifecycle-hooks)

#### Angular CLI
The Angular CLI allows the developer to quickly create components, modules and tests. Learning to use the CLI helped me to improve my workflow.

**Resources:** 
[Angular CLI tools](https://cli.angular.io/)

#### Routing
**Aspects Learned**
* Defining new routes
* Configuring the routing module

**Resources:**
[Angular Routing Guide](https://angular.io/guide/router#defining-a-basic-route)

#### Front-End Testing
Front-end testing is done using Jest/Jasmine which allows us to perform
snapshot-testing. Snapshot testing is a technique that ensures that
the UI does not change unexpectedly.

**Aspects Learned**
* Snapshot Testing
* Mock functions (`spyOn`, `callFake`)

**Resources:**
[Snapshot testing](https://jestjs.io/docs/en/snapshot-testing),
[Jest Object](https://jestjs.io/docs/en/jest-object),
[Jasmine callFake](https://medium.com/@cinish/jasmine-spying-using-callfake-23625310bacf)

#### RXJS
**Aspects Learned**
* Perform async operations via `Observable`
* Understanding RxJS operators

**Resources:**
[RxJS Observables](https://angular.io/guide/rx-library), 
[Subscription](https://rxjs-dev.firebaseapp.com/guide/subscription),
[RxJS Operators](https://rxjs-dev.firebaseapp.com/guide/operators)

## Back-End Knowledge
---

### Objectify
**Aspects Learned**
* Defining and registering an Entity (corresponding to a single POJO class)
* Basic operations (e.g. `save()`, `delete()`, `save()`)
* Learnt about key-only queries and its benefits
* Understanding the key format and how it has changed from Objectify v5 to v6
* Understand the usage of `key.toUrlSafe()` and `key.toLegacyUrlSafe()`
* Understanding Query cursors
* Configuring `OfyHelper` for development and production environments
* Understanding how `LocalServiceTestHelper` and `LocalServiceTestConfig` are used in testing against local app engine services

**Resources:**
[Objectify Concepts](https://github.com/objectify/objectify/wiki/Concepts),
[Entities](https://github.com/objectify/objectify/wiki/Entities),
[Objectify v6](https://github.com/objectify/objectify/wiki/UpgradeVersion5ToVersion6),
[Query Cursor](https://cloud.google.com/appengine/docs/standard/java/datastore/query-cursors)
[LocalServiceTestHelper](https://cloud.google.com/appengine/docs/standard/java/tools/localunittesting/javadoc/com/google/appengine/tools/development/testing/LocalServiceTestHelper)

### Google Cloud Datastore
**Aspects Learned**
* Deploying to a staging server on GAE
* Setting up, running and connecting to a Datastore Emulator
* Exporting data from a staging server 
* Importing data into Datastore Emulator for backward compatibility testing
* Testing different versions against staging server

**Resources:**
[Running the Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator),
[Export and Import Data](https://cloud.google.com/datastore/docs/tools/emulator-export-import)

### E2E Testing
**Aspects Learned**  
* PageObject Design Pattern

**Resources:**
[PageObject Design Pattern](https://martinfowler.com/bliki/PageObject.html)

### Solr 
Solr is an open source enterprise search platform built on Apache Lucene. One major feature of Solr that TEAMMATES tries to leverage on is full-text search.  

**What is full text search?**  
Full text search
* is a more advanced way to search a database
* finds all instances of a term (word) in a table without having to scan rows and without having to know which column a term is stored in
* works by using text indexes, which stores positional information for all terms found in the columns you create the text index on
* is term-based, not pattern-based

**Resources:**
[Full Text Search](http://infocenter.sybase.com/help/index.jsp?topic=/com.sybase.help.sqlanywhere.12.0.1/dbusage/full-text-search-what-is-it.html)

**Aspects Learned**
* Running and configuring Solr as a local search service
* Providing Solr as a service to the backend using SolrJ Java Client
* Indexing search documents in Solr
* Understanding Collections, Sharding, Cores, Replicas
* Running search service in docker
* Query paramters and Filter Queries in Solr
* Adding Copy Field rules to schema

**Resources:**
[Solr Local Setup](https://solr.apache.org/guide/8_8/solr-tutorial.html#exercise-1), [Basics of Apache Solr](https://www.infoworld.com/article/3209685/why-you-should-use-apache-solr.html#:~:text=Apache%20Solr%20is%20both%20a,document%20database%20with%20SQL%20support.&text=Solr%20is%20a%20search%20engine,NoSQL%20database%20with%20transactional%20support), [Terminologies](https://solr.apache.org/guide/8_8/solr-glossary.html), [Docker/Solr](https://lucidworks.com/post/solr-on-docker-2/), [Filter query](https://solr.apache.org/guide/7_3/common-query-parameters.html), [Copy Field](https://solr.apache.org/guide/8_8/schema-api.html#add-a-new-copy-field-rule)

### Github Actions
**Aspects Learned**
* Github actions and changing the workflow for CI
* Learning YAML (e.g block scalars, variables, syntax etc)

**Resources:**
[Github Actions](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions), [YAML](https://www.yaml.info/learn/index.html)


### Regex
**Aspects Learned**
* Filtering query strings with regex

**Resources:**  
[Regex hands-on/cheatsheet](https://regexr.com/)
