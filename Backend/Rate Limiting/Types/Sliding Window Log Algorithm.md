
**Sliding Window Log** is a rate-limiting algorithm that **tracks the exact time of each request** and allows a new request **only if the number of requests in the last T seconds is within the limit**.

Key idea:

> **Always look back exactly T seconds from “now”.**

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20240313162126/Sliding-Window-Log-Algorithm.webp)


### How it works (step-by-step)

1. For each client, store **timestamps of all recent requests**.
    
2. When a new request arrives:
    
    - remove timestamps older than `now − T`
        
    - count remaining timestamps
        
3. If count < limit → ✅ allow request and add timestamp
    
4. If count ≥ limit → ❌ reject request
    

---

### Example (this makes it clear)

**Configuration**

- Limit = 5 requests per 10 seconds
    

**Scenario**  
Requests arrive at times:

```
t = 1, 2, 3, 4, 5   → allowed (5 requests)
```

Now at:

```
t = 6 → ❌ rejected (already 5 in last 10 sec)
```

At:

```
t = 12
```

Window is now `(2 → 12)`:

- requests at `t=1` is dropped from log
    
- remaining: `2,3,4,5` → 4 requests
    

So:

```
t = 12 → ✅ allowed
```

👉 No sudden reset. Window **slides continuously**.

---

### What sliding window log guarantees

- ✅ **Perfect accuracy**
    
- ✅ **No boundary burst problem**
    
- ❌ **High memory usage**
    
- ❌ **Expensive for high traffic**
    

---

### When it’s used

- Low traffic systems
    
- Very strict rate limits
    
- Security-sensitive endpoints
    
- Teaching exact behavior


---

### One-sentence interview answer

> Sliding window log rate limiting tracks timestamps of individual requests and enforces limits by counting how many occurred within a continuously moving time window.

---

### Memory hook

- **Store timestamps**
    
- **Drop old ones**
    
- **Always count last T seconds**
