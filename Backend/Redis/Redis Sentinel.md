Redis Sentinel is a high-availability system for Redis that monitors Redis instances, detects failures, and automatically performs failover when a master goes down. Redis Sentinel runs on port 26379.
![[redse.png]]

### **Sentinel capabilities at a macroscopic level (i.e. the big picture):**

- **Monitoring**. Sentinel constantly checks if your master and replica instances are working as expected.
- **Notification**. Sentinel can notify the system administrator, or other computer programs, via an API, that something is wrong with one of the monitored Redis instances.
- **Automatic failover**. If a master is not working as expected, Sentinel can start a failover process where a replica is promoted to master, the other additional replicas are reconfigured to use the new master, and the applications using the Redis server are informed about the new address to use when connecting.
- **Configuration provider**. Sentinel acts as a source of authority for clients service discovery: clients connect to Sentinels in order to ask for the address of the current Redis master responsible for a given service. If a failover occurs, Sentinels will report the new address.

### **Fundamental things to know about Sentinel before deploying**

1. You need at least three Sentinel instances for a robust deployment.
2. The three Sentinel instances should be placed into computers or virtual machines that are believed to fail in an independent way. So for example different physical servers or Virtual Machines executed on different availability zones.
3. Sentinel + Redis distributed system does not guarantee that acknowledged writes are retained during failures, since Redis uses asynchronous replication. However there are ways to deploy Sentinel that make the window to lose writes limited to certain moments, while there are other less secure ways to deploy it.
4. You need Sentinel support in your clients. Popular client libraries have Sentinel support, but not all.
5. There is no HA setup which is safe if you don't test from time to time in development environments, or even better if you can, in production environments, if they work. You may have a misconfiguration that will become apparent only when it's too late (at 3am when your master stops working).
6. **Sentinel, Docker, or other forms of Network Address Translation or Port Mapping should be mixed with care**: Docker performs port remapping, breaking Sentinel auto discovery of other Sentinel processes and the list of replicas for a master. Check the [section about _Sentinel and Docker_](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/#sentinel-docker-nat-and-possible-issues) later in this document for more information.

### 1️⃣ Master stops responding

Each Sentinel is pinging the master.

If a Sentinel doesn’t get replies for `down-after-milliseconds`:

- It marks the master as **SDOWN** (subjectively down)

> This is **local opinion**, not enough to act.

---

### 2️⃣ Quorum agreement → ODOWN

Sentinels gossip with each other.

If **enough Sentinels (quorum)** agree the master is down:

- Master is marked **ODOWN** (objectively down)

👉 **Only now is failover allowed**

Example:

```
3 Sentinels
quorum = 2
→ 2 agree = ODOWN
```

---

### 3️⃣ Leader Sentinel election (critical part)

Sentinels now **elect one Sentinel as the leader** for this failover.

- Uses a **Raft-like leader election**
- Each Sentinel can vote **once per epoch**
- The Sentinel with majority votes becomes **failover leader**

🔥 **This is important**:

- ❌ All Sentinels do NOT promote a replica
- ✅ **Exactly one Sentinel** performs the failover

---

### 4️⃣ Replica selection (new master)

The leader Sentinel chooses the best replica based on:

- Highest replication offset (most up-to-date)
- Lowest priority (`replica-priority`)
- Health & connectivity
- Not disconnected too long

That replica is:

```
SLAVEOF NO ONE

```

→ promoted to **new master**

---

### 5️⃣ Reconfiguration

Leader Sentinel:

- Reconfigures other replicas to follow the new master
- Updates Sentinel configs
- Publishes events

---

### 6️⃣ Clients learn the new master

Clients:

- Ask Sentinel: `SENTINEL get-master-addr-by-name`
- Or get notified via pub/sub
- Reconnect to new master automatically

---

## Key clarification (very important)

❌ **Quorum does NOT directly elect the new master**

✅ **Quorum only decides:**

> “Is the master really dead?”

Then:

- **Leader Sentinel** does the actual promotion

---

## Timeline summary

```
Master down
   ↓
SDOWN (local)
   ↓
ODOWN (quorum agrees)
   ↓
Leader Sentinel elected
   ↓
Best replica promoted
   ↓
Cluster healed

```

---

## What if quorum is NOT reached?

No quorum = **NO FAILOVER**

This prevents:

- Split brain
- Two masters
- Data corruption

Example:

```
3 Sentinels, quorum = 2
Only 1 alive → no failover
```

---

## One-liner (perfect for exams / interviews)

> When a Redis master fails, Sentinels reach quorum to mark it objectively down, elect a leader Sentinel, and that leader promotes the most up-to-date replica to master and reconfigures the system.

**Redis Sentinel Pros:**

- With three nodes, you can build up a fully functional Sentinel deployment. _(Image 2)_
- Simplicity - it’s usually simple to maintain and configure.
- Highly available, you can build a Redis Sentinel deployment that can survive certain failures without any need for human intervention.
- Work as long as a single master instance is available; it can survive the failure of all slave instances.
- Multiple slave nodes can replicate data from a master node.

**Redis Sentinel Cons:**

- Not scalable; writes must go to the master, cannot solve the problem of read-write separation.
- Slaves may serve reads, but because of asynchronous replication, outdated reads may result.
- It doesn’t sharded data, so master and slave utilization will be imbalanced.
- The slave node is a waste of resources because it does not serve as a backup node.
- Redis-Sentinel must be supported by the client. The client holds half of the magic.

Resources -

[https://medium.com/hepsiburadatech/redis-solutions-standalone-vs-sentinel-vs-cluster-f46e703307a9](https://medium.com/hepsiburadatech/redis-solutions-standalone-vs-sentinel-vs-cluster-f46e703307a9)

[Redis Sentinel, learning from scratch to deploy to the production environment | by Kienlt | Medium](https://medium.com/@kienlt.qn/redis-sentinel-learning-from-scratch-to-deploy-to-the-production-environment-18d03ea61b27)

[https://dev.to/hedgehog/set-up-a-redis-sentinel-3m50](https://dev.to/hedgehog/set-up-a-redis-sentinel-3m50)