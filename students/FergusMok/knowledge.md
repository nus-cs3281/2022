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