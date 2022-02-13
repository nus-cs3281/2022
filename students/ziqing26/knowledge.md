## Angular
Angular is a development platform built on TypeScript. There are three types of Angular directices in general:

1. Components: directives with a template.
2. Attribute directives such as `NgClass` and `NgStyle`: directives that change the appearance or behaviour of an element.
3. Structural directives such as `NgIf`, `NgFor` and `NgSwitch`: directives that change the DOM layout by adding and removing DOM elements.

There are two types of forms in Angular:
1. Reactive: Reusuable and synchronous data flow between the view and the data model.
2. Template-driven: TEAMMATES generally uses this type of forms. It focuses on simple scenarios despite being not reusable.

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

## Google Cloud Datastore
TEAMMATES uses Google Cloud Datastore as database.
Through the onboarding task, I learnt about keys-only query which is a subtype of projection queries. Such queries allow querying for specific properties with lower latency and cost.

I learnt that eventually consistent queries generally run faster but may return stale results compared to strongly consistent queries which guarantee the most updated result but take longer to run. I want to learn more about Datastore in the future weeks, such as Indexes.

Resources:

[Documentation Guides on Datastore Queries](https://cloud.google.com/datastore/docs/concepts/queries#projection_queries)

[Data Consistency in Datastore Queries](https://cloud.google.com/appengine/docs/standard/java/datastore/data-consistency)