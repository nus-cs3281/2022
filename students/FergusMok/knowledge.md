# Frontend
### Angular
Angular is a frontend framework built on TypeScript. A majority of Angular's functions uses Typescript decorators, which are adds functionalities to functions and class.

Each Angular component uses a selector (for other components to reference this component), HTML template, and CSS file to decorate the HTML template.

Furthermore, within the HTML template, we are able to use Angular directives. Examples are *ngIf, *ngFor. The asterik is synthetic sugar that surrounds the HTML with a \<ng-template>, and is useful for adding and removing tag elements. Another interesting feature is that Angular supports two-way data binding directly, where the HTML's value can affect the actual value and vice versa. Done with [(NgModel)]

See: 
1. https://angular.io/guidestructural-directives#what-are-structural-directives;
2. https://angular.io/guide/two-way-binding#adding-two-way-data-binding


Angular's CLI is also extremely useful, and most basic features from building and testing are ready out of the box. 

See: https://angular.io/cli

### RxJS
RxJS is a library that helps with async and event-based functions in TEAMMATES through Observables and Subscriptions. RxJS can also be used with other frameworks, like React and Vue. 

Common pattern of usage:
1. Create a service class, with a function that calls the backend API. This function returns an Observable.
2. We can call the service from our component, and add on <b>operators</b> to the Observable, such as pipe and subscribe.
3. Pipe will chain observable operators, and subscribe will activate the observabe to listen for the emitted values.

### Jasmine and Jest
Jasmine is a testing framework. It describes test cases, and can make use of `spies`, that can mock return values, track the status of the function. Furthermore, combined with using inspecting HTML elements, we can check the values of the components in different conditions.
Jest is another testing framework used for snapshots. We're able to take snapshots, save them, and compare them later when running the tests again. This is especially useful for regression testing.

# Backend
### Google Cloud Datastore
I learnt how Datastore's key-value works, it's strengths and limitations, and important conventions. These conventions are seemly counterintuitive for users with an SQL background for smaller applications, but makes sense when building applications at scale.

#### Counters
For example, as datastore's structure does not support aggregating functions, functions such as counting is an O(n) operation. The Datastore community's (counterintuitive) convention is to have multiple Counter class. 

These counters may also face simultaneous write limitations, which is known as contention, when counters change at >5/s. This results in needing to implement 'sharding' counters.

Google's article: https://medium.com/@duhroach/datastore-sharded-counters-2ba6da7475b0

#### Hotspotting
Datastore's (counterintuitive) convention when writing a large amount of data is to avoid monotonically increasing IDs. This is because ranges of storage with similar IDs are stored on the same 'node'(known as tablets), and massive writes to a node will lead to a significant slowdown, called hotspotting. This is a significant pain point for time-series data. 

Former Googler: https://ikaisays.com/2011/01/25/app-engine-datastore-tip-monotonically-increasing-values-are-bad/

The convention is to prepend with a known amount of random numbers/hash, or prepend the ID with other useful fields that can be used for querying later on.

Schema Design: https://cloud.google.com/bigtable/docs/schema-design
#### Indexes
Datastore is built in a way that requires indexes for every single field that requires that needs to be queried. This is because Datastore cannot reference the data of columns, and ONLY the key during the query. The (counterintuitive) convetion is to make indexes for most fields of an entity, and this can lead to 90% of the storage for an entity to be indexes alone. This leads to a trade-off for more performance at scale. 

However, Google does not bill for storage, and only for writes and reads.

Google's tutorial: https://youtu.be/d4CiMWy0J70?t=75


### Additional Tips
1. To pass additional flags to `npm run`, you can use append `-- --<flag>`. E.g `npm run test -- --detect-open-handles`