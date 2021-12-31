### Jest

Mocks or spies in Jest allow one to capture calls to a function,
and replace its function implementation or return values.
It is an important technique in isolating test subjects and replacing its dependencies.

In our front-end tests, we use mocks in unit tests to ensure that
our service methods are called correctly with the proper parameters.
I learned to create mocks by using `spyOn`, which are then chained with
methods such as `returnValue` or `callFake` to delegate calls from the spy to the mock function or return value.

Jest allows for snapshot testing with `toMatchSnapshot`, which can be automatically updated when components change.
Jest also allows one to test asynchronous code involving callbacks or promises;
for instance, some of our tests were modified to support callbacks by taking and using an additional `done` argument.

**Resources:**
- [Jest documentation](https://jestjs.io/docs/en/getting-started.html): provides an understanding of the various forms of testing, including mocks, asynchronous code handling, snapshot testing, and more.

### Angular

Angular is a framework written in TypeScript and used for the frontend of TEAMMATES.
Angular makes use of modules and components, whereby each component instance comes with its own lifecycle.
Hooks can be used for each event in the lifecycle to execute the appropriate logic when events such as
initialization, destruction, or changes occur.

Components make use of services that help to provide functionality;
within the context of TEAMMATES, these can be API services that make the relevant calls to the backend.

**Resources:**
- [Angular tutorial: Tour of Heroes](https://angular.io/tutorial): simple introduction that covers the key topics and concepts of Angular in a single application.

### Objectify

The Objectify API as used in TEAMMATES allows us to define new entities
corresponding to the key/value layout within the Google App Engine datastore,
and query for them or modify and delete them.

As for the queries, it is helpful to note that keys-only queries can be effectively free,
as they only return the keys of the result entities without needing to return the entire entities themselves.
This can be helpful as reading and writing costs are both non-negligible in large-scale applications.

**Resources:**
- [Objectify Wiki](https://github.com/objectify/objectify/wiki)

### Google App Engine

GAE is the cloud computing platform used in TEAMMATES.
It bundles together many services, some of which have since been released as standalone services.

To manage and test logging (unavailable on the development server),
I deployed TEAMMATES to a private staging server,
which involved learning the use of `gcloud` services to setup and monitor application activity.
Cron jobs were also learned and modified to suit our purposes through editing `cron.xml`.

**Resources:**
- [TEAMMATES Wiki: Deploying to Staging](https://github.com/TEAMMATES/teammates-ops/blob/master/platform-guide.md#deploying-to-a-staging-server)
- [GAE Standard Documentation in Java](https://cloud.google.com/appengine/docs/standard/java)

### Google Cloud Logging

Cloud Logging is one of the services offered under GCP,
and a key service to be used in the audit logs feature of TEAMMATES.
As costs are key in a large-scale application,
learning the pricing information regarding our logging services is important
so as to pre-empt operating costs and design our features in such a way as to reduce them.

On staging, I performed some tests regarding our new audit logs,
and then metrics were monitored through the cloud console;
such metrics include [metrics on log usage](https://console.cloud.google.com/logs/metrics) among others.

**Resources:**
- [Google Cloud Logging: code samples](https://cloud.google.com/logging/docs/samples): references for using Cloud Logging in Java
- [Google Cloud Operations Suite: pricing](https://cloud.google.com/stackdriver/pricing): contains information on pricing for logging and monitoring

### CSRF tokens

Cross-site request forgery (CSRF) is a form of malicious attack,
whereby a user with access privileges is unknowingly tricked into submitting an illicit web request.

Traditionally, these can be protected against with methods such as CSRF tokens.
A unique valid CSRF token value is set for some request, typically in a hidden form field,
such that only requests from the particular web application will be considered valid.

However, X-CSRF tokens serve as another alternative.
The server can send a cookie setting a random token, which is received and read by JavaScript on the client-side.
The token is then sent with each subsequent request through a custom `X-CSRF-TOKEN` HTTP header,
which is validated by the server.

This technique is used by frameworks such as Angular, and used in TEAMMATES as well.
By default, we use these protections with non-GET requests such as POST and PUT,
for instance in the POST requests for creating new audit logs.

**Resources:**
- [Wikipedia page on CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery)
- [StackOverflow answer on difference between CSRF and X-CSRF token](https://stackoverflow.com/questions/34782493/difference-between-csrf-and-x-csrf-token)

### Timezone configuration

As our front-end and back-end use different libraries and databases when obtaining timezone information,
and these timezones can change over time, we need to keep those two databases in sync.
Both rely on IANA's tz database as the authoritative source for timezone information,
but may be out-of-date and require manual updating when IANA releases a new timezone version.
The front-end and back-end databases can be updated through means such as the `grunt `tool with `moment-timezone`,
or the `tzupdater` tool.

**Resources:**
- [IANA Time Zone Database](https://www.iana.org/time-zones)
- [TEAMMATES ops maintainer guide](https://github.com/TEAMMATES/teammates-ops/blob/master/maintainer-guide.md): further information on conducting timezone database updates

