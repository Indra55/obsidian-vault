

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20240725172914/Leaky-Bucket-Algorithm.png)



## 1. Core idea (intuition first)

The **leaky bucket** algorithm models rate limiting as:

- a **bucket** with a **small hole** at the bottom
    
- **requests = water**
    
- water can enter the bucket at **any speed**
    
- water leaks out at a **constant, fixed rate**
    

👉 **Output rate is always smooth and constant**, no matter how bursty the input is.

If the bucket fills up:

- extra water is **discarded** (or request is rejected)
    

---

## 2. What problem leaky bucket solves

Backend systems often face:

- sudden request bursts
    
- uneven traffic
    
- downstream services that can’t handle spikes (DB, external APIs)
    

Leaky bucket ensures:

- backend processes requests at a **stable, predictable rate**
    
- bursts do **not** propagate downstream
    

> It is designed for **traffic smoothing**, not flexibility.

---

## 3. How leaky bucket works (step-by-step)

1. Incoming requests arrive at random rates
    
2. Each request is added to a **queue (bucket)**
    
3. Requests are processed at a **fixed rate** (leak rate)
    
4. If the queue exceeds capacity:
    
    - new requests are dropped or rejected
        

---

## 4. Key components of leaky bucket

|Component|Meaning|
|---|---|
|Bucket capacity|Maximum number of queued requests|
|Leak rate|Requests processed per unit time|
|Queue|Buffer holding pending requests|
|Overflow policy|Drop or reject when full|

---

## 5. Timeline example (very interview-friendly)

**Configuration**

- Bucket size = 5
    
- Leak rate = 1 request/second
    

**Traffic**

- At `t = 0`, 5 requests arrive instantly
    
- Bucket fills completely
    
- Requests are processed:
    
    - t=1 → 1 processed
        
    - t=2 → 1 processed
        
    - t=3 → 1 processed …
        

**Now**

- If a 6th request arrives at t=0 → ❌ rejected
    
- If it arrives at t=3 → ✅ accepted (space freed)
    

---

## 6. What leaky bucket guarantees

✅ **Strict output rate**  
✅ **Smooth traffic**  
✅ **No bursts passed downstream**

What it does **not** allow:  
❌ sudden spikes  
❌ burst tolerance

---

## 7. Leaky bucket vs Token bucket (conceptual difference)

|Aspect|Leaky Bucket|Token Bucket|
|---|---|---|
|Burst handling|❌ No|✅ Yes|
|Output rate|Constant|Variable|
|Traffic smoothing|Excellent|Moderate|
|Strictness|Very strict|Flexible|
|Typical use|Network shaping|API rate limiting|

👉 **Interview line**

> Leaky bucket enforces a fixed processing rate, while token bucket allows controlled bursts.

---

## 8. Is leaky bucket rate limiting or throttling?

This is a **tricky interview point**.

- Leaky bucket is often classified as **rate limiting**
    
- Conceptually, it behaves more like **throttling**
    

Why?

- It **paces** requests
    
- It does not immediately reject unless capacity is exceeded
    

Best answer:

> Leaky bucket is a rate-limiting algorithm with throttling-like behavior.

---

## 9. Where leaky bucket is used

- Network traffic shaping
    
- Routers and switches
    
- Streaming systems
    
- Real-time pipelines
    
- Situations requiring **constant throughput**
    

Less common for:

- user-facing APIs (too strict)
    
- bursty client behavior
    

---

## 10. Advantages

- Predictable system behavior
    
- Protects downstream services
    
- Simple mental model
    
- Prevents burst amplification
    

---

## 11. Disadvantages

- Poor user experience under bursts
    
- Higher latency due to queuing
    
- Can drop valid requests
    
- Not flexible for real-world API traffic
    

---

## 12. One-paragraph interview definition (memorize)

> The leaky bucket algorithm is a rate-limiting mechanism that processes requests at a constant, fixed rate by placing incoming requests into a bounded queue and draining them steadily over time. If the queue becomes full, excess requests are rejected. It is primarily used for traffic smoothing and strict throughput control rather than burst tolerance.

---

## 13. One-line memory hook

> **Leaky bucket smooths traffic by enforcing a constant output rate, dropping requests when capacity is exceeded.**

