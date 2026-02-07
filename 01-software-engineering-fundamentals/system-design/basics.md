# System Design Basics

## Overview

System design interviews test your ability to design large-scale distributed systems. Focus on trade-offs and communication.

---

## Framework for System Design

### 1. Requirements (5 min)
- **Functional**: What should the system do?
- **Non-functional**: Scale, latency, availability

### 2. Estimation (5 min)
- Users, requests/second, storage needs

### 3. High-Level Design (10 min)
- Draw main components and data flow

### 4. Deep Dive (20 min)
- Detail critical components
- Discuss trade-offs

### 5. Wrap Up (5 min)
- Review, identify bottlenecks

---

## Key Concepts

### Scalability

| Type | Description | Example |
|------|-------------|---------|
| Vertical | Add more power (CPU, RAM) | Bigger server |
| Horizontal | Add more machines | More servers |

### Load Balancing

Distributes traffic across servers.

**Algorithms**:
- Round Robin
- Least Connections
- IP Hash
- Weighted

```
Client → Load Balancer → Server 1
                      → Server 2
                      → Server 3
```

### Caching

Store frequently accessed data in memory.

**Types**:
- Client-side (Browser)
- CDN
- Application (Redis, Memcached)
- Database query cache

**Strategies**:
| Strategy | Description |
|----------|-------------|
| Cache-aside | App checks cache, then DB |
| Write-through | Write to cache and DB |
| Write-behind | Write to cache, async to DB |

### Database

**SQL vs NoSQL**:

| SQL | NoSQL |
|-----|-------|
| Structured schema | Flexible schema |
| ACID transactions | BASE (eventual consistency) |
| Vertical scaling | Horizontal scaling |
| Complex queries | Simple queries |

**Scaling Databases**:
- **Replication**: Master-slave for read scaling
- **Sharding**: Split data across databases
- **Partitioning**: Divide tables

---

## CAP Theorem

You can only guarantee 2 of 3:

| Property | Meaning |
|----------|---------|
| **C**onsistency | All nodes see same data |
| **A**vailability | Every request gets response |
| **P**artition Tolerance | System works despite network failures |

- **CP**: MongoDB, HBase
- **AP**: Cassandra, DynamoDB
- **CA**: Traditional RDBMS (not distributed)

---

## Common Patterns

### Microservices

```
       API Gateway
      /     |     \
Service  Service  Service
   A        B        C
   |        |        |
  DB       DB       DB
```

**Pros**: Independent scaling, deployment, tech choice
**Cons**: Complexity, network latency, debugging

### Message Queues

Async communication between services.

```
Producer → Queue → Consumer
           (Kafka, RabbitMQ, SQS)
```

**Benefits**:
- Decoupling
- Load leveling
- Fault tolerance

### CDN (Content Delivery Network)

Serve static content from edge servers near users.

```
User → Nearest CDN Edge → Origin Server (if cache miss)
```

---

## Back-of-the-Envelope Estimation

### Latency Numbers

| Operation | Time |
|-----------|------|
| L1 cache reference | 0.5 ns |
| RAM reference | 100 ns |
| SSD read | 100 µs |
| HDD seek | 10 ms |
| Network round trip | 500 ms |

### Data Size

| Unit | Size |
|------|------|
| 1 KB | Short email |
| 1 MB | Image |
| 1 GB | Movie |
| 1 TB | Small dataset |
| 1 PB | Large company data |

### Quick Calculations

- 1 million requests/day ≈ 12 req/sec
- 1 billion requests/day ≈ 12,000 req/sec
- 1 GB / day = 12 KB/sec

---

## Common System Design Questions

### URL Shortener (Easy)
- Generate short code
- Store mapping (short → long)
- Redirect requests

### Twitter Feed (Medium)
- Fan-out on write vs read
- Cache hot users
- Timeline service

### Chat System (Medium)
- WebSockets for real-time
- Message queue for delivery
- Read receipts, presence

### Netflix (Hard)
- Video encoding & storage
- CDN for streaming
- Recommendation service

---

## Diagram Template

```
                        ┌──────────┐
                        │   CDN    │
                        └────┬─────┘
                             │
┌────────┐    ┌──────────────┴───────────────┐
│ Client │───→│        Load Balancer          │
└────────┘    └──────────────┬───────────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
        ┌─────┴─────┐  ┌─────┴─────┐  ┌─────┴─────┐
        │  Server   │  │  Server   │  │  Server   │
        └─────┬─────┘  └─────┬─────┘  └─────┬─────┘
              │              │              │
              └──────────────┼──────────────┘
                             │
              ┌──────────────┴──────────────┐
              │           Cache              │
              │         (Redis)              │
              └──────────────┬──────────────┘
                             │
              ┌──────────────┴──────────────┐
              │         Database             │
              │    (Master + Replicas)       │
              └─────────────────────────────┘
```

---

## Resources

- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [Designing Data-Intensive Applications](https://dataintensive.net/)
