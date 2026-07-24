graph TD
    Client((Client / Browser))

    subgraph "API & Compute Layer"
        LB[Load Balancer]
        API["FastAPI Instances<br/>(Stateless API)"]
        Worker["Background Task<br/>(Async Logging)"]
    end

    subgraph "Caching & Rate Limiting"
        Redis[("Redis<br/>(Cache & Counters)")]
    end

    subgraph "Persistent Storage"
        PG[("PostgreSQL<br/>(Source of Truth)")]
    end

    Client -->|1. Create URL<br/>2. Redirects<br/>3. Analytics| LB
    LB --> API

    API <-->|Token Bucket Check<br/>Cache Read/Write| Redis
    API <-->|Read/Write<br/>Relational Data| PG

    API -.->|Fire & Forget<br/>Click Event| Worker
    Worker -.->|Log Analytics| PG
    Redis -.->|Periodic Flush<br/>Hot Counters| PG

    %% Styling
    classDef client fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef compute fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef cache fill:#ffebee,stroke:#d32f2f,stroke-width:2px;
    classDef db fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;

    class Client client;
    class API,LB,Worker compute;
    class Redis cache;
    class PG db;
