# Cloud Computing Interview Questions

- What is cloud computing?
  - Cloud computing is the on-demand delivery of IT resources (compute, storage, networking, databases, etc.) over the internet with pay-as-you-go pricing. Instead of owning and maintaining physical hardware, you rent capacity from a cloud provider (AWS, GCP, Azure). The main service models are IaaS (infrastructure), PaaS (platform), and SaaS (software).

- What are the main cloud providers and how do they differ?
  - The three major public cloud providers are Amazon Web Services (AWS), Google Cloud Platform (GCP), and Microsoft Azure. AWS has the broadest service catalogue and largest market share. GCP is particularly strong in data analytics, machine learning, and Kubernetes (Google created Kubernetes based on its internal Borg system and open-sourced it). Azure integrates tightly with Microsoft's enterprise ecosystem (Active Directory, Office 365). All three offer equivalent core primitives (VMs, object storage, managed databases, serverless functions).

- What are regions and availability zones?
  - A **region** is a geographical area containing one or more data centres (e.g. `us-east-1`). An **availability zone (AZ)** is an isolated data centre (or cluster of data centres) within a region with independent power, cooling, and networking. Deploying resources across multiple AZs protects against a single-facility failure. Deploying across multiple regions protects against regional outages and reduces latency for globally distributed users.

- What is a CDN (Content Delivery Network)?
  - A CDN is a globally distributed network of edge servers that cache and serve static content (images, JS, CSS, videos) from the location closest to the end user. This reduces latency, offloads origin server traffic, and improves availability. Popular CDNs include Cloudflare, AWS CloudFront, and Akamai. Dynamic content can also be accelerated via CDN by routing requests through an optimised backbone.

- What is ingress in the context of Kubernetes / cloud networking?
  - **Ingress** is an API object in Kubernetes that manages external HTTP/HTTPS access to services inside a cluster. It acts as a reverse proxy / Layer 7 load balancer, routing traffic based on host names or URL paths to the appropriate backend service. An Ingress Controller (e.g. NGINX Ingress, AWS ALB Ingress, Traefik) implements the rules defined in the Ingress resource. Without an Ingress, you would need a separate LoadBalancer service (and its associated cloud load balancer cost) per service.

- What is a load balancer and what types exist?
  - A load balancer distributes incoming traffic across multiple backend instances to improve availability and scalability.
    - **Layer 4 (transport-layer)**: Routes based on IP and TCP/UDP port — fast, low overhead (e.g. AWS NLB).
    - **Layer 7 (application-layer)**: Routes based on HTTP attributes (URL, headers, cookies) — supports path-based routing, SSL termination, sticky sessions (e.g. AWS ALB, NGINX).
    - **DNS-based (global)**: Routes at the DNS level across regions (e.g. AWS Route 53, Cloudflare Load Balancing).

- What is an API Gateway?
  - An API Gateway is a managed entry point that sits in front of backend services and handles cross-cutting concerns: routing, authentication/authorisation, rate limiting, request transformation, logging, and analytics. It decouples clients from internal service topology and enforces consistent policies across APIs. Examples: AWS API Gateway, Kong, Apigee, Traefik. In a microservices architecture it also acts as a BFF (Backend for Frontend) layer.

- What are webhooks?
  - A webhook is a user-defined HTTP callback triggered by an event on a remote server. Instead of a client polling an API repeatedly ("is there new data?"), the server POSTs a notification payload to a pre-configured URL as soon as the event occurs. Webhooks are lightweight and real-time but require the receiving endpoint to be accessible to the sender (typically a public URL, though internal endpoints within the same network or VPN are also possible), idempotent (duplicate deliveries can happen), and able to respond quickly (otherwise use a queue to hand off work asynchronously).

- What is a message queue and why is it used?
  - A message queue is a durable, asynchronous communication channel where producers publish messages and consumers read them independently. This decouples services, smooths traffic spikes (buffering), and improves resilience (if a consumer is down, messages wait). Common patterns:
    - **Point-to-point (queue)**: Each message is consumed by exactly one consumer (e.g. AWS SQS, RabbitMQ queues).
    - **Publish/Subscribe (topic)**: Messages are broadcast to all subscribed consumers (e.g. AWS SNS, Redis Pub/Sub, RabbitMQ exchanges).
    - **Log-based streaming**: Messages are retained in an ordered log and consumers track their own offset, enabling replay (e.g. Apache Kafka, AWS Kinesis).

- What is Apache Kafka and when should it be used?
  - Kafka is a distributed, log-based message streaming platform. Producers append events to **topics** (divided into **partitions** for parallelism). Consumers track their position via **offsets**, allowing replay and independent consumption rates. Key strengths: very high throughput, persistent storage, event replay, ordered delivery per partition, and stream processing (via Kafka Streams or ksqlDB). Suited for event-driven architectures, audit logs, data pipelines, and real-time analytics. Not ideal for simple task queues where fine-grained message acknowledgement is needed.

- What are the main database types and when do you choose each?
  - **Relational (SQL)** – tabular, schema-enforced, ACID transactions, joins. Best for structured data with complex relationships and strong consistency (e.g. PostgreSQL, MySQL). 
  - **Document** – schema-flexible JSON/BSON documents, easy horizontal scaling. Good for content management, catalogues, user profiles (e.g. MongoDB, Firestore).
  - **Key-Value** – ultra-fast lookups by key, simple data model. Ideal for caching, session storage, leaderboards (e.g. Redis, DynamoDB, Memcached).
  - **Wide-Column** – rows with dynamic columns grouped into column families. Suited for write-heavy workloads and time-series-like data at massive scale (e.g. Apache Cassandra, HBase).
  - **Time-Series** – optimised for sequenced measurements indexed by timestamp. Used in monitoring, IoT, financial tick data (e.g. InfluxDB, TimescaleDB, Prometheus).
  - **Graph** – data modelled as nodes and edges, optimised for traversing relationships. Best for social networks, fraud detection, recommendation engines (e.g. Neo4j, Amazon Neptune).
  - **Search** – full-text and faceted search with relevance ranking. Used alongside primary databases (e.g. Elasticsearch, Apache Solr).
  - **NewSQL** – relational semantics with horizontal scaling. Useful when you need SQL + scale (e.g. CockroachDB, Google Spanner).

- What is the CAP theorem?
  - The CAP theorem states that a distributed system can guarantee at most **two** of the following three properties simultaneously:
    - **Consistency** – every read receives the most recent write.
    - **Availability** – every request receives a response (not necessarily the latest data).
    - **Partition Tolerance** – the system continues operating despite network partitions.
  - Because network partitions are unavoidable in practice, the real trade-off is between **CP** (consistent but may reject requests during a partition, e.g. HBase, Zookeeper) and **AP** (available but may return stale data, e.g. Cassandra, DynamoDB). Relational databases on a single node are CA but cannot be partition-tolerant at scale.

- What is caching and at what levels can it be applied?
  - Caching stores computed or fetched results temporarily to avoid redundant work. Levels:
    - **Client-side** – browser cache, service worker cache.
    - **CDN / Edge** – static assets cached at PoPs close to users.
    - **API / Application layer** – in-process cache (e.g. `caffeine`) or distributed cache (Redis, Memcached) for DB query results or computed responses.
    - **Database query cache** – some RDBMS cache result sets internally.
    - **Hardware** – CPU L1/L2/L3 caches, SSD page cache.
  - Key cache strategies: **cache-aside** (app manages cache), **write-through** (writes go to cache + DB together), **write-behind** (writes to cache, flushed to DB asynchronously), **read-through** (cache fetches from DB on miss transparently).
  - Important considerations: TTL (time-to-live), cache invalidation, and cache stampede prevention.

- What is microservices architecture and how does it compare to a monolith?
  - A **monolith** packages all functionality into a single deployable unit. Simple to develop and deploy initially, but becomes harder to scale, maintain, and independently deploy as it grows.
  - **Microservices** split the application into small, independently deployable services each owning its own data store and communicating over APIs or events. Benefits: independent scaling, independent deployment, technology flexibility, fault isolation. Trade-offs: distributed systems complexity (network failures, eventual consistency, distributed tracing, service discovery, increased operational overhead).
  - A practical middle ground is the **modular monolith** — well-structured internal modules that can be extracted into services when needed.

- What are containers and container orchestration?
  - A **container** packages an application with all its dependencies (code, runtime, libraries) into a portable, isolated unit using OS-level virtualisation. Docker is the most common container runtime. Containers are lighter than VMs (shared OS kernel), start faster, and are immutable and reproducible.
  - **Container orchestration** automates deployment, scaling, load balancing, self-healing, and rolling updates of containers across a cluster of machines. **Kubernetes (K8s)** is the industry-standard orchestrator. Key abstractions: Pod (one or more containers), Deployment (desired state + rolling updates), Service (stable network endpoint), ConfigMap/Secret (configuration).

- What is serverless computing?
  - Serverless (FaaS – Function as a Service) lets you run code without managing servers. You deploy individual functions that are triggered by events (HTTP request, queue message, schedule). The cloud provider handles provisioning, scaling (to zero and back), and billing (per invocation / execution time). Examples: AWS Lambda, Google Cloud Functions, Azure Functions. Benefits: no infrastructure management, automatic scaling, low cost for sporadic workloads. Drawbacks: cold starts, execution time limits, vendor lock-in, harder to debug/test locally.

- What is event-driven architecture?
  - In event-driven architecture (EDA) services communicate by producing and consuming **events** — immutable records of something that happened. This decouples producers from consumers (they don't need to know about each other). Key patterns:
    - **Event Notification** – a service emits a lightweight event signalling a state change; consumers react independently.
    - **Event-Carried State Transfer** – events carry enough data so consumers don't need to query the source.
    - **Event Sourcing** – the application state is derived entirely from an append-only log of events (enables full audit trail and time-travel queries).
    - **CQRS (Command Query Responsibility Segregation)** – separates the write model (commands) from the read model (queries), often used with Event Sourcing to maintain optimised read projections.

- What is Infrastructure as Code (IaC)?
  - IaC means managing and provisioning cloud infrastructure through declarative or imperative code files rather than manual console actions. Benefits: version control, repeatability, peer review, automated deployment. Tools:
    - **Terraform** – cloud-agnostic, declarative HCL, most widely used.
    - **AWS CloudFormation / CDK** – AWS-native; CDK lets you define infrastructure in general-purpose languages (TypeScript, Python).
    - **Pulumi** – general-purpose programming languages for infrastructure definition.
    - **Ansible** – procedural, agent-less; more focused on configuration management.

- What is horizontal vs vertical scaling?
  - **Vertical scaling (scale up)** – adding more CPU, RAM, or storage to an existing machine. Simple but has a hard ceiling and creates a single point of failure.
  - **Horizontal scaling (scale out)** – adding more instances behind a load balancer. More complex (requires stateless services or shared state) but theoretically unlimited and more resilient.
  - Most cloud-native architectures favour horizontal scaling combined with auto-scaling groups that adjust capacity automatically based on metrics (CPU, request rate, queue depth).

- What is a service mesh?
  - A service mesh is an infrastructure layer that handles service-to-service communication in a microservices environment. It is typically implemented via **sidecar proxies** (e.g. Envoy) injected alongside each service. Features: mutual TLS (mTLS), load balancing, circuit breaking, retries, distributed tracing, and traffic shaping — all without changing application code. Popular service meshes: Istio, Linkerd, AWS App Mesh. Adds operational complexity; most valuable in large microservices deployments.

- What is a circuit breaker pattern?
  - The circuit breaker pattern prevents cascading failures in distributed systems. When calls to a downstream service start failing beyond a threshold, the circuit "opens" and subsequent calls fail immediately (fast-fail) instead of waiting for a timeout. After a cooldown period, the circuit moves to "half-open" and allows a probe request; if successful, it closes again. This stops overloading a struggling service and allows the caller to degrade gracefully (serve cached data, return defaults). Implemented in libraries like Resilience4j, Hystrix, or via a service mesh.

- What is a reverse proxy?
  - A reverse proxy sits in front of one or more backend servers and forwards client requests to them. From the client's perspective it looks like a single endpoint. Functions: load balancing, SSL termination, caching, compression, request routing, and hiding internal service topology. NGINX and HAProxy are common software reverse proxies; cloud-managed equivalents include AWS ALB and Cloudflare.

- What is the difference between SQL and NoSQL databases?
  - **SQL** databases use a fixed, relational schema with tables, rows, and foreign-key relationships. They provide strong ACID guarantees and powerful query capabilities via joins and aggregations. Scaling horizontally is harder (sharding is complex).
  - **NoSQL** databases use flexible data models (document, key-value, column-family, graph). They are typically designed for horizontal scaling, high availability, and eventual consistency. They sacrifice some query power (no arbitrary joins) and consistency guarantees for scale and flexibility. The right choice depends on data shape, access patterns, and consistency requirements.

- How do you design for high availability and disaster recovery?
  - **High availability (HA)**: Eliminate single points of failure by deploying across multiple AZs, using load balancers, auto-scaling, and health checks. Target a defined SLA (e.g. 99.9% = ~8.7 hours downtime/year).
  - **Disaster Recovery (DR)**: Plan for region-wide failures. Key metrics: **RTO** (Recovery Time Objective – max acceptable downtime) and **RPO** (Recovery Point Objective – max acceptable data loss). Strategies range from backup/restore (cheapest, high RTO/RPO) to warm standby to active-active multi-region (most expensive, near-zero RTO/RPO).
  - Common techniques: database read replicas and cross-region replication, regular backups with tested restore procedures, chaos engineering to validate resilience.
