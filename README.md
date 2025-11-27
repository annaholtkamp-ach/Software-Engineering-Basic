# Software-Engineering-Basic
Master Course Software Engineering Basic

```mermaid
flowchart LR

  %% Clients
  subgraph Clients
    partner[Partner systems / Voice agents]
  end

  %% Edge / offline mode
  subgraph Edge["On-device Lite Voice Agent"]
    edgeASR[Lite ASR & NLU<br/>(on-device)]
    edgeQueue[Local FIFO queue & encrypted cache]
    edgeASR --> edgeQueue
  end

  partner -->|HTTPS or gRPC<br/>(audio and metadata)| ingestionAPI
  edgeQueue -->|Sync on reconnect| ingestionAPI

  %% Core backend
  subgraph Backend
    ingestionAPI[Ingestion API<br/>(FastAPI)]
    mq[Message broker<br/>(RabbitMQ)]
    workers[Extraction workers<br/>(Celery)]
    voiceEngine[Voice AI Engine<br/>(ASR / Prosody / Timbre / Emotion)]
    governance[Voice Governance Service]
    rawStore[(Raw audio storage<br/>(S3))]
    metaDB[(Metadata DB<br/>(PostgreSQL))]
    featureStore[(Feature Store<br/>(Layer embeddings))]
  end

  ingestionAPI -->|auth and validate| mq
  ingestionAPI -->|store metadata| metaDB
  mq --> workers
  workers --> rawStore
  workers --> voiceEngine
  voiceEngine --> featureStore
  governance -->|consent and retention| metaDB

  %% Compute backends (GPU & INT)
  subgraph Compute["Compute backends"]
    gpu[GPU nodes]
    intNode[INT nodes]
  end

  voiceEngine --> gpu
  voiceEngine --> intNode

  %% Observability
  subgraph Observability
    otel[OpenTelemetry SDK / Collector]
    dash0[dash0 observability platform]
  end

  ingestionAPI --> otel
  workers --> otel
  voiceEngine --> otel
  governance --> otel
  otel --> dash0
```
