**Token bucket** is a rate-limiting algorithm that **allows requests to run immediately as long as you have tokens**.

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20240116162804/Blank-diagram-%287%29.png)


Think of it as:

- a bucket that **stores tokens**
    
- tokens are **added at a fixed rate**
    
- **each request spends 1 token**
    
- if no tokens → request is rejected (or delayed)
    

The key idea:

> **You can go fast now if you were slow earlier.**

---

### How it works (step-by-step)

1. Tokens are added to the bucket at a constant rate (up to a max capacity).
    
2. When a request arrives:
    
    - if a token exists → **consume token, allow request immediately**
        
    - if no token → **reject or wait**
        
3. Tokens keep refilling over time.
    

---

### Example (this makes it click)

**Configuration**

- Token refill rate = 1 token / second
    
- Bucket capacity = 5 tokens
    

**Scenario**  
Assume the system was idle, so the bucket is **full (5 tokens)**.

At time `t = 0`, **5 requests arrive at once**.

**What happens**

```
All 5 requests consume 5 tokens
→ All 5 are processed instantly
→ Tokens left = 0
```

Now:

```
t=1 → 1 token refilled
t=2 → 1 token refilled
```

If a request arrives at `t = 1.5`:

- 1 token available
    
- ✅ request allowed immediately
    

If another arrives immediately after:

- no token
    
- ❌ request rejected / delayed
    

---

### What token bucket guarantees

- ✅ **Allows short bursts**
    
- ✅ **Controls long-term average rate**
    
- ❌ **Does not smooth output strictly**
    

---

### Why it’s used for APIs

Real users send requests in bursts (refresh, click, retry).  
Token bucket **handles bursts gracefully** while still enforcing limits.

---

### One-sentence interview answer

> The token bucket algorithm rate-limits traffic by allowing requests to consume tokens that are generated at a fixed rate, enabling short bursts while enforcing a long-term average request rate.

---

### Memory hook

- **Tokens refill slowly**
    
- **Requests spend tokens instantly**
    
- **No tokens = no request**
    

