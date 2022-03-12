### GCP: Billing Mechanism

I did a research on how Google Cloud's services are being billed, especially on the mechanism for
Datastore and App Engine. This gives me better way to appreciate the importance of achieving balance
between feature/performance over reallife contraints such as budget contraint.

Documents Referred:
* https://cloud.google.com/appengine/pricing
* https://cloud.google.com/datastore/pricing

### GCP: Datastore Indexing

I learned how Datastore works and the importance of indexing. As by default only ascending order
is indexed by Datastore, more compilcated queries involving sorting and ranging needs a custom index
to be built before queries can be processed.

Documents Referred:
* https://cloud.google.com/datastore/docs/concepts/indexes
* https://cloud.google.com/appengine/docs/standard/java/datastore/indexes

### GCP: Datastore, Objectify and Google Cloud GUI

Knowledge on how to manipulate datastore entities are gained along the way of working on onboarding
tasks. This includes the use of Objectify API and the DatastoreClient framework.

Documents Referred:
* https://www.javadoc.io/doc/com.googlecode.objectify/objectify/latest/com/googlecode/objectify/Objectify.html
* https://github.com/GabiAxel/google-cloud-gui
* Codes in the teammates repo itself

### Angular: Life-Cycle Hooks, View/ContentChild and ElementRef

Researches on these core APIs were done when I attempt to add event listeners for mouse scroll events.

Documents Referred:
* https://angular.io/guide/lifecycle-hooks
* https://angular.io/api/core/Directive
* https://angular.io/api/core/ViewChild
* https://angular.io/api/core/ContentChild
* https://angular.io/api/core/ElementRef

### Angular: Attribute Directive and HostListener

This seems to be a more convenient way to change the behaviour of a component. It is also very convenient for
code reuse. Directive can be declared once and easily imported to any other modules. It is more flexible than
the ElementRef solution to add event listener.

Document Referred:
* https://angular.io/guide/attribute-directives
* https://angular.io/api/core/HostListener

### Angular: EventEmitter and Handlers

This is a way to synchronize and pass output data from a sub-component to its parent component. It is very
convenient in use.

Document Referred:
* https://angular.io/api/core/EventEmitter
* https://angular.io/api/core/Output

### Jest: Fixing Failed Test

During development, I encountered problem of failing tests. One issue is the missing dependencies. By default,
when generate Angular's component using `ng generate`, the jest test file does not import the module itself but
declares the component in the test file.

This leads to missing dependencies for rendering the component during the test. However, if I import the module
in the test file, it will show an error saying the component is declared in multiple modules. The way to resolve
this is to remove the declaration of component from the test.

The other issue encountered is the error of attempt to convert circular structure to json. This is mainly due to
missing dependencies during the initialization of tests. The link below provides a easy way to check which module
is missing for imports.

Document Referred:
* https://stackoverflow.com/questions/45764202/angular-test-fails-because-component-is-part-of-the-declarations-of-2-modules
* https://stackoverflow.com/questions/63895685/unhandledpromiserejectionwarning-typeerror-converting-circular-structure-to-js

### RxJS: Observable, Pipe, Finalize()

Observable is runned asynchronously and if there is the other async function running within an observable (such
as the other observable's subscription or setInterval()), the running sequence is not guaranteed. Therefore, the
outside finalize() can actually run before codes inside finish execution.

Document Referred:
* https://rxjs.dev/api/operators/finalize
* https://rxjs.dev/guide/operators
* https://rxjs.dev/guide/observable