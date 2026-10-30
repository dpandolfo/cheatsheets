# Redis Cheatsheet

## Connection

```bash
# Connect to local Redis
redis-cli

# Connect to specific host/port
redis-cli -h localhost -p 6379

# Connect with password
redis-cli -a password

# Connect to specific database
redis-cli -n 1

# Check connection
redis-cli ping

# Get server info
redis-cli info
```

## String Operations

```bash
# Set key-value
SET key value

# Set with expiration (seconds)
SET key value EX 3600

# Set with expiration (milliseconds)
SET key value PX 3600000

# Set only if key doesn't exist
SET key value NX

# Set only if key exists
SET key value XX

# Set with multiple options
SET key value EX 3600 NX

# Get value
GET key

# Get and set (return old value, set new value)
GETSET key newvalue

# Get multiple values
MGET key1 key2 key3

# Set multiple values
MSET key1 value1 key2 value2

# Get range of string
GETRANGE key 0 4

# Set range of string
SETRANGE key 0 value

# Append to string
APPEND key value

# String length
STRLEN key

# Increment number
INCR counter

# Increment by specific value
INCRBY counter 5

# Increment by float
INCRBYFLOAT counter 1.5

# Decrement number
DECR counter

# Decrement by specific value
DECRBY counter 5
```

## List Operations

```bash
# Push element to left
LPUSH mylist value

# Push element to right
RPUSH mylist value

# Push multiple elements
RPUSH mylist value1 value2 value3

# Pop from left
LPOP mylist

# Pop from right
RPOP mylist

# Pop with count
LPOP mylist 2

# Get list length
LLEN mylist

# Get range of elements
LRANGE mylist 0 -1

# Get element at index
LINDEX mylist 0

# Set element at index
LSET mylist 0 value

# Insert before/after
LINSERT mylist BEFORE pivot value
LINSERT mylist AFTER pivot value

# Remove elements
LREM mylist count value

# Trim list to range
LTRIM mylist 0 10

# Blocking pop (wait for element)
BLPOP mylist 10

# Blocking pop from right
BRPOP mylist 10
```

## Set Operations

```bash
# Add element to set
SADD myset member

# Add multiple elements
SADD myset member1 member2 member3

# Remove element
SREM myset member

# Check if member exists
SISMEMBER myset member

# Get set size
SCARD myset

# Get all members
SMEMBERS myset

# Get random member
SRANDMEMBER myset

# Get random members with count
SRANDMEMBER myset 2

# Pop random member
SPOP myset

# Get intersection of sets
SINTER set1 set2

# Get union of sets
SUNION set1 set2

# Get difference of sets
SDIFF set1 set2

# Move element to another set
SMOVE source destination member
```

## Sorted Set Operations

```bash
# Add member with score
ZADD myzset 1 member1

# Add multiple members
ZADD myzset 1 member1 2 member2 3 member3

# Remove member
ZREM myzset member

# Get score of member
ZSCORE myzset member

# Get rank (ascending)
ZRANK myzset member

# Get rank (descending)
ZREVRANK myzset member

# Get sorted set size
ZCARD myzset

# Get members by score range
ZRANGEBYSCORE myzset 0 10

# Get members by score range (descending)
ZREVRANGEBYSCORE myzset 10 0

# Get members by score range with scores
ZRANGEBYSCORE myzset 0 10 WITHSCORES

# Get range by index
ZRANGE myzset 0 -1

# Get range by index (descending)
ZREVRANGE myzset 0 -1

# Count members in score range
ZCOUNT myzset 0 10

# Increment score
ZINCRBY myzset 5 member
```

## Hash Operations

```bash
# Set field value
HSET myhash field value

# Set multiple fields
HSET myhash field1 value1 field2 value2

# Set only if field doesn't exist
HSETNX myhash field value

# Get field value
HGET myhash field

# Get all field values
HGETALL myhash

# Get multiple field values
HMGET myhash field1 field2

# Get all fields
HKEYS myhash

# Get all values
HVALS myhash

# Get hash size
HLEN myhash

# Check if field exists
HEXISTS myhash field

# Delete field
HDEL myhash field

# Delete multiple fields
HDEL myhash field1 field2

# Increment field
HINCRBY myhash field 1

# Increment field by float
HINCRBYFLOAT myhash field 1.5

# Get string length of field value
HSTRLEN myhash field
```

## Key Management

```bash
# Delete key
DEL key

# Delete multiple keys
DEL key1 key2 key3

# Check if key exists
EXISTS key

# Get key type
TYPE key

# Get all keys (warning: slow on large databases)
KEYS *

# Get keys matching pattern
KEYS user:*

# Set expiration (seconds)
EXPIRE key 3600

# Set expiration (milliseconds)
PEXPIRE key 3600000

# Set expiration at timestamp
EXPIREAT key timestamp

# Get TTL (time to live)
TTL key

# Get TTL (milliseconds)
PTTL key

# Remove expiration
PERSIST key

# Rename key
RENAME oldkey newkey

# Rename only if new key doesn't exist
RENAMENX oldkey newkey

# Randomize key
RANDOMKEY

# Scan keys (cursor-based, memory efficient)
SCAN 0 MATCH user:* COUNT 100
```

## Database Management

```bash
# Select database
SELECT 1

# Get database size
DBSIZE

# Flush current database
FLUSHDB

# Flush all databases
FLUSHALL

# Save database to disk
SAVE

# Save asynchronously
BGSAVE

# Last save time
LASTSAVE

# Get server info
INFO

# Get specific info section
INFO memory

# Monitor commands in real-time
MONITOR
```

## Pub/Sub

```bash
# Subscribe to channel
SUBSCRIBE mychannel

# Subscribe to multiple channels
SUBSCRIBE channel1 channel2

# Subscribe to pattern
PSUBSCRIBE news.*

# Publish message
PUBLISH mychannel "Hello"

# Unsubscribe
UNSUBSCRIBE mychannel

# Unsubscribe from all
UNSUBSCRIBE

# Get number of subscribers
PUBSUB CHANNELS

# Get subscribers count for channel
PUBSUB NUMSUB mychannel
```

## Transactions

```bash
# Start transaction
MULTI

# Execute command (queued)
SET key value

# Execute all queued commands
EXEC

# Discard transaction
DISCARD

# Watch key for changes
WATCH key

# Unwatch key
UNWATCH

# Example transaction
MULTI
SET user:1:name "John"
INCR user:1:age
EXEC
```

## Useful Patterns

### Rate Limiting

```bash
# Increment counter with expiration
INCR rate_limit:user:1
EXPIRE rate_limit:user:1 60

# Check if over limit
GET rate_limit:user:1
```

### Session Management

```bash
# Store session
HSET session:abc123 user_id 1 username "john"
EXPIRE session:abc123 3600

# Get session
HGETALL session:abc123
```

### Leaderboard

```bash
# Add score
ZADD leaderboard 100 player1

# Get top 10
ZREVRANGE leaderboard 0 9 WITHSCORES

# Get player rank
ZREVRANK leaderboard player1
```

### Caching

```bash
# Set with expiration
SET cache:key value EX 300

# Get from cache
GET cache:key

# Delete from cache
DEL cache:key
```

## Persistence and Backup

```bash
# Enable RDB persistence
# In redis.conf: save 900 1

# Enable AOF persistence
# In redis.conf: appendonly yes

# Backup RDB file
cp /var/lib/redis/dump.rdb /backup/dump.rdb

# Restore from RDB
cp /backup/dump.rdb /var/lib/redis/dump.rdb

# View memory usage
MEMORY STATS

# Analyze memory usage
MEMORY DOCTOR
```

## Performance Monitoring

```bash
# Get latency stats
LATENCY LATEST

# Monitor latency
LATENCY MONITOR

# Get slowlog
SLOWLOG GET 10

# Slowlog length
SLOWLOG LEN

# Reset slowlog
SLOWLOG RESET

# Get used memory
INFO memory
```
