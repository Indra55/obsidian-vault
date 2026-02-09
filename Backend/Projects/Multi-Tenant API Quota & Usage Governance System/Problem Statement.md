# Project: Multi-Tenant API Quota & Usage Governance System

> A production-style system that enforces **plan-based API limits**, **monthly quotas**, and **real-time rate control** for SaaS products.

---

## Problem Statement (Why this exists)

Most SaaS APIs must:

- Protect infrastructure from abuse
    
- Enforce **fair usage**
    
- Differentiate users by **pricing plans**
    
- Track usage for **billing**
    

A simple rate limiter cannot solve this.

**This project builds a complete quota & enforcement layer** that sits in front of APIs and decides:

> _Can this request be served right now?_

---

## Core Responsibilities

The system must:

1. Identify the tenant (API key)
    
2. Enforce short-term rate limits (burst + sustained)
    
3. Enforce long-term monthly quotas
    
4. Apply plan-based rules
    
5. Support soft & hard enforcement
    
6. Work correctly under concurrency
    

---

## High-Level Architecture

![Image](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2022/06/22/multi1-1119x630.png)


### Request lifecycle

```
Client
 → API Gateway
   → API Key Auth
   → Rate Limiter (real-time)
   → Quota Engine (monthly)
   → Target Service
```

This mirrors **real SaaS infra**.

---

## System Components (Structured)

### A. API Gateway (Node.js)

**Role**

- Entry point for all requests
    
- Stateless
    
- Applies middleware chain
    

**Responsibilities**

- Extract API key
    
- Route request
    
- Attach usage metadata to response
    

---

### B. API Key & Tenant Resolution

Each request carries:

```
X-API-Key
```

Lookup result:

```
api_key → tenant_id → plan_id
```

This mapping is **cached** (Redis / in-memory) to avoid DB hits.

---

### C. Plan & Policy Engine

Plans define **rules**, not code.

Example:

```json
{
  "free": {
    "burst_rps": 5,
    "sustained_rpm": 60,
    "monthly_quota": 100000,
    "enforcement": "hard"
  },
  "pro": {
    "burst_rps": 20,
    "sustained_rpm": 600,
    "monthly_quota": 5000000,
    "enforcement": "soft"
  }
}
```

**Why this matters**

- Business logic is data-driven
    
- Plans can change without redeploying
    

---

## Enforcement Strategy (Important)

This is where your project stops being “basic”.

|Control Type|Algorithm|Storage|
|---|---|---|
|Burst control|Token Bucket|Redis|
|Sustained rate|Sliding Window|Redis|
|Monthly quota|Counter + TTL|Redis|
|Billing data|Aggregated logs|PostgreSQL|

Each request must pass **all checks**.

---

## Rate Limiting Layer (Hot Path)

### Token Bucket (burst)

- Allows short spikes
    
- Refills over time
    
- Prevents micro-bursts
    

Redis state:

```
bucket:{tenant}:{route}
→ tokens
→ last_refill_ts
```

Implemented via **Lua scripts** to ensure atomicity.

---

### Sliding Window (sustained rate)

- Accurate rate enforcement
    
- No boundary exploits
    

Redis state:

```
window:{tenant}:{route}
→ sorted set of timestamps
```

---

## Monthly Quota Engine

### Goal

Enforce **long-term usage limits** efficiently.

Redis key:

```
quota:{tenant}:{yyyy-mm}
```

- INCR per request
    
- TTL = seconds until next month
    
- No DB hits in hot path
    

### On overflow:

- Hard plan → block
    
- Soft plan → allow + mark overage
    

This mirrors **real billing systems**.

---

## Soft vs Hard Enforcement

### Hard Enforcement

```
Quota exceeded → HTTP 429
```

### Soft Enforcement

```
Quota exceeded →
  allow request
  tag as overage
  log for billing
```

This distinction alone makes the project **enterprise-grade**.

---

## Data Storage Design

### Redis (real-time state)

- Rate limiter buckets
    
- Sliding windows
    
- Monthly counters
    
- API key cache
    

### PostgreSQL (business data)

- Tenants
    
- API keys
    
- Plans
    
- Monthly usage summaries
    
- Overages
    

This separation shows **performance awareness**.

---

## Middleware Execution Order

```txt
1. Auth middleware
2. Plan resolver
3. Burst limiter
4. Sustained limiter
5. Monthly quota check
6. Forward request
```

Fail fast → cheapest checks first.

---

## Observability & Feedback

Each response includes:

```
X-RateLimit-Remaining
X-Quota-Remaining
X-Quota-Reset
```

This is **developer-friendly**, like Stripe/OpenAI.

---

## Project Folder Structure

```
/gateway
  /middleware
    auth.ts
    rateLimit.ts
    quota.ts

  /limiting
    tokenBucket.ts
    slidingWindow.ts
    lua/

  /quota
    monthlyCounter.ts

  /plans
    resolver.ts
    config.json

  /redis
    client.ts

  /db
    schema.sql
```

This structure alone communicates **systems maturity**.

---

## Failure & Edge Case Handling

- Redis failure → fail closed or degraded mode
    
- Clock drift → server-side timestamps only
    
- Concurrent requests → atomic Lua scripts
    
- New tenant → lazy key initialization
    

These are **interview-level considerations**.

---

## How This Scales

- Horizontal gateway scaling (stateless)
    
- Shared Redis cluster
    
- Independent service evolution
    
- Plan changes without code changes
    

This is **production-thinking**, not student code.

---

## Description

> Designed and implemented a **multi-tenant API quota and usage governance system** with plan-based enforcement, real-time rate limiting, and monthly quota tracking using Node.js, Redis, and PostgreSQL.

---

## Why this project works

You demonstrate:

- Distributed state handling
    
- Time-based algorithms
    
- SaaS billing logic
    
- Infrastructure design
    
- Clean abstractions


