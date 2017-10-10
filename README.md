# redisHA
Solution of redis failover, read/write separation, and read load-balance. Total client-independent

# What can it do
- Let your redis master/slave failover automatically
- Switch clients to the new master when failover happens
- read/write separation
- Read traffic load-balance (between slaves or all servers)
- No multi-master problem (compare to legacy HAProxy redis check)
- Total client-independent, doesn't require any client libraries

# Prerequisite
- Redis
- HAProxy (community edition is enough)

# How to use

## Depoly a 3-server redis enviorment (1 master, 2 slaves)
For this part, you can find further instructions at https://redis.io/topics/replication

## Deploy 3 sentinels
1. Make 3 copies of sentinel.conf. Modify them, replace properties surrounded by angle brackets with real ones.
2. Start 3 sentinels
```
redis-sentinel ./sentinel.conf
```

## Run HAProxy
1. Modify haproxy.cfg, replace properties surrounded by angle brackets with real ones, and make changes accord to comments if needed.
2. Start HAProxy
```
haproxy -f ./haproxy.cfg
```

## Then it's done
just let your clients use HAProxy's 16379 port for writes and 16479 port for reads

# How to test failover
Connect to current master, execute command:
```
DEBUG sleep 10
```
the failover should happen after 8 secs, then you can visit http://<ip-of-haproxy>:8888/haproxy?stats to see if HAProxy's stat changes

# More to do
Make your HAProxy able to failover (keepalived recommended)
