# Scalable URL Shortener – Spring Boot Microservices (FAANG-Ready)

This repository showcases a **production-grade, cloud-native URL Shortener system** designed using **Spring Boot microservices**. The system is architected to handle **millions of requests per day**, emphasizing **scalability, resilience, observability, and operational excellence**—the same dimensions evaluated in FAANG system design interviews.

---

## 1. System Overview

**Core Goals**

* Handle millions of redirect requests with low latency
* Horizontally scalable and stateless services
* Strong consistency for writes, eventual consistency for reads
* Fully observable and cloud-deployable

**High-Level Architecture**

* API Gateway → Backend Microservices
* Cache-first read path
* Async event-driven analytics
* Kubernetes-based deployment

---

## 2. Microservices Breakdown (Total: 7 Services)

### 1️⃣ API Gateway Service (`url-gateway-service`)

**Purpose**: Single entry point for all external traffic

**Responsibilities**

* Request routing
* Rate limiting & throttling
* Authentication delegation
* Circuit breaking

**Tech Stack / Dependencies**

* Spring Cloud Gateway
* Resilience4j
* Redis (rate limiting)
* Micrometer

---

### 2️⃣ URL Shortening Service – Write Path (`url-shortener-service`)

**Purpose**: Create short URLs and persist mappings

**Responsibilities**

* Short code generation (Base62 / Snowflake)
* Idempotent URL creation
* Validation & expiry handling

**Tech Stack / Dependencies**

* Spring Boot
* Spring Data JPA / R2DBC
* MySQL / PostgreSQL
* Flyway
* OpenAPI (Swagger)

---

### 3️⃣ URL Redirect Service – Read Path (`url-redirect-service`)

**Purpose**: Ultra-low latency redirect handling (hot path)

**Responsibilities**

* Cache-first lookups
* HTTP 301/302 redirects
* Fallback to DB on cache miss

**Tech Stack / Dependencies**

* Spring Boot WebFlux
* Redis Cluster
* Caffeine (L1 cache)
* Netty
* Resilience4j

---

### 4️⃣ Analytics Service (`url-analytics-service`)

**Purpose**: Track and aggregate URL access metrics

**Responsibilities**

* Async event ingestion
* Click aggregation
* Geo / device analytics

**Tech Stack / Dependencies**

* Spring Boot
* Kafka
* Spring Cloud Stream
* ClickHouse / Druid / BigQuery

---

### 5️⃣ Authentication & Authorization Service (`auth-service`)

**Purpose**: Secure access and user identity

**Responsibilities**

* OAuth2 / JWT authentication
* API key management
* Role-based access control

**Tech Stack / Dependencies**

* Spring Security
* Spring Authorization Server
* JWT
* Redis (token store)

---

### 6️⃣ Admin & Management Service (`url-admin-service`)

**Purpose**: Operational control and governance

**Responsibilities**

* Enable/disable URLs
* Expiry management
* Abuse detection
* Audit logging

**Tech Stack / Dependencies**

* Spring Boot
* Spring Security
* PostgreSQL

---

### 7️⃣ Notification / Event Consumer Service (`url-event-service`) *(Optional but Senior-Level)*

**Purpose**: Handle background workflows

**Responsibilities**

* URL expiry processing
* Alerting & notifications
* Cleanup jobs

**Tech Stack / Dependencies**

* Spring Boot
* Kafka Consumers
* Scheduler (Quartz)

---

## 3. Data & Infrastructure Components

### Databases

* **Primary DB**: MySQL / PostgreSQL (write-optimized)
* **Read Replicas** for scaling
* Sharding by `short_code`

### Cache

* Redis Cluster
* TTL-based eviction
* Write-through strategy

### Messaging

* Kafka Topics:

  * `url-created`
  * `url-accessed`
  * `url-expired`

---

## 4. Cross-Cutting Concerns

### Observability

* Micrometer + Prometheus
* Grafana dashboards
* ELK Stack (logs)
* OpenTelemetry + Jaeger

### Resilience

* Circuit breakers
* Bulkheads
* Retries & rate limiting
* Graceful shutdown

---

## 5. Cloud & Deployment

* Docker (multi-stage builds)
* Kubernetes (HPA, PDBs)
* AWS / GCP / Azure
* Managed Redis, Kafka, SQL

---

## 6. Load Testing & Benchmarking

**Tools**

* k6 / Gatling

**Test Scenarios**

* 10M+ redirect requests
* Cache hit vs miss analysis
* Redis / DB failure simulation

**Metrics Tracked**

* P99 latency
* Throughput (RPS)
* Error rate
* Cache hit ratio

---

## 7. Non-Functional Requirements

* Availability: 99.99%
* Redirect latency: <10ms (cached)
* Scalability: Horizontal
* Consistency: Strong writes, eventual reads

---

## 8. Why This Project Matters

This project demonstrates:

* Senior-level system design thinking
* Real-world scalability patterns
* Cloud-native engineering
* Production-readiness

It is intentionally designed to be **interview-ready for FAANG and top-tier product companies**.

---

## 9. Future Enhancements

* Bloom filters (cache penetration)
* CDN integration
* Multi-region active-active
* Chaos engineering
* Cost optimization strategies
