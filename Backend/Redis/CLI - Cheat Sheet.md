## 🔹 Redis CLI Basics

```bash
redis-cli                 # connect to local redis
redis-cli -h host -p port # connect to remote redis
redis-cli -a password     # auth
redis-cli --raw           # raw output (good for debugging)
```

```redis
PING        # check connection
QUIT        # exit cli
```

---

## 🔹 Server & Info

```redis
INFO                # server info
INFO memory
INFO replication
INFO stats

DBSIZE              # number of keys in current DB
TIME                # server time
CONFIG GET *        # view config
CONFIG SET key val  # set config
```

---

## 🔹 Key Management

```redis
KEYS *              # list all keys (❌ avoid in prod)
SCAN 0              # safe iteration
SCAN 0 MATCH user:* COUNT 10

EXISTS key
DEL key
UNLINK key          # async delete
TYPE key
RENAME key newkey
TTL key
PTTL key
EXPIRE key seconds
PEXPIRE key ms
PERSIST key         # remove expiry
```

---

## 🔹 String Commands (most common)

```redis
SET key value
GET key
MSET k1 v1 k2 v2
MGET k1 k2

SETNX key value     # set if not exists
SETEX key 60 value  # set with expiry

INCR key
INCRBY key 10
DECR key
DECRBY key 5

APPEND key value
STRLEN key
```

💡 **Rate limiter core**

```redis
INCR user:123
EXPIRE user:123 60
```

---

## 🔹 Hashes (like objects)

```redis
HSET user:1 name hitanshu age 21
HGET user:1 name
HMGET user:1 name age
HGETALL user:1

HEXISTS user:1 name
HDEL user:1 age
HLEN user:1
HINCRBY user:1 visits 1
```

---

## 🔹 Lists (queues, logs)

```redis
LPUSH q job1
RPUSH q job2
LPOP q
RPOP q

LRANGE q 0 -1
LLEN q
LTRIM q 0 9
```

🔁 **Blocking queue**

```redis
BLPOP q 0
BRPOP q 5
```

---

## 🔹 Sets (unique items)

```redis
SADD users 1 2 3
SMEMBERS users
SISMEMBER users 2
SREM users 2
SCARD users

SUNION a b
SINTER a b
SDIFF a b
```

---

## 🔹 Sorted Sets (leaderboards, rate limits)

```redis
ZADD leaderboard 100 user1
ZADD leaderboard 200 user2

ZRANGE leaderboard 0 -1
ZREVRANGE leaderboard 0 -1 WITHSCORES

ZRANK leaderboard user1
ZSCORE leaderboard user1
ZREM leaderboard user1
```

💡 **Sliding window rate limiting**

```redis
ZADD hits timestamp requestId
ZREMRANGEBYSCORE hits 0 old_timestamp
ZCARD hits
```

---

## 🔹 Pub / Sub

```redis
SUBSCRIBE channel
PUBLISH channel "hello"
UNSUBSCRIBE
```

---

## 🔹 Transactions

```redis
MULTI
INCR key
EXPIRE key 60
EXEC

DISCARD
WATCH key
UNWATCH
```

---

## 🔹 Lua Scripting

```redis
EVAL "return redis.call('GET', KEYS[1])" 1 mykey
```

---

## 🔹 Persistence

```redis
SAVE            # blocking save
BGSAVE          # background save
LASTSAVE
```

---

## 🔹 Replication

```redis
ROLE
REPLICAOF host port
REPLICAOF NO ONE
```

---

## 🔹 Memory & Debugging

```redis
MEMORY USAGE key
MEMORY STATS
OBJECT ENCODING key
MONITOR           # see all commands (⚠ heavy)
```

---

## 🔹 Flush (Dangerous)

```redis
FLUSHDB
FLUSHALL
```

---

## Redis CLI combos you should remember

```redis
INCR + EXPIRE      # fixed window rate limiter
ZADD + ZCARD       # sliding window limiter
LPUSH + BRPOP      # queue
SETEX              # cache
HSET               # object store
```
