### Google Cloud Billing Mechanism

I did a research on how Google Cloud's services are being billed, especially on the mechanism for
Datastore and App Engine. This gives me better way to appreciate the importance of achieving balance
between feature/performance over reallife contraints such as budget contraint.

Documents Referred:
* https://cloud.google.com/appengine/pricing
* https://cloud.google.com/datastore/pricing

### Google Cloud Datastore Indexing

I learned how Datastore works and the importance of indexing. As by default only ascending order
is indexed by Datastore, more compilcated queries involving sorting and ranging needs a custom index
to be built before queries can be processed.

Documents Referred:
* https://cloud.google.com/datastore/docs/concepts/indexes
* https://cloud.google.com/appengine/docs/standard/java/datastore/indexes

### Datastore, Objectify and Google Cloud GUI

Knowledge on how to manipulate datastore entities are gained along the way of working on onboarding
tasks. This includes the use of Objectify API and the DatastoreClient framework.

Documents Referred:
* https://www.javadoc.io/doc/com.googlecode.objectify/objectify/latest/com/googlecode/objectify/Objectify.html
* https://github.com/GabiAxel/google-cloud-gui
* Codes in the teammates repo itself

### Angular's Life-Cycle Hooks, Directives, View/ContentChild and ElementRef

Researches on these core APIs were done when I attempt to add event listeners for mouse scroll events.

Documents Referred:
* https://angular.io/guide/lifecycle-hooks
* https://angular.io/api/core/Directive
* https://angular.io/api/core/ViewChild
* https://angular.io/api/core/ContentChild
* https://angular.io/api/core/ElementRef