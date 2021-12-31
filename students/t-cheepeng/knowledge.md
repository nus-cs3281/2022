### Testing in Angular
Aspects: component testing, snapshot testing, unit testing and mocking.

Resource: [Angular Documentation](https://angular.io/guide/testing)

---

### GitHub Auth Operations
Aspects: Github is deprecating all password authentication in favour of token-based authentication by Aug 2021. Affects CLI for git. Customization of access controls with PATs

Resource: [GitHub Blogs](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)

---

### Apexcharts
Aspects: FOSS charts library for web pages.

Resource: [Apexcharts Website](https://apexcharts.com/)

---

### Google Analytics
Aspects: Tracking of page views, events, costs associated with usage of analytics and opt-out privacy policies

Resource: [Google Analytics Documentation](https://developers.google.com/analytics/devguides/collection/gtagjs)

---

### Google Cloud Logging
Aspects: Logs API(GAE platform), Cloud Logging(GCP platform), differences between the two and how cloud logging on the upgraded GCP platform is more powerful than GAE, and logs-related quotas

Resource: [Log API](https://cloud.google.com/appengine/docs/standard/java/javadoc/com/google/appengine/api/log/package-summary), [Cloud Logging](https://cloud.google.com/logging/docs)

---

### Cloud Logging Labels
Aspects: Cloud logging labels that allow user-defined labels, their limitations and defaults

Resource: [Using log-based metrics](https://cloud.google.com/logging/docs/logs-based-metrics/labels)

---

### Angular testing for window object
Aspects: window object is frequently used in the codebase of TEAMMATES, e.g., window.location.href. The usage of Angular as a framework with jest testing means that window object should usually not be accessed directly. Testing of the window object in unit tests then require some APIs and dependency injection techniques

Resource: [InjectionToken](https://angular.io/api/core/InjectionToken), [Injecting Window to Angular](https://jasminexie.github.io/injecting-window-in-an-angular-application/)

---

### Proxy Pattern
Aspects: Structural design pattern that allows a placeholder for another object type. Mainly used for controlling access to the original object, allowing actions before requesting original object. Used in TEAMMATES backend as a layer over Google Cloud Task API and CLoud Logging API. Allows for mocking of 3rd party API services that we do not have direct control over for unit testing.

Resource: [PR for Proxy Pattern](https://github.com/TEAMMATES/teammates/pull/11021)

---

### Data Warehouse
Aspects: Uses of a data warehouse, purpose of a warehouse and architecture of it. Differences between a data warehouse and a database.

Resource: [AWS Data Warehouse Concepts](https://aws.amazon.com/data-warehouse/)

---

### New HTML Tags
Aspects: The `<datalist>` tag and how it is used to support autocomplete as well as combining text inputs with select dropdowns

Resource: [HTML datalist Tag](https://www.w3schools.com/tags/tag_datalist.asp)