Overall, I believe that because I was the least experienced (or at least I felt I was), I was also able to learn a whole lot from this module, especially front-end-wise.

### Angular

While I used Angular to make [a PR for TEAMMATES](https://github.com/jayasting98/teammates/pull/1) before the semester started, I think I still had a lot more to learn about it, like front-end unit testing (especially this because that initial PR had no tests at that point in time) which I was able to learn when I eventually [actually made that PR in the real TEAMMATES repo](https://github.com/TEAMMATES/teammates/pull/11549). Due to the bindings, I had to pay especially close attention to the component testing scenarios of a component with inputs and outputs and a component inside a test host.

However, that was mostly component and snapshot testing. In order to also learn how to do testing for services, I also did [testing for the feedback responses service](https://github.com/TEAMMATES/teammates/pull/11629). Though, I learned that testing services seemed largely similar to and yet much simpler than testing components.

Beyond testing, I also learned how to create services themselves in [this onboarding task commit](https://github.com/TEAMMATES-2122S2-Team2/teammates/commit/3b008f4628cfa8f5f887b63ac95703e13bf69f75) where I created the service to get feedback response statistics from the backend. I also learned how to [integrate](https://github.com/TEAMMATES-2122S2-Team2/teammates/commit/0d4cc6e18a4f8ebef8e0dad88777def78fca403a) this service with the actual page component in order to actually obtain statistics to display using RxJS.

As for components or their templates, I learned about more about how to use Angular's HTML templates in order to direct inputs to and outputs from a component through property binding and event binding respectively. I also learned about how the custom structural directive `tmIsLoading` worked in [this PR](https://github.com/TEAMMATES/teammates/pull/11628) as I was debugging when I initially wrongly caused the loading spinner to always display when I was in fact trying to display something else (eventually found out it was because I used the same boolean variable used to display the spinner, so don't be like me; check the usages of any variable you reuse). I also learned how to use `<ng-container>` and `<ng-template>` in that same PR, particularly with structural directives like `ngIf`.

Resources:
- [Component Testing Basics](https://angular.io/guide/testing-components-basics)
- [Component Testing Scenarios](https://angular.io/guide/testing-components-scenarios)
- [Service Testing](https://angular.io/guide/testing-services)
- [Services (Dependency Injection)](https://angular.io/guide/dependency-injection)
- [Property Binding](https://angular.io/guide/property-binding)
- [Event Binding](https://angular.io/guide/event-binding)
- [Structural Directives](https://angular.io/guide/structural-directives)
- [<ng-container>]https://angular.io/api/core/ng-container)
- [<ng-template>](https://angular.io/api/core/ng-template)

### RxJS

In order to [integrate](https://github.com/TEAMMATES-2122S2-Team2/teammates/commit/0d4cc6e18a4f8ebef8e0dad88777def78fca403a) Angular services that used asynchronous requests with components, I had to learn about Observables and Subscriptions from RxJS. I also had to learn other things from RxJS like the operators `pipe` or `first` for the previously mentioned component testing I did due to the fact that `EventEmitter` objects used for event binding apparently functioned like RxJS `Observable` objects.

Resources:
- [Observable](https://rxjs.dev/guide/observable)
- [Subscription](https://rxjs.dev/guide/subscription)
- [Operators](https://rxjs.dev/guide/operators)

### HTML/Bootstrap/Web development in general

While I have taken some online web development courses in my free time before, I have actually never touched web development in a real project, only backend and mobile application development. Thus, doing some front-end work benefitted me a lot. For example, I was able to use my initially largely untested (and back then, slowly fading) knowledge of HTML and/or Bootstrap to some use such as in [my onboarding task commits](https://github.com/TEAMMATES-2122S2-Team2/teammates/commit/0d4cc6e18a4f8ebef8e0dad88777def78fca403a) where I (re-)learned how to align everything nicely using the Bootstrap grid system (sorry if this is really basic) or in [TEAMMATES PR #11628](https://github.com/TEAMMATES/teammates/pull/11628). Actually, after doing the front-end stuff in the onboarding task, I decided to go into the back-end for the deadline extensions feature so that I could learn TEAMMATES front to back, but perhaps I should have stayed in the front-end for the deadline extensions feature too to learn more. Still, virtually all my non-deadline extensions feature PRs were front-end related so maybe I was still able to learn as much as I could about the front-end.

Resources:
- [Bootstrap grid system](https://getbootstrap.com/docs/4.0/layout/grid/)

### Jest/Jasmine

I learned how to use these to do front-end unit testing in Angular as previously mentioned, particularly things like `expect` to check values are as expected, `spyOn` to mock services, `beforeEach` for common test setup code, and related attributes/functions (`toBeTruthy()`, etc.).

Also, I learned about snapshot testing. I initially had no idea this existed before (sorry if this is basic), and yet it seems to be pretty widely used (?) so learning of its existence seemed important.

Resources:
- [expect (Using Matchers)](https://jestjs.io/docs/using-matchers)
- [spyOn](https://jasmine.github.io/api/edge/global.html#spyOn)
- [beforeEach (Setup and Teardown)](https://jestjs.io/docs/setup-teardown)
- [Snapshot Testing](https://jestjs.io/docs/snapshot-testing)

### D3.js

I learned how to use D3 to display charts. I used this to create [the feedback responses statistics chart](https://github.com/TEAMMATES-2122S2-Team2/teammates/commit/b09e174dcf9bca6aa0d775d70a8b721a38c4f2b3).

Resources:
- [Build a simple line chart with D3.js in Angular](https://medium.com/weekly-webtips/build-a-simple-line-chart-with-d3-js-in-angular-ccd06e328bff)
- [Official D3 Example - Line Chart](https://observablehq.com/@d3/line-chart)

### Google Cloud Datastore/Objectify

I have previously used Firebase Cloud Firestore, an NoSQL database. I remember how when I used Firestore, I also noticed Datastore, but I just told myself to look at it at another time, and it seems like the time was now. Overall, I found out more about Datastore and how it works, like how it is also a NoSQL database, and I found similarities between entities and documents, and between kinds and collections, which was how I was able to understand it quickly.

For the deadline extensions feature, we had to maintain maps from email addresses to deadlines within the feedback session entities. I learned that this was not a standard Datastore value type so a possible way of storing this would be to store it as a Blob. I also learned that to do this within Objectify, this can be done through the Serialize annotation.

In order to validate requests to update the deadline maps, we needed to check if the emails in the requests actually existed for the corresponding course. One way would be to load every single `CourseStudent` entity and every `Instructor` entity. However, I learned that this costs a certain amount and not only that, the cost scales for every read of every instance. I found out about projection queries, which only scales with the number of queries, not the number of entities read in that query. This was more economical and thus, I chose to do this instead. Strangely, I do not think projection queries are documented in Objectify, so I had to refer to StackOverflow to find out how to do projection queries within Objectify.

I also learned that projection queries needed indices, and I initially wrongly thought that this was only for the properties that were projected, not other properties within the same query that were, say, filtered for instance. I also previously read that every property of each entity kind already has a built-in index of its own, so I initially wrongly assumed that I did not need to write any more indices for my projection queries. However, Fergus (I believe?) pointed out to me that this was wrong and looking at it again, it does make more sense for all properties used in a query, so both projections and filters, to require a composite index altogether. However, this then came with a downside, as I also found out that indices cost money to maintain too due to their storage costs.

Resources:
- [How data is stored in Datastore (Entities, Properties, and Keys)](https://cloud.google.com/datastore/docs/concepts/entities)
- [Datastore supported value types](https://cloud.google.com/datastore/docs/concepts/entities#supported_value_types)
- [Serialization](https://github.com/objectify/objectify/wiki/Entities#serializing)
- [Datastore Pricing](https://cloud.google.com/datastore/pricing)
- [Projection queries](https://cloud.google.com/datastore/docs/concepts/queries#projection_queries)
- [Projection queries in Objectify](https://stackoverflow.com/questions/49691925/objectify-projection-query-with-id-as-well-in-the-result)
- [Projection queries requires indexes](https://cloud.google.com/datastore/docs/concepts/queries#limitations_on_projections)
- [Built-in indexes](https://cloud.google.com/datastore/docs/concepts/indexes#built_in_indexes)
- [Composite indexes](https://cloud.google.com/datastore/docs/concepts/indexes#composite_indexes)

### Google Cloud App Engine

I have also only previously used Google Cloud Functions or Firebase Cloud Functions. I also remember how when I used either of them, I also noticed App Engine and then also told myself to look at it at another time, so getting to learn it by joining TEAMMATES, like Datastore, was such a great thing.

I think the main thing I learned was the task queues, though unfortunately, they are already phased out. I am at least hoping that this knowledge is transferable to what I believe is the new equivalent service of Google Cloud, which is Cloud Tasks. Regardless, I had to use task queues in order to run the workers created by Samuel which handle deadline extension entities for the deadline extensions feature.

Resources:
- [Creating Push Queues](https://cloud.google.com/appengine/docs/standard/java/taskqueue/push/creating-push-queues)
