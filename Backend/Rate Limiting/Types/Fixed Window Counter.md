
**Fixed Window Counter** is a rate-limiting algorithm that **counts how many requests arrive in a fixed time window** (like 1 second or 1 minute).

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20240725172949/Fixed-Window-Algorithm.png)

### What it is (plain words)


- Time is split into **non-overlapping windows**
    
- Each window has a **counter**
    
- If the counter exceeds the limit → **reject requests**
    
- When the window ends → **counter resets to 0**
    

Key idea:

> **Limits requests per window, then forgets everything at the boundary.**

---

### How it works (step-by-step)

1. Choose a window size (e.g., 1 minute).
    
2. Keep a counter for that window.
    
3. For each request:
    
    - increment the counter
        
    - if counter > limit → reject
        
4. When the window time ends:
    
    - reset counter
        
    - start a new window
        

---

### Example (important)

**Configuration**

- Limit = 5 requests per minute
    
- Window = 12:00:00 → 12:00:59
    

**Scenario**

- At 12:00:10 → 5 requests arrive → ✅ all allowed
    
- At 12:00:50 → another request → ❌ rejected (limit reached)
    

Now the window ends.

- At 12:01:00 → counter resets to 0
    
- At 12:01:01 → 5 new requests arrive → ✅ all allowed
    

👉 **Result:**  
10 requests were accepted within ~11 seconds.

---

### The big problem (interview favorite)

**Boundary burst problem**

Because counters reset abruptly, a client can:

- send many requests at the **end of one window**
    
- then many more at the **start of the next**
    

This causes **unnatural traffic spikes**.

---

### What fixed window guarantees

- ✅ Very simple
    
- ✅ Low memory usage
    
- ❌ Allows bursts at window boundaries
    
- ❌ Inaccurate for strict rate control
    

---

### When it’s used

- Low-risk APIs
    
- Simple systems
    
- Teaching / basic implementations
    
- When approximate limits are acceptable
    

---

### One-sentence interview answer

> Fixed window counter rate limiting divides time into fixed intervals, counts requests per interval, and rejects requests once the limit is exceeded, resetting the counter at the start of each new window.

---

### Memory hook

- **Count → reject → reset**
    
- **Fast but naive**
    
