# Software-Engineering-Basic
Master Course Software Engineering Basic

```mermaid
flowchart LR
  classDef comp fill:#f9f9f9,stroke:#333,stroke-width:1px,rx:4,ry:4;

  %% Clients
  subgraph Clients
    partner[Partner systems / Voice agents]
  end
  class partner comp;

  %% Edge / offline mode
  subgraph Edge["On-device Lite Voice Agent"]
    edgeASR[Lite ASR + NLU\n(on-device)]
    edgeQueue[Local FIFO queue\n+ encrypted cache]
    edgeASR --> edgeQueue
  end
  class edgeASR,edgeQueue comp;

  partner -->|HTTPS / gRPC\n(audio + metadata)| ingestionAPI;
  edgeQueue -->|Sync on reconnect| ingestionAPI;

  %% Core backend
  subgraph Backend
    ingestionAPI[Ingestion API\n(FastAPI)]
    mq[Message broker\nRabbitMQ]
    workers[Extraction workers\nCelery]
    voiceEngine[Voice AI Engine\nASR / Prosody / Timbre / Emotion]
    governance[Voice Governance Service]
    rawStore[(Raw audio storage\nS3)]
    metaDB[(Metadata DB\n(PostgreSQL))]
    featureStore[(Feature Store\nLayer embeddings)]
  end
  class ingestionAPI,mq,workers,voiceEngine,governance,rawStore,metaDB,featureStore comp;

  ingestionAPI -->|auth & validate| mq;
  ingestionAPI -->|store metadata| metaDB;
  mq --> workers;
  workers --> rawStore;
  workers --> voiceEngine;
  voiceEngine --> featureStore;
  governance -->|consent + retention| metaDB;

  %% Compute backends (GPU + INT)
  subgraph Compute["Compute backends"]
    gpu[GPU nodes]
    int[INT nodes]
  end
  class gpu,int comp;

  voiceEngine --> gpu;
  voiceEngine --> int;

  %% Observability
  subgraph Observability
    otel[OpenTelemetry SDK / Collector]
    dash0[dash0 observability platform]
  end
  class otel,dash0 comp;

  ingestionAPI --> otel;
  workers --> otel;
  voiceEngine --> otel;
  governance --> otel;
  otel --> dash0;
```
