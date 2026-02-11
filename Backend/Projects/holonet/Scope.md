
![Image](https://media2.dev.to/dynamic/image/width%3D1600%2Cheight%3D900%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgnnd2c1i7ksf3ou2fu7q.png)


### What users can do

- Submit **public GitHub repo**
    
- Provide **build command** + **start command**
    
- Get a **live URL** on a subdomain
    

### What YOU will learn

✅ AWS EC2  
✅ Docker (build + run)  
✅ CI/CD concepts (build vs deploy separation)  
✅ Reverse proxy & networking  
✅ DNS & wildcard domains  
✅ Logs, failure modes, isolation

This is no longer “basic”.

---

## Infrastructure Stack (fixed, opinionated)

### Cloud

- **AWS EC2 (Ubuntu)**
    
    - Single instance
        
    - t3.small / t3.medium

Why EC2?

- You _see_ the OS
    
- You debug real Linux
    
- You understand networking instead of hiding behind services


---

### Networking

- **Elastic IP**
    
- Security Group:
    
    - 22 (SSH)
        
    - 80 (HTTP)
        
    - 443 (HTTPS)


This alone teaches:

- Public vs private IP
    
- Firewall thinking

---

## Domain & DNS (real-world skill)

### Domain setup

- Buy a domain (cheap `.xyz` or `.dev`)
    
- DNS provider (Route53 / Cloudflare)
    

### Critical part (wildcard DNS)

```
*.yourdomain.dev → EC2 Elastic IP
```

This is **not optional** — this is core infra learning.

You’ll understand:

- DNS resolution
    
- Why platforms need wildcard routing
    
- How one IP serves infinite apps

---

##  Reverse Proxy (where backend meets networking)

### Tool
### **Nginx**
##  Control Plane (your backend app)

### Tech

- Node.js + Express
    
- PostgreSQL (or SQLite for MVP)

### Responsibilities

- Accept deployment request
    
- Clone repo
    
- Trigger build
    
- Track status
    
- Assign subdomain + port

**Important:**  
Control plane does **not** serve user traffic.  
It only _orchestrates_.

This separation = real backend design.

---

## Build Plane (CI without calling it CI)

This is where you actually learn **CI/CD**.

### Flow

1. Control plane clones repo
    
2. Generates Dockerfile
    
3. Runs `docker build`
    
4. Streams logs
    
5. Tags image

### Why this matters

You’ll understand:

- Why builds are slow
    
- Why caching matters
    
- Why builds must be isolated

**Security lesson:**  
User code is untrusted → build in container, not host.

---

## Runtime Plane (Docker, for real)

### What runs

- One container per app
    
- Fixed memory + CPU limits
    
- Each app exposes one port
    

You will learn:

- Port binding
    
- Container lifecycle
    
- Crash handling
    
- Resource starvation

No Kubernetes yet — and that’s correct.

---

## CI/CD — without GitHub Actions magic

Instead of hiding learning inside GitHub Actions:

### MVP approach

- **You are the CI**
    
- Build happens on your server
    
- Deploy happens on your server

Later upgrade:

- GitHub webhook → auto redeploy


This teaches:

- CI ≠ GitHub Actions
    
- CD ≠ cloud magic
    
- Pipelines are just scripts + orchestration


---

## Data Model (small but realistic)

You’ll need tables like:

- Users (optional)
    
- Projects
    
- Deployments
    
- Routes (subdomain → port)
    
- Build logs
    

This forces you to think:

- State vs runtime
    
- Idempotency
    
- Failure recovery

