Rate limiting is a Backend mechanism that restricts how frequently a client can access a resource within a time window to protect system stability, fairness, and cost. It is usually enforced early in the request lifecycle using algorithms like token bucket or sliding window. In distributed systems, rate limiting requires shared state, typically implemented using Redis with atomic operations. When limits are exceeded, the server responds with HTTP 429 and retry metadata. In production, rate limiting is commonly applied at both the edge (NGINX/API gateways) and application level.

![[Pasted image 20251224231150.png]]
## Where rate limiting fits in the request life-cycle

Client
↓
Edge / Reverse Proxy   
↓
Rate Limiter  ← (often here)   
↓
Authentication   
↓
Authorization   
↓
Business Logic   
↓
Database / External Services

## What happens internally when a request arrives

1. Identify the client (IP / userId / API key)
    
2. Generate a **rate-limit key**
    
    `rate_limit:{client}:{endpoint}`
    
3. Check request count / token availability
    
4. Decide:
    
    - ✅ allow request
        
    - ❌ reject with `HTTP 429`


## HTTP behavior & headers

When blocked:

`HTTP/1.1 429 Too Many Requests 
`Retry-After: 30` 
`X-RateLimit-Limit: 100`
`X-RateLimit-Remaining: 0`

**Why headers matter**

- Client can back off gracefully
    
- Prevents retry storms


## Redis-based rate limiting (interview gold)

### Why Redis?

- In-memory → fast
    
- Atomic commands (`INCR`)
    
- TTL support (`EXPIRE`)
    
- Shared across instances
    

### Fixed window in Redis

`INCR key EXPIRE key 60`

### Token bucket in Redis

- Store:
    
    - token count
        
    - last refill timestamp
        
- Compute refill dynamically
    

**Important**  
Use **Lua scripts** to ensure atomicity.

## All Rate-Limiting Algorithms - **one clean comparison**

| Algorithm                            | Core idea                                      | Bursts allowed?    | Accuracy      | Cost     | Main problem             |
| ------------------------------------ | ---------------------------------------------- | ------------------ | ------------- | -------- | ------------------------ |
| [[Fixed Window Counter]]             | Count requests in fixed time blocks            | ❌ (boundary abuse) | Low           | Very low | Burst at window edges    |
| [[Sliding Window Log Algorithm]]     | Store every request timestamp                  | ❌ (strict)         | Perfect       | High     | Memory & CPU heavy       |
| [[Sliding Window Counter Algorithm]] | Weighted sum of current + previous window      | ⚠️ Limited         | High (approx) | Medium   | Approximation error      |
| [[Leaky Bucket]]                     | Queue + constant drain rate                    | ❌ No               | High          | Medium   | Too strict, adds latency |
| [[Token Bucket Algorithm]]           | Tokens refill over time, requests spend tokens | ✅ Yes              | High (avg)    | Medium   | Needs careful tuning     |


## Difference Between Rate Limiting and throtling

| Aspect                    | **Rate Limiting**                                                         | **Throttling**                                               |
| ------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------ |
| **Core purpose**          | Enforces a **hard cap** on how many requests are allowed in a time window | **Slows down** request processing when limits are approached |
| **When limit is crossed** | Requests are **rejected**                                                 | Requests are **delayed or queued**                           |
| **Typical response**      | `HTTP 429 Too Many Requests`                                              | Slower response time, usually **no rejection**               |
| **Behavior type**         | **Binary decision** (allow / deny)                                        | **Gradual control** (speed regulation)                       |
| **Use case**              | Prevent abuse, protect backend capacity                                   | Smooth traffic, avoid sudden spikes                          |
