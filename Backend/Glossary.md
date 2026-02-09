API - A backend API is a set of **well-defined endpoints, methods, request formats, and response formats** that allow external clients to **access, modify, and interact with server-side data and logic** in a controlled and secure way.

Endpoint - A specific **URL + HTTP method** that performs a defined backend operation.  
Example: `GET /api/users/42`

HTTP Methods:
- **GET** → Read data    
- **POST** → Create data
- **PUT** → Replace data
- **PATCH** → Partially update data
- **DELETE** → Remove data

Status Code - Numeric code indicating request result.
- `200` OK
- `201` Created
- `400` Bad Request
- `401` Unauthorized
- `403` Forbidden
- `404` Not Found
- `500` Internal Server Error

Middleware - A function that runs before or after a request reaches the main logic.
Used for auth, logging, validation, rate limiting.

Controller - Handles request → response flow.
Calls services, validates input, formats output.

Service Layer - Contains business logic independent of HTTP.
Keeps controllers thin.

ORM / ODM - Tool that maps **objects ↔ database records**.  
Examples: Prisma, Sequelize, Mongoose.

Authentication - Process of verifying who a user is (login).

Authorization - Process of deciding what a user is allowed to do.

JWT – A JSON Web Token is a compact, signed token used to securely transmit user identity and claims between client and server, enabling stateless authentication across requests.

Salting – Salting is the process of adding random data to a password before hashing it, ensuring that identical passwords produce different hashes and protecting against rainbow-table attacks.

Rate Limiting – Rate limiting restricts the number of requests a client can make to a backend within a given time window to prevent abuse, overload, or denial-of-service attacks.

Caching – Caching is the practice of storing frequently accessed data in fast storage (like memory) to reduce database load and improve response time.

Load Balancer – A load balancer distributes incoming client requests across multiple backend servers to improve availability, reliability, and performance.

Horizontal Scaling – Horizontal scaling increases system capacity by adding more server instances and distributing load across them.

Vertical Scaling – Vertical scaling increases system capacity by upgrading resources (CPU, RAM, storage) of an existing server.

WebSocket – WebSocket is a protocol that establishes a persistent, bidirectional connection between client and server for real-time data exchange.

CI/CD – Continuous Integration and Continuous Deployment is an automated pipeline that builds, tests, and deploys backend code changes quickly and reliably.

Idempotent – An operation is idempotent if performing it multiple times results in the same system state as performing it once.

ORM – An Object Relational Mapper that maps application objects to relational database tables, enabling database interaction using code instead of SQL.

Transaction – A sequence of database operations executed as a single unit that guarantees atomicity, consistency, isolation, and durability.

ACID – A set of properties (Atomicity, Consistency, Isolation, Durability) that ensure reliable database transactions.

Index – A database structure that improves query performance by enabling faster data lookup.

Query Optimization – The process of improving database queries to reduce execution time and resource usage.

N+1 Problem – A performance issue where one query triggers multiple additional queries, often due to poor ORM usage.

Deadlock – A situation where two or more transactions block each other indefinitely while waiting for resources.

Concurrency – The ability of a backend to handle multiple requests or operations at the same time.

Race Condition – A bug that occurs when the outcome of operations depends on the timing or order of execution.

Message Queue – An asynchronous communication mechanism that allows services to send and process messages independently.

Event-Driven Architecture – A design where services react to events instead of direct requests.

Retry Logic – A mechanism that retries failed operations to handle transient errors.

Circuit Breaker – A resilience pattern that stops requests to a failing service to prevent cascading failures.

Timeout – A predefined limit after which an operation is aborted if no response is received.

Health Check – An endpoint or mechanism used to verify that a backend service is running and responsive.

Graceful Shutdown – A process where a server stops accepting new requests and completes ongoing ones before shutting down.

Environment Variables – External configuration values used to manage secrets and environment-specific settings.

Secrets Management – Secure storage and access control for sensitive data like API keys and credentials.

API Versioning – Managing changes to APIs without breaking existing clients.

Pagination – Splitting large datasets into smaller, manageable response chunks.

Filtering – Limiting response data based on query parameters.

Serialization – Converting data structures into a format suitable for network transmission, such as JSON.

Deserialization – Converting transmitted data back into usable application objects.