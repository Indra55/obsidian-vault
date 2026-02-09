
**Sliding Window Counter** is an **optimized version of sliding window log**.  
Instead of storing every request timestamp, it **keeps counts for the current and previous fixed windows** and **estimates** how many requests happened in the last _T_ seconds.

Key idea:

> **Approximate a sliding window using two counters.**

![Image](https://thealgoristsblob.blob.core.windows.net/thealgoristsimages/sliding-window-counter-2.png)


### How it works (step-by-step)

1. Split time into fixed windows (e.g., 1 minute).
    
2. Keep:
    
    - count of **current window**
        
    - count of **previous window**
        
3. When a request arrives:
    
    - calculate how much of the previous window overlaps with “now”
        
    - compute a **weighted sum**:
        
        ```
        effective_count =
          current_count
          + previous_count × overlap_ratio
        ```
        
4. If `effective_count ≤ limit` → ✅ allow  
    else → ❌ reject
    

---

### Example (this is the key)

**Configuration**

- Limit = 10 requests / minute
    
- Window size = 60 seconds
    

**Situation**

- Previous window count = 10
    
- Current window count = 2
    
- We are **12 seconds into** the current window
    

**Overlap**

- 48 seconds of previous window overlap (48/60 = 0.8)
    

**Effective count**

```
= 2 + (10 × 0.8)
= 10
```

👉 Request is **allowed**, but just barely.

If one more request comes immediately:

```
= 3 + (10 × 0.8) = 11 ❌
```

Rejected.

---

### What sliding window counter guarantees

- ✅ Much smoother than fixed window
    
- ✅ No hard reset at boundaries
    
- ✅ Low memory (just two counters)
    
- ❌ Slightly approximate (not exact)
    

---

### Why it exists

- Sliding window log = **accurate but expensive**
    
- Fixed window = **cheap but bursty**
    
- Sliding window counter = **best middle ground**
    

---

### One-sentence interview answer

> Sliding window counter rate limiting approximates a true sliding window by combining counts from the current and previous time windows using a weighted calculation, reducing boundary bursts while keeping memory usage low.

---

### Memory hook

- **Two windows**
    
- **Weighted sum**
    
- **Smooth but approximate**
    

