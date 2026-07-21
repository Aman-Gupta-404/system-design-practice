# System Design NFR & Scale Cheat Sheet (SDE-2 / Senior SWE)

## Core Thinking Framework

```text
Business Requirement
        ↓
User Behavior
        ↓
Traffic Pattern
        ↓
Scale Estimation
        ↓
Non-Functional Requirements
        ↓
Architecture
```

---

## 1. Product Categories

| Category      | Examples         | Typical Focus              |
| ------------- | ---------------- | -------------------------- |
| Messaging     | WhatsApp, Slack  | Low latency, durability    |
| Feed          | Instagram, X     | Read-heavy, caching        |
| Streaming     | YouTube, Netflix | CDN, bandwidth             |
| Payments      | UPI, Stripe      | Consistency, idempotency   |
| Booking       | BookMyShow       | Concurrency, consistency   |
| Collaboration | Google Docs      | Conflict resolution        |
| Storage       | Dropbox          | Durability, object storage |
| Ride Hailing  | Uber             | Realtime location          |

---

## 2. Ask These Questions

1.  What makes users angry?
2.  Read-heavy or write-heavy?
3.  What happens if data is stale?
4.  What happens if data is lost?
5.  Can the service go down?
6.  Expected latency?
7.  Data growth?
8.  Peak traffic?

---

## 3. Read vs Write

| Pattern         | Examples           | Design Direction          |
| --------------- | ------------------ | ------------------------- |
| Reads >> Writes | Instagram, YouTube | Cache, CDN, Read replicas |
| Reads ~= Writes | Chat               | WebSockets, queues        |
| Writes >> Reads | Metrics            | Streaming, batching       |

---

## 4. Latency Cheat Sheet

| Feature            |          Target |
| ------------------ | --------------: |
| Chat delivery      |         <100 ms |
| Search             |         <300 ms |
| Feed load          |         <500 ms |
| API response       | <200 ms (ideal) |
| Payment completion |           1–5 s |
| Analytics          |         Minutes |
| Video startup      |           2–5 s |

Use these as discussion anchors, not hard requirements.

---

## 5. Availability Targets

| SLA     | Downtime/year |
| ------- | ------------: |
| 99%     |    ~3.65 days |
| 99.9%   |    ~8.8 hours |
| 99.99%  |       ~53 min |
| 99.999% |        ~5 min |

---

## 6. Consistency Guide

| System        | Priority                         |
| ------------- | -------------------------------- |
| Payments      | Strong consistency               |
| Seat booking  | Strong consistency               |
| Chat          | Usually consistency for ordering |
| Feed          | Eventual consistency             |
| Likes         | Eventual consistency             |
| Notifications | Availability                     |

---

## 7. Durability

Never lose: - Payments - Orders - Messages - Uploaded files

Can often tolerate some loss: - Metrics - Logs - Analytics -
Notifications

---

## 8. Approximate Public Scale Numbers (2025/2026 order-of-magnitude)

These are approximate interview numbers.

| Product   |                 MAU |
| --------- | ------------------: |
| Instagram |                 ~2B |
| WhatsApp  |                 ~3B |
| YouTube   |               ~2.5B |
| Gmail     |                 ~2B |
| Netflix   |   ~300M subscribers |
| X         | ~600M monthly users |
| Uber      | ~170M monthly users |

Rule: memorize order of magnitude (100M, 1B, 3B), not exact values.

---

## 9. Traffic Estimation Formula

Assume: - DAU - Requests/user/day

    Total Requests/day = DAU × Requests per user

    Average RPS = Requests/day / 86400

    Peak RPS = Average × (5 to 10)

Example: DAU = 100M Requests/day/user = 20

Requests/day = 2B Average ≈ 23K RPS Peak ≈ 120K--230K RPS

---

## 10. Storage Estimation

    Storage/day =
    Users/day × Objects/user × Size/object

Example: 10M photos/day × 3 MB ≈ 30 TB/day

---

## 11. Typical NFRs

- Scalability
- Availability
- Consistency
- Durability
- Reliability
- Latency
- Throughput
- Elasticity
- Security
- Cost
- Observability

---

## 12. Mapping Questions to Architecture

| Question            | Leads to             |
| ------------------- | -------------------- |
| Many reads?         | Cache/CDN            |
| Many writes?        | Queue/stream         |
| Global users?       | Multi-region         |
| Huge files?         | Object storage       |
| Low latency?        | In-memory cache      |
| Strong consistency? | Transactions/locking |
| Spikes?             | Autoscaling          |

---

## 13. Common Systems

### WhatsApp

- Low latency
- Ordering
- Durability
- Availability

### Instagram

- Read-heavy
- CDN
- Cache
- Eventual consistency

### YouTube

- CDN
- Object storage
- Async processing
- Massive bandwidth

### BookMyShow

- Strong consistency
- Locking
- Idempotency

### Google Docs

- Conflict resolution
- OT/CRDT
- Low latency

---

## 14. Interview Flow

1.  Functional requirements
2.  Estimate scale
3.  Non-functional requirements
4.  APIs
5.  High-level design
6.  Database
7.  Deep dives
8.  Bottlenecks

---

## 15. Where to Learn Scale Numbers

Use engineering blogs and annual reports: - Meta Engineering - Google
Engineering - Uber Engineering - Netflix Tech Blog - Cloudflare Blog -
AWS Architecture Blog - ByteByteGo - High Scalability (archive)

Don't memorize exact values. Learn orders of magnitude.

---

## 16. Golden Rule

Derive, don't memorize.

User expectations → Traffic → Scale → NFRs → Architecture
