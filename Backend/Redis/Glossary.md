Redis – An in-memory, single-threaded (command execution) data store designed for extremely low-latency access, commonly used for caching, real-time analytics, session storage, and messaging.

Key – A binary-safe identifier that uniquely references a value in Redis and is used for hashing, sharding, expiration, and eviction decisions.

Value – The data associated with a key, stored in optimized internal encodings depending on size and access patterns to reduce memory usage.

TTL – Time To Live is metadata attached to a key that causes Redis to automatically delete it after a specified duration, enabling automatic cleanup of stale or ephemeral data.

Expiration – The mechanism by which Redis tracks TTLs using lazy deletion during access and active expiration cycles in the background.

Eviction Policy – A memory management strategy that determines which keys are removed when Redis reaches its configured maxmemory limit.

LRU – Least Recently Used eviction approximates recency by tracking access patterns and removes keys that have not been accessed recently.

LFU – Least Frequently Used eviction tracks access frequency counters and removes keys that are least frequently accessed over time.

Volatile Keys – Keys that have an associated TTL and are eligible for eviction under volatile eviction policies.

Non-Volatile Keys – Keys without TTL that are only evicted when using allkeys eviction policies.

Persistence – Redis mechanisms that allow data to survive restarts by writing in-memory data to disk.

RDB – Redis Database persistence creates point-in-time snapshots using forked child processes, offering fast restarts but potential data loss between snapshots.

AOF – Append Only File persistence logs every write command, providing better durability at the cost of higher disk I/O and recovery time.

AOF Rewrite – A background process that compacts the AOF file by rewriting it into a minimal set of commands to reduce file size.

Replication – Asynchronous data copying from a primary node to replicas to improve read scalability and provide redundancy.

Replication Lag – The delay between a write on the primary and its visibility on replicas due to network or processing delays.

Primary Node – The authoritative Redis node that handles all write operations in a replication setup.

Replica Node – A Redis node that continuously applies updates from the primary and can be promoted during failure.

Redis Sentinel – A distributed system of monitoring processes that detect primary failures, perform leader election, and coordinate automatic failover.

Quorum – The minimum number of Sentinels that must agree before declaring a primary node as down.

Failover – The coordinated process where a replica is promoted to primary and clients are redirected without manual intervention.

Redis Cluster – A distributed Redis deployment that enables horizontal scaling by automatically partitioning data across multiple nodes.

Sharding – The process of splitting the keyspace across nodes using a deterministic hashing mechanism.

Hash Slot – One of 16,384 logical partitions used by Redis Cluster to map keys to specific nodes.

Key Hash Tag – A substring inside a key that forces multiple keys to map to the same hash slot for atomic operations.

Resharding – The process of moving hash slots between nodes to rebalance load without downtime.

Ephemeral Data – Short-lived data stored in Redis that is expected to expire or be regenerated, such as sessions, locks, and caches.

Pub/Sub – A messaging pattern in Redis where messages are published to channels and delivered to all active subscribers without persistence.

Channel – A named message stream used in Redis Pub/Sub that does not store messages once delivered.

Stream – A persistent, append-only log data structure designed for event sourcing and reliable message processing.

Consumer Group – A coordination mechanism that allows multiple consumers to process stream entries with message acknowledgment and recovery.

Pending Entries List – A data structure that tracks messages delivered but not yet acknowledged in a consumer group.

Pipeline – A client-side optimization that batches multiple commands into a single network round trip to reduce latency.

Transaction – A group of commands wrapped in MULTI and EXEC that execute sequentially without interleaving but without rollback guarantees.

Atomic Operation – An operation that Redis guarantees to execute entirely without interruption from other commands.

Lua Scripting – Server-side scripting that allows multiple Redis commands to be executed atomically in a single operation.

Distributed Lock – A locking mechanism implemented using Redis primitives to coordinate access across multiple processes or services.

Redlock – A distributed locking algorithm designed to provide fault tolerance across multiple Redis instances.

Cache Aside – A caching pattern where the application explicitly loads data into Redis on cache miss and updates the cache on writes.

Write Through Cache – A pattern where writes go to both Redis and the database synchronously to keep them consistent.

Write Back Cache – A pattern where writes go to Redis first and are asynchronously flushed to the database later.

Hot Key – A key that receives disproportionately high traffic and can become a bottleneck in Redis or a cluster node.

Keyspace Notifications – Redis feature that emits events when keys are modified, expired, or evicted.

Memory Fragmentation – Inefficient memory usage caused by allocator behavior and varying key lifetimes.

Single-Threaded Execution – Redis processes commands sequentially on a single thread, avoiding locks while relying on fast I/O and event loops.

Event Loop – The mechanism Redis uses to multiplex network I/O and schedule command execution efficiently.
