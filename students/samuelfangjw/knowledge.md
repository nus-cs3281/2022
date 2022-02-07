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

### Tips and Tricks
* When viewing a repository or pull request, press `.` to bring up the built in vs-code based web editor. Especially handy if you need to view other parts of the codebase when reviewing a PR.